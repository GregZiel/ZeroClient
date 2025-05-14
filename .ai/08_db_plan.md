# ZeroClient Database Schema

## 1. Tables

### auth.users (Provided by Supabase)
Leveraging Supabase's built-in authentication system.

### profiles
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK, references auth.users(id) | User ID from Supabase auth |
| first_name | varchar(100) | not null | User's first name |
| last_name | varchar(100) | not null | User's last name |
| company | varchar(200) | | User's company name |
| phone | varchar(50) | | User's phone number |
| timezone | varchar(50) | default 'UTC' | User's preferred timezone |
| preferred_language | varchar(10) | default 'en' | User's preferred language code |
| created_at | timestamp | default now() | Record creation timestamp |
| updated_at | timestamp | default now() | Record update timestamp |

### user_preferences
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK | Unique identifier |
| user_id | uuid | FK references profiles(id), not null | User ID |
| notification_enabled | boolean | default true | Whether notifications are enabled |
| polling_frequency | integer | default 1 | Default polling frequency in seconds |
| settings | jsonb | | Additional user settings in JSON format |
| created_at | timestamp | default now() | Record creation timestamp |
| updated_at | timestamp | default now() | Record update timestamp |

### backend_connections
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK | Unique identifier |
| user_id | uuid | FK references profiles(id), not null | Owner user ID |
| name | varchar(200) | not null | Friendly name for the connection |
| url | varchar(500) | not null | Backend URL |
| api_key | varchar(500) | not null | Encrypted API key |
| current_status | varchar(50) | default 'unknown' | Current connection status |
| last_contact_at | timestamp | | Last successful contact with backend |
| last_accessed_at | timestamp | | Last time a user accessed this connection |
| metadata | jsonb | | Additional connection metadata |
| created_at | timestamp | default now() | Record creation timestamp |
| updated_at | timestamp | default now() | Record update timestamp |

### connection_status_history
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK | Unique identifier |
| connection_id | uuid | FK references backend_connections(id), not null | Backend connection ID |
| status | varchar(50) | not null | Connection status |
| recorded_at | timestamp | default now(), not null | When status was recorded |
| details | jsonb | | Additional status details or error information |

### widget_configurations
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK | Unique identifier |
| connection_id | uuid | FK references backend_connections(id), not null | Associated backend connection |
| name | varchar(200) | not null | Widget name |
| allowed_domains | text[] | | Array of domains allowed for CORS |
| license_url | varchar(500) | | Licensing URL if applicable |
| configuration | jsonb | not null | Widget configuration details |
| created_at | timestamp | default now() | Record creation timestamp |
| updated_at | timestamp | default now() | Record update timestamp |
| created_by | uuid | FK references profiles(id), not null | User who created the widget |

### widget_access_logs
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK | Unique identifier |
| widget_id | uuid | FK references widget_configurations(id), not null | Widget ID |
| accessed_at | timestamp | default now(), not null | Access timestamp |
| domain | varchar(255) | | Domain that accessed the widget |
| ip_address | varchar(50) | | IP address of the requester |
| user_agent | text | | User agent string |
| success | boolean | not null | Whether access was successful |

### activity_logs
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK | Unique identifier |
| user_id | uuid | FK references profiles(id) | User ID (null for anonymous) |
| action | varchar(100) | not null | Action performed |
| entity_type | varchar(100) | | Type of entity acted upon |
| entity_id | uuid | | ID of entity acted upon |
| timestamp | timestamp | default now(), not null | When action occurred |
| ip_address | varchar(50) | | IP address of the user |
| user_agent | text | | User agent string |
| device_info | jsonb | | Additional device information |
| details | jsonb | | Additional action details |

### performance_metrics
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | uuid | PK | Unique identifier |
| connection_id | uuid | FK references backend_connections(id) | Backend connection if applicable |
| metric_type | varchar(100) | not null | Type of metric (e.g., 'response_time', 'cache_miss') |
| value | float | not null | Metric value |
| recorded_at | timestamp | default now(), not null | When metric was recorded |
| details | jsonb | | Additional metric details |

## 2. Relationships

1. **profiles** (1) → (1) **auth.users** (One-to-one: Each auth user has one profile)
2. **profiles** (1) → (0..N) **user_preferences** (One-to-many: A user can have multiple preference sets)
3. **profiles** (1) → (0..N) **backend_connections** (One-to-many: A user can have multiple backend connections)
4. **backend_connections** (1) → (0..N) **connection_status_history** (One-to-many: A connection has multiple status history entries)
5. **backend_connections** (1) → (0..N) **widget_configurations** (One-to-many: A connection can have multiple widgets)
6. **profiles** (1) → (0..N) **widget_configurations** (One-to-many: A user creates multiple widgets)
7. **widget_configurations** (1) → (0..N) **widget_access_logs** (One-to-many: A widget has multiple access logs)
8. **profiles** (1) → (0..N) **activity_logs** (One-to-many: A user has multiple activity logs)
9. **backend_connections** (1) → (0..N) **performance_metrics** (One-to-many: A connection has multiple performance metrics)

## 3. Indexes

1. **profiles**:
   - index_profiles_on_created_at (created_at)

2. **backend_connections**:
   - index_backend_connections_on_user_id (user_id)
   - index_backend_connections_on_current_status (current_status)
   - index_backend_connections_on_last_contact_at (last_contact_at)

3. **connection_status_history**:
   - index_connection_status_history_on_connection_id (connection_id)
   - index_connection_status_history_on_recorded_at (recorded_at)

4. **widget_configurations**:
   - index_widget_configurations_on_connection_id (connection_id)
   - index_widget_configurations_on_created_by (created_by)

5. **widget_access_logs**:
   - index_widget_access_logs_on_widget_id (widget_id)
   - index_widget_access_logs_on_accessed_at (accessed_at)

6. **activity_logs**:
   - index_activity_logs_on_user_id (user_id)
   - index_activity_logs_on_timestamp (timestamp)
   - index_activity_logs_on_action (action)
   - index_activity_logs_on_entity_type_and_entity_id (entity_type, entity_id)

7. **performance_metrics**:
   - index_performance_metrics_on_connection_id (connection_id)
   - index_performance_metrics_on_metric_type (metric_type)
   - index_performance_metrics_on_recorded_at (recorded_at)

## 4. Row Level Security Policies

### profiles
```sql
-- Enable RLS
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;

-- Users can only view and edit their own profile
CREATE POLICY profiles_owner_policy ON profiles
  USING (id = auth.uid())
  WITH CHECK (id = auth.uid());

-- Admins can view all profiles
CREATE POLICY profiles_admin_select ON profiles
  FOR SELECT
  USING (EXISTS (
    SELECT 1 FROM auth.users
    WHERE auth.users.id = auth.uid() AND auth.users.role = 'admin'
  ));
```

### backend_connections
```sql
-- Enable RLS
ALTER TABLE backend_connections ENABLE ROW LEVEL SECURITY;

-- Users can only view and manage their own connections
CREATE POLICY connections_owner_policy ON backend_connections
  USING (user_id = auth.uid())
  WITH CHECK (user_id = auth.uid());

-- Admins can view all connections
CREATE POLICY connections_admin_select ON backend_connections
  FOR SELECT
  USING (EXISTS (
    SELECT 1 FROM auth.users
    WHERE auth.users.id = auth.uid() AND auth.users.role = 'admin'
  ));
```

### widget_configurations
```sql
-- Enable RLS
ALTER TABLE widget_configurations ENABLE ROW LEVEL SECURITY;

-- Users can only manage widgets they created
CREATE POLICY widgets_creator_policy ON widget_configurations
  USING (created_by = auth.uid())
  WITH CHECK (created_by = auth.uid());

-- Users can view widgets for connections they own
CREATE POLICY widgets_connection_owner_select ON widget_configurations
  FOR SELECT
  USING (EXISTS (
    SELECT 1 FROM backend_connections
    WHERE backend_connections.id = widget_configurations.connection_id
    AND backend_connections.user_id = auth.uid()
  ));

-- Admins can view and manage all widgets
CREATE POLICY widgets_admin_policy ON widget_configurations
  USING (EXISTS (
    SELECT 1 FROM auth.users
    WHERE auth.users.id = auth.uid() AND auth.users.role = 'admin'
  ));
```

## 5. Design Notes

1. **Simplified Authentication**: Leveraging Supabase's built-in authentication system with a simple admin/regular user role distinction for the MVP.

2. **Encryption for Sensitive Data**: API keys should be encrypted before storage. Supabase provides column encryption capabilities that should be utilized.

3. **JSONB for Flexibility**: Using JSONB columns for configuration and metadata allows for extensibility without schema changes, which is valuable for a rapid MVP development.

4. **Comprehensive Logging**: The activity_logs table captures all user interactions in a single table with standardized fields, simplifying implementation while providing valuable data.

5. **Historical Data**: Connection status history is stored separately for diagnostic purposes, allowing for analysis of connectivity patterns.

6. **Performance Considerations**: Indexes are added to frequently queried fields, particularly timestamps and foreign keys, to maintain performance as data grows.

7. **Row Level Security**: Implementing RLS policies directly in PostgreSQL provides robust security without requiring application-level access control for every operation.

8. **Extensibility**: The schema is designed to be extensible for future requirements while maintaining simplicity for the ultra-rapid MVP timeline.

By Alice @ Claude 3.7 Sonnet, 20250514