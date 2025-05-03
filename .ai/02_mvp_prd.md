# Product Requirements Document (PRD) - ZeroClient MVP

## 1. Product Overview

ZeroClient is a web application for monitoring industrial installations, designed to operate in challenging network conditions. The system enables effective monitoring of multiple heterogeneous installations through automatically generated tabular views and basic historical charts.

Key features:

- Microfrontend-based architecture
- Support for minimum two backends (IBI Lab, URHydro)
- Data caching system
- Responsive mobile interface
- Embeddable widgets for external websites
- Deployment on Mikrus 1.0 Pro platform

## 2. User Problem

Users need reliable access to industrial installation data under conditions of:

- Unstable internet connectivity
- Limited bandwidth
- Mobile access
- Diverse data sources
- Economic transmission constraints

The system addresses these problems through:

- Data caching
- Automatically generated views
- Efficient transmission management
- Offline support
- Mobile-friendly interface

## 3. Functional Requirements

1. Authentication and Authorization:

   - 4 access levels (Informational, Monitoring, Service, Administrative)
   - Manual user addition by administrator

2. Subscription Management:

   - Adding backends via API URL and API key
   - Editing friendly names for objects
   - Removing subscriptions

3. Data Visualization:

   - Automatically generated tabular views
   - Historical charts (12h with averaging)
   - Current status with last contact time
   - Embeddable widgets for external websites

4. Caching System:

   - Cache-first mode
   - Manual cache control
   - Data polling every 1 second (configurable)

## 4. Product Boundaries

Elements outside MVP scope:

- Reports and alarms
- React Native application
- ThingsBoard support
- Connection type autodetection
- Advanced cache management
- Self-service account management
- Ticketing system
- Full API autodiscovery
- Backend processing modules
- Multilingual support
- Device control functionality

## 5. User Stories

US-001: System Login

- As a user, I want to log into the system
- Acceptance Criteria:
  - Login form with username and password fields
  - Input validation
  - Redirect to appropriate view after login
  - Login error handling

US-002: Adding New Subscription

- As a service user, I want to add a new backend for monitoring
- Acceptance Criteria:
  - Form with API URL and API key fields
  - Data validation
  - Automatic connection testing
  - Success/error confirmation

US-003: Current Data Viewing

- As a user, I want to see the current state of monitored devices
- Acceptance Criteria:
  - Automatically generated data table
  - 1-second refresh rate
  - Clear connection status indicator
  - Last server contact time
  - Active data transfer indicator

US-004: History Viewing

- As a user, I want to see measurement history
- Acceptance Criteria:
  - Chart with last 12h of data
  - Chart scrolling capability
  - Data averaging
  - Data export

US-005: User Management

- As an administrator, I want to manage user accounts
- Acceptance Criteria:
  - User list
  - New user addition
  - Permission editing
  - Account deactivation

US-006: Widget Embedding

- As a user, I want to embed a widget on an external website
- Acceptance Criteria:
  - Embed code generation
  - Displayed data configuration
  - Widget responsiveness
  - Automatic refresh

US-007: Field Work Preparation

- As a technician, I want to prepare data for field work
- Acceptance Criteria:
  - Installation selection for caching
  - Download progress indicator
  - Downloaded data size information
  - Cache completeness confirmation
  - Force cache refresh option

US-008: Refresh Rate Configuration

- As a user, I want to adjust data polling frequency
- Acceptance Criteria:
  - Refresh interval selection (1-60s)
  - Separate settings for different views
  - Preference saving
  - Active refresh indicator

## 6. Success Metrics

1. Technical:

   - Successful data subscription from IBI Lab and URHydro
   - Response time under 1s for basic operations
   - Offline operation after initial data load
   - Transfer usage below 1MB/h during continuous monitoring

2. Usability:

   - One-handed operation on mobile devices
   - Proper operation in poor GSM signal conditions
   - Successful widget embedding on external sites
   - Data readability on mobile screens

3. Business:

   - Deployment on minimum 2 production installations
   - Zero data loss during connection interruptions
   - Positive feedback from field users
   - Stable operation on Mikrus 1.0 Pro
