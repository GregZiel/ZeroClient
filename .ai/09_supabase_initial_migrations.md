-- supabase/migrations/20250513152000_zeroclient_initial_schema.sql

-- -----------------------------------------------------------------------------
-- ZeroClient Initial Database Schema Migration
-- 
-- This migration creates the initial database schema for the ZeroClient MVP.
-- It includes tables for user profiles, backend connections, widget configurations,
-- and various logging and metrics tables.
--
-- Tables created:
-- - profiles: Extended user profile information
-- - user_preferences: User-specific settings
-- - backend_connections: Industrial backend system connections
-- - connection_status_history: Historical connection status tracking
-- - widget_configurations: Embeddable widget settings
-- - widget_access_logs: Widget usage tracking
-- - activity_logs: Comprehensive user activity tracking
-- - performance_metrics: System performance measurements
-- -----------------------------------------------------------------------------

-- -----------------------------------------------------------------------------
-- profiles table
-- Extends Supabase auth.users with additional profile information
-- -----------------------------------------------------------------------------
create table profiles (
    id uuid primary key references auth.users(id),
    first_name varchar(100) not null,
    last_name varchar(100) not null,
    company varchar(200),
    phone varchar(50),
    timezone varchar(50) default 'UTC',
    preferred_language varchar(10) default 'en',
    created_at timestamp default now(),
    updated_at timestamp default now()
);

-- enable row level security
alter table profiles enable row level security;

-- create index for faster queries on created_at
create index index_profiles_on_created_at on profiles(created_at);

-- users can only view and edit their own profile
create policy profiles_owner_policy on profiles
    using (id = auth.uid())
    with check (id = auth.uid());

-- admins can view all profiles
create policy profiles_admin_select on profiles
    for select
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

-- trigger to update the updated_at timestamp
create or replace function update_updated_at_column()
returns trigger as $$
begin
    new.updated_at = now();
    return new;
end;
$$ language plpgsql;

create trigger update_profiles_updated_at
before update on profiles
for each row
execute function update_updated_at_column();

-- -----------------------------------------------------------------------------
-- user_preferences table
-- Stores user-specific application preferences
-- -----------------------------------------------------------------------------
create table user_preferences (
    id uuid primary key default gen_random_uuid(),
    user_id uuid not null references profiles(id) on delete cascade,
    notification_enabled boolean default true,
    polling_frequency integer default 1,
    settings jsonb,
    created_at timestamp default now(),
    updated_at timestamp default now()
);

-- enable row level security
alter table user_preferences enable row level security;

-- users can only view and edit their own preferences
create policy user_preferences_owner_policy on user_preferences
    using (user_id = auth.uid())
    with check (user_id = auth.uid());

-- admins can view all preferences
create policy user_preferences_admin_select on user_preferences
    for select
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

create trigger update_user_preferences_updated_at
before update on user_preferences
for each row
execute function update_updated_at_column();

-- -----------------------------------------------------------------------------
-- backend_connections table
-- Stores information about industrial backend systems
-- -----------------------------------------------------------------------------
create table backend_connections (
    id uuid primary key default gen_random_uuid(),
    user_id uuid not null references profiles(id) on delete cascade,
    name varchar(200) not null,
    url varchar(500) not null,
    api_key varchar(500) not null, -- should be encrypted in application layer
    current_status varchar(50) default 'unknown',
    last_contact_at timestamp,
    last_accessed_at timestamp,
    metadata jsonb,
    created_at timestamp default now(),
    updated_at timestamp default now()
);

-- enable row level security
alter table backend_connections enable row level security;

-- create indexes for frequently queried fields
create index index_backend_connections_on_user_id on backend_connections(user_id);
create index index_backend_connections_on_current_status on backend_connections(current_status);
create index index_backend_connections_on_last_contact_at on backend_connections(last_contact_at);

-- users can only view and manage their own connections
create policy connections_owner_policy on backend_connections
    using (user_id = auth.uid())
    with check (user_id = auth.uid());

-- admins can view all connections
create policy connections_admin_select on backend_connections
    for select
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

create trigger update_backend_connections_updated_at
before update on backend_connections
for each row
execute function update_updated_at_column();

-- -----------------------------------------------------------------------------
-- connection_status_history table
-- Tracks historical status changes for diagnostic purposes
-- -----------------------------------------------------------------------------
create table connection_status_history (
    id uuid primary key default gen_random_uuid(),
    connection_id uuid not null references backend_connections(id) on delete cascade,
    status varchar(50) not null,
    recorded_at timestamp default now() not null,
    details jsonb
);

-- enable row level security
alter table connection_status_history enable row level security;

-- create indexes for frequently queried fields
create index index_connection_status_history_on_connection_id on connection_status_history(connection_id);
create index index_connection_status_history_on_recorded_at on connection_status_history(recorded_at);

-- users can view history for their own connections
create policy status_history_connection_owner_select on connection_status_history
    for select
    using (exists (
        select 1 from backend_connections
        where backend_connections.id = connection_status_history.connection_id
        and backend_connections.user_id = auth.uid()
    ));

-- admins can view all history
create policy status_history_admin_select on connection_status_history
    for select
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

-- -----------------------------------------------------------------------------
-- widget_configurations table
-- Stores settings for embeddable widgets
-- -----------------------------------------------------------------------------
create table widget_configurations (
    id uuid primary key default gen_random_uuid(),
    connection_id uuid not null references backend_connections(id) on delete cascade,
    name varchar(200) not null,
    allowed_domains text[],
    license_url varchar(500),
    configuration jsonb not null,
    created_at timestamp default now(),
    updated_at timestamp default now(),
    created_by uuid not null references profiles(id)
);

-- enable row level security
alter table widget_configurations enable row level security;

-- create indexes for frequently queried fields
create index index_widget_configurations_on_connection_id on widget_configurations(connection_id);
create index index_widget_configurations_on_created_by on widget_configurations(created_by);

-- users can only manage widgets they created
create policy widgets_creator_policy on widget_configurations
    using (created_by = auth.uid())
    with check (created_by = auth.uid());

-- users can view widgets for connections they own
create policy widgets_connection_owner_select on widget_configurations
    for select
    using (exists (
        select 1 from backend_connections
        where backend_connections.id = widget_configurations.connection_id
        and backend_connections.user_id = auth.uid()
    ));

-- admins can view and manage all widgets
create policy widgets_admin_policy on widget_configurations
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

create trigger update_widget_configurations_updated_at
before update on widget_configurations
for each row
execute function update_updated_at_column();

-- -----------------------------------------------------------------------------
-- widget_access_logs table
-- Tracks widget usage for analytics
-- -----------------------------------------------------------------------------
create table widget_access_logs (
    id uuid primary key default gen_random_uuid(),
    widget_id uuid not null references widget_configurations(id) on delete cascade,
    accessed_at timestamp default now() not null,
    domain varchar(255),
    ip_address varchar(50),
    user_agent text,
    success boolean not null
);

-- enable row level security
alter table widget_access_logs enable row level security;

-- create indexes for frequently queried fields
create index index_widget_access_logs_on_widget_id on widget_access_logs(widget_id);
create index index_widget_access_logs_on_accessed_at on widget_access_logs(accessed_at);

-- users can view logs for widgets they created
create policy widget_logs_creator_select on widget_access_logs
    for select
    using (exists (
        select 1 from widget_configurations
        where widget_configurations.id = widget_access_logs.widget_id
        and widget_configurations.created_by = auth.uid()
    ));

-- users can view logs for widgets connected to their backends
create policy widget_logs_connection_owner_select on widget_access_logs
    for select
    using (exists (
        select 1 from widget_configurations
        join backend_connections on backend_connections.id = widget_configurations.connection_id
        where widget_configurations.id = widget_access_logs.widget_id
        and backend_connections.user_id = auth.uid()
    ));

-- admins can view all logs
create policy widget_logs_admin_select on widget_access_logs
    for select
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

-- -----------------------------------------------------------------------------
-- activity_logs table
-- Comprehensive tracking of user activities
-- -----------------------------------------------------------------------------
create table activity_logs (
    id uuid primary key default gen_random_uuid(),
    user_id uuid references profiles(id) on delete set null,
    action varchar(100) not null,
    entity_type varchar(100),
    entity_id uuid,
    timestamp timestamp default now() not null,
    ip_address varchar(50),
    user_agent text,
    device_info jsonb,
    details jsonb
);

-- enable row level security
alter table activity_logs enable row level security;

-- create indexes for frequently queried fields
create index index_activity_logs_on_user_id on activity_logs(user_id);
create index index_activity_logs_on_timestamp on activity_logs(timestamp);
create index index_activity_logs_on_action on activity_logs(action);
create index index_activity_logs_on_entity_type_and_entity_id on activity_logs(entity_type, entity_id);

-- users can view their own activity logs
create policy activity_logs_owner_select on activity_logs
    for select
    using (user_id = auth.uid());

-- admins can view all activity logs
create policy activity_logs_admin_select on activity_logs
    for select
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

-- system can insert logs for any user (no with check clause)
create policy activity_logs_insert on activity_logs
    for insert
    with check (true);

-- -----------------------------------------------------------------------------
-- performance_metrics table
-- Stores system performance measurements
-- -----------------------------------------------------------------------------
create table performance_metrics (
    id uuid primary key default gen_random_uuid(),
    connection_id uuid references backend_connections(id) on delete cascade,
    metric_type varchar(100) not null,
    value float not null,
    recorded_at timestamp default now() not null,
    details jsonb
);

-- enable row level security
alter table performance_metrics enable row level security;

-- create indexes for frequently queried fields
create index index_performance_metrics_on_connection_id on performance_metrics(connection_id);
create index index_performance_metrics_on_metric_type on performance_metrics(metric_type);
create index index_performance_metrics_on_recorded_at on performance_metrics(recorded_at);

-- users can view metrics for their own connections
create policy metrics_connection_owner_select on performance_metrics
    for select
    using (
        connection_id is null or
        exists (
            select 1 from backend_connections
            where backend_connections.id = performance_metrics.connection_id
            and backend_connections.user_id = auth.uid()
        )
    );

-- admins can view all metrics
create policy metrics_admin_select on performance_metrics
    for select
    using (exists (
        select 1 from auth.users
        where auth.users.id = auth.uid() and auth.users.role = 'admin'
    ));

-- system can insert metrics (no with check clause)
create policy metrics_insert on performance_metrics
    for insert
    with check (true);

-- -----------------------------------------------------------------------------
-- Create trigger function to record connection status changes
-- -----------------------------------------------------------------------------
create or replace function record_connection_status_change()
returns trigger as $$
begin
    if old.current_status is distinct from new.current_status then
        insert into connection_status_history (connection_id, status, details)
        values (new.id, new.current_status, jsonb_build_object('previous_status', old.current_status));
    end if;
    return new;
end;
$$ language plpgsql;

-- Create trigger to record status changes
create trigger record_backend_connection_status_change
after update of current_status on backend_connections
for each row
execute function record_connection_status_change();

-- -----------------------------------------------------------------------------
-- Create function to log authentication attempts
-- -----------------------------------------------------------------------------
create or replace function log_auth_event()
returns trigger as $$
declare
    log_action varchar;
    log_details jsonb;
begin
    -- Determine action based on the event
    if tg_op = 'INSERT' then
        log_action := 'user_signup';
    elsif tg_op = 'UPDATE' then
        if old.email is distinct from new.email then
            log_action := 'email_change';
            log_details := jsonb_build_object('old_email', old.email, 'new_email', new.email);
        else
            log_action := 'user_update';
        end if;
    end if;
    
    -- Log the event
    insert into activity_logs (
        user_id,
        action,
        entity_type,
        entity_id,
        details
    ) values (
        new.id,
        log_action,
        'auth.users',
        new.id,
        log_details
    );
    
    return new;
end;
$$ language plpgsql;

-- Create trigger for auth events
create trigger log_auth_events
after insert or update on auth.users
for each row
execute function log_auth_event();
