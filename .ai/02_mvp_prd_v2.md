# Product Requirements Document (PRD) - ZeroClient MVP

## 1. Product Overview

ZeroClient is a web-based monitoring system designed to provide efficient visualization and management of data from multiple industrial installations. The system operates in a microfrontend architecture, allowing for a high degree of flexibility and scalability. ZeroClient addresses the critical need for reliable monitoring in challenging connection environments, particularly mobile and bandwidth-constrained scenarios.

The MVP will focus on delivering core functionality for monitoring ZeroSCADA backends, with auto-generated tabular views, basic caching capabilities, and a mobile-optimized interface. The system will support at least two backend installations (IBI Lab and URHydro) and provide four levels of user access.

The platform will operate in a cache-first mode with manual cache control, allowing for offline data access and optimized performance in poor connectivity conditions. Data polling will occur every second with configurable frequency to balance real-time updates with bandwidth constraints.

Primary deployment will target Mikrus 1.0 Pro environments (1 vCPU, 640 MB RAM, 10 GB SSD, Ubuntu Linux with Docker), ensuring economic efficiency while maintaining adequate performance for small installations.

## 2. User Problem

Industrial technicians, monitoring personnel, service staff, and administrators face significant challenges when attempting to monitor heterogeneous industrial installations, particularly in the following scenarios:

1. Mobile environments where laptop use is impractical
2. Areas with poor GSM coverage or unstable internet connections
3. Situations with bandwidth limitations or economic constraints (roaming, data packages)
4. Installations at varying stages of deployment readiness

These conditions create several critical problems:

- Difficulty accessing real-time data from installations
- Inability to maintain situational awareness during connectivity interruptions
- Challenges in managing multiple backend systems with different configurations
- Inefficiency when forced to use desktop-oriented interfaces on mobile devices
- Excessive data usage when monitoring systems in economically sensitive connection scenarios

ZeroClient addresses these problems by providing a cache-first, mobile-optimized monitoring solution that operates efficiently in challenging connectivity environments while allowing access to multiple heterogeneous backends through a single interface.

## 3. Functional Requirements

### 3.1 Authentication and Authorization

1. User registration and login system
2. Four-tier access level hierarchy:
   - Informational: Read-only access to collected data and visualizations
   - Monitoring: Additional access to detailed logs and problem information
   - Service: Additional ability to add devices and change access keys
   - Administrative: Full control, including user management
3. Initial user creation through administrative interface
4. Secure token-based authentication

### 3.2 Backend Integration

1. Support for subscription to at least two ZeroSCADA backends:
   - IBI Lab (internal/simulated)
   - URHydro (actual project)
2. Backend connection via URL API and API key
3. Persistence for object subscriptions
4. Basic object management:
   - Editing friendly names
   - Deleting subscriptions
5. Connection status indicator for each installation
6. Timestamp display for last successful backend contact

### 3.3 Data Visualization

1. Auto-generated tabular/text views for all connected devices
2. Two primary view types:
   - Current status with last measurement and backend contact time
   - Simple history chart (last 12 hours with averaging)
3. ISO date format for all timestamps
4. Mobile-optimized interface for one-handed operation
5. Embeddable widget version for external websites

### 3.4 Caching System

1. Basic "cache all" capability
2. On-demand element downloading:
   - Widget definitions
   - Page layouts
   - Device definitions
   - Measurement data
3. Manual cache control
4. Optimized operation in poor connectivity conditions
5. Minimal data transfer for monitoring operations

### 3.5 Data Collection

1. Configurable data polling frequency (default: 1 second)
2. Retry mechanisms for failed connection attempts
3. Error handling for unstable connections
4. Basic data transfer monitoring

## 4. Product Boundaries

The following features and capabilities are explicitly excluded from the MVP:

1. Report generation
2. Alarm management
3. Native mobile application (React Native)
4. Support for ThingsBoard and other backend types
5. Device connection type detection (WiFi/GSM)
6. Advanced cache management features
7. Self-service user management (password reset, etc.)
8. In-app support ticket system
9. Full API autodiscovery (schema-only in MVP)
10. Processing modules in the ZeroClient backend
11. Model Context Protocol (MCP) integration
12. LLM-based analysis
13. Multi-language support (English-only for MVP)

The MVP will concentrate on delivering core functionality with emphasis on reliability, mobile usability, and operation in challenging connectivity environments. Future versions will incorporate these additional features based on user feedback and business priorities.

## 5. User Stories

### US-001: User Registration and Login

As a user, I want to register an account and log in to access the ZeroClient system.

Acceptance Criteria:

- Users can log in with a username and password
- Invalid login attempts show appropriate error messages
- User sessions persist until explicitly logged out or expired after 24 hours
- Users cannot access system features without authentication

### US-002: User Access Level Assignment

As an administrator, I want to assign appropriate access levels to users so they have the right permissions for their role.

Acceptance Criteria:

- Administrators can assign one of four access levels to any user
- Access level changes take effect immediately
- The system prevents users from accessing features beyond their access level
- The interface adapts to show only permitted features based on access level

### US-003: Backend Subscription

As a service-level user, I want to subscribe to a ZeroSCADA backend using its URL and API key.

Acceptance Criteria:

- Users can add a new backend by entering its URL and API key
- The system validates the connection before confirming subscription
- Successfully added backends appear in the backend list immediately
- Error messages provide clear guidance when connection fails

### US-004: Backend Management

As a service-level user, I want to manage my subscribed backends by editing their friendly names or removing them.

Acceptance Criteria:

- Users can edit the friendly name of any subscribed backend
- Users can delete a backend subscription after confirmation
- Changes to backend names appear immediately throughout the interface
- Deleted backends are immediately removed from the list and all associated data is cleared

### US-005: Auto-generated Tabular View

As a monitoring user, I want the system to automatically generate tabular views of device data so I can quickly assess system status without custom screens.

Acceptance Criteria:

- The system presents device data in a readable table format without configuration
- Tables display all available device parameters with appropriate data types
- Table view updates with the configured polling frequency
- Tables are optimized for both desktop and mobile viewing

### US-006: Current Status View

As an informational user, I want to see the current status of a device including the last measurement and contact time.

Acceptance Criteria:

- The interface displays the most recent measurement for each parameter
- Each measurement shows a timestamp in ISO format
- Last contact time with the backend is clearly displayed
- The view is optimized for mobile screens with one-handed operation

### US-007: Historical Data View

As a monitoring user, I want to view historical measurement data in a chart format.

Acceptance Criteria:

- Charts display the last 12 hours of measurement data
- Data averaging is applied appropriately for the time range
- Users can identify trends and patterns in the data
- The chart interface is responsive and works on mobile devices

### US-008: Basic Cache Management

As a user, I want the system to automatically cache data so I can continue monitoring when my connection is unstable.

Acceptance Criteria:

- The system automatically caches essential data for offline access
- The interface clearly indicates when displaying cached data vs. live data
- The system automatically attempts to update cached data when connectivity returns
- Cached data remains available during connectivity interruptions

### US-009: Manual Cache Control

As a service-level user, I want to have basic manual control over the caching system.

Acceptance Criteria:

- Users can manually initiate caching of all elements
- Users can clear the cache when needed
- The interface shows what elements are currently cached
- Users can select specific elements to download for offline access

### US-010: Connection Status Indicator

As a user, I want to see the connection status for each backend installation.

Acceptance Criteria:

- The interface displays a clear status indicator for each backend
- Status updates when connectivity changes
- Last successful connection time is displayed
- Appropriate status messages help diagnose connection issues

### US-011: Mobile-Optimized Interface

As a field technician, I want to use the system on my smartphone with one hand.

Acceptance Criteria:

- All key functions are accessible with thumb-based navigation
- Interface elements are properly sized for touch interaction
- Forms and controls work correctly on small screens
- Critical information remains visible without scrolling on standard smartphone screens

### US-012: Configurable Data Polling

As a monitoring user, I want to adjust the data polling frequency to balance real-time updates with bandwidth usage.

Acceptance Criteria:

- Users can select from predefined polling frequency options
- Users can configure separate refresh rates for different views
- Changes to polling frequency take effect immediately
- The system respects the configured frequency without drift
- Current polling configuration is clearly indicated in the interface

### US-013: Widget Embedding

As a service user, I want to generate embeddable widgets to display device data on external websites.

Acceptance Criteria:

- The system provides embed codes for current status widgets
- Embedded widgets display accurate, auto-updating data
- Widgets adapt to their container size on the target website
- Embedded widgets respect public/private data permissions

### US-014: Error Handling for Unstable Connections

As a user in a poor connectivity area, I want the system to gracefully handle connection failures.

Acceptance Criteria:

- The system implements retry mechanisms with appropriate backoff
- Users are notified of connection problems without disruptive alerts
- The interface remains usable when connection is lost
- The system automatically recovers when connection is restored

### US-015: Secure Authentication

As a user, I want my authentication to be secure to protect access to potentially sensitive industrial data.

Acceptance Criteria:

- The system uses secure token-based authentication
- Passwords are appropriately hashed and secured
- Sessions expire after inactivity or explicit logout
- Failed login attempts are limited to prevent brute force attacks

### US-016: Field Work Preparation

As a technician, I want to deliberately prepare data for field work in areas with poor or no connectivity.

Acceptance Criteria:

- Users can select specific installations for comprehensive caching
- Users can manually trigger cache operations for selected installations
- The system provides download progress indicators during caching
- Cache completeness confirmation is provided before going offline
- The interface shows the total size of downloaded data
- Users can force a cache refresh to ensure latest data before field work

## 6. Success Metrics

The success of the ZeroClient MVP will be measured using the following metrics:

### 6.1 Technical Performance Metrics

1. Successful data subscription and retrieval from both target backends (IBI Lab and URHydro)
2. Consistent operation in GSM signal conditions down to -100 dBm
3. Cache effectiveness: Ability to view cached data after 30+ minutes offline
4. Data efficiency: <1MB data usage per hour of active monitoring
5. Load time: <3 seconds for initial load on 3G connection
6. Frontend performance: <100ms response time for UI interactions
7. Polling reliability: >99% success rate for data polls under normal conditions

### 6.2 User Experience Metrics

1. Ability to operate primary functions one-handed on a smartphone
2. Widget effectiveness: Successful embedding and data display on WordPress and standard HTML pages
3. Mobile usability: Completion of key tasks in <30 seconds on a smartphone
4. Offline usability: Ability to review cached data without connection errors or UI disruption

### 6.3 Business Success Metrics

1. Successful deployment on target Mikrus 1.0 Pro environment
2. Field technician adoption: Used successfully during URHydro station installation/service
3. Widget deployment: Successfully embedded on at least one production website
4. Backend diversity: Demonstrated connectivity to both target backend systems

---
Created on 2025-05-03 by Grzegorz Zieliński with assistance from Sohee (Claude 3.7 Sonnet Thinking).
