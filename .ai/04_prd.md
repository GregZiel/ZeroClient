# Product Requirements Document (PRD) - ZeroClient Ultra-Rapid MVP

## 1. Product Overview

ZeroClient is a web-based industrial monitoring system designed for operation in challenging connectivity environments. This ultra-rapid MVP will be developed in a compressed 3-4 day timeline, focusing exclusively on core functionality with a strong emphasis on upfront design.

The system will deliver a cache-first monitoring solution with auto-generated tabular views of industrial data, mobile-optimized interface, and basic offline capabilities. The MVP will support connections to two specific backend systems (IBI Lab and URHydro) while providing minimal but effective authentication.

For initial deployment, the system will target DigitalOcean/Cloudflare infrastructure, with later migration to Mikrus 1.0 Pro environments planned for subsequent iterations.

This document outlines the requirements for a functional, deployable system achievable within the ultra-rapid development constraints while establishing a foundation for future evolution.

## 2. User Problem

Field technicians and monitoring personnel face significant challenges when accessing industrial data in environments with:

1. Unstable or limited connectivity
2. Need for mobile-based monitoring
3. Multiple backend systems requiring separate logins
4. Inefficient interfaces not optimized for field conditions

ZeroClient addresses these problems through:
- Automatic generation of monitoring views without requiring pre-configuration
- Cache-first architecture enabling offline operation
- Mobile-optimized interface designed for one-handed operation
- Unified access to multiple backend systems
- Minimal data transfer design for economically-sensitive connections

## 3. Functional Requirements

### 3.1 Authentication and User Management
1. Basic login with email/password
2. Simple user creation via admin interface
3. Minimal access control (admin vs regular user)
4. Protected routes requiring authentication

### 3.2 Backend Connection Management
1. Add new backend via URL and API key
2. Test connection functionality
3. Manage existing connections (rename/delete)
4. Display connection status for each backend
5. Show timestamp of last successful connection

### 3.3 Data Visualization
1. Auto-generated tabular view for device data
2. Basic status indicators for connection health
3. Data polling at configurable frequency (default: 1 second)
4. Mobile-optimized view layouts
5. Last updated timestamp for each value

### 3.4 Caching System
1. IndexedDB storage for offline data access
2. Manual cache controls (refresh/clear)
3. Cache status indicators
4. Offline detection and fallback to cached data

### 3.5 Widget Functionality
1. Simple embeddable script for external websites
2. Basic device data display in embedded context
3. HTTP-only data fetching for widgets
4. Widget configuration options
5. Embed code generation

### 3.6 Deployment and Operations
1. Docker containerization for cloud deployment
2. CI/CD pipeline with GitHub Actions
3. Initial deployment to DigitalOcean/Cloudflare
4. Automated testing before deployment
5. Basic monitoring for system health
6. Preparation for future Mikrus deployment

## 4. Product Boundaries

The following features are explicitly excluded from the ultra-rapid MVP:

1. Advanced access control (four-tier system will be simplified to admin/regular)
2. Advanced cache management features
3. Historical data visualization (may be included if time permits)
4. Support for backends beyond IBI Lab and URHydro
5. Microfrontend architecture (will be monolithic for MVP)
6. Comprehensive error handling
7. Detailed user preferences
8. Bandwidth-aware operation modes
9. Comprehensive dashboard
10. Self-service user management
11. Mikrus deployment (planned for future iteration)

These features are documented for future development but will not be included in the 3-4 day implementation.

## 5. User Stories

### US-001: User Authentication
As a user, I want to log into the system to access monitoring features.

Acceptance Criteria:
- Basic login form with email and password
- Simple error handling for invalid credentials
- Redirect to main view after successful login
- Protected routes requiring authentication

### US-002: Backend Connection
As a user, I want to add a new backend connection to monitor an industrial installation.

Acceptance Criteria:
- Form with URL and API key inputs
- Test connection functionality
- Success/failure feedback
- Newly added backend appears in list immediately

### US-003: Device Data Monitoring
As a user, I want to view current device data in a tabular format.

Acceptance Criteria:
- Auto-generated table showing device parameters
- Values update at configured polling rate (default 1s)
- Last updated timestamp for each value
- Connection status indicator
- Mobile-friendly table layout

### US-004: Basic Cache Control
As a user, I want to manually control data caching for offline use.

Acceptance Criteria:
- "Cache all" button to force data storage
- Cache status indicator
- Clear cache option
- System indicates when viewing cached vs live data

### US-005: Widget Generation
As a user, I want to create a simple embeddable widget for external websites.

Acceptance Criteria:
- Basic widget configuration options
- Generated embed code
- Preview capability
- Widget displays selected device data when embedded

### US-006: Mobile Interface Usage
As a field technician, I want to use the system effectively on my smartphone.

Acceptance Criteria:
- Primary controls accessible in thumb zone
- Touch-friendly input elements
- Responsive layout adapting to screen size
- Readability of data on mobile screens

### US-007: Continuous Integration & Deployment
As a developer, I want automated testing and deployment to ensure quality and rapid iteration.

Acceptance Criteria:
- GitHub Actions pipeline runs on each commit
- Automated tests execute before deployment
- Build failures prevent deployment
- Successful builds deploy automatically to staging environment
- Production deployment requires manual approval

## 6. Success Metrics

### 6.1 Development Timeline Metrics
- Completion of essential design and project setup in day 1
- Implementation of core functionality within days 2-3
- Functional deployment with CI/CD by end of day 4
- All essential user stories implemented

### 6.2 Technical Performance Metrics
- Successful connection to both target backends
- Functional operation in areas with poor GSM coverage
- Cache functionality allowing offline data access
- Data usage under 1MB per hour of monitoring
- Initial load under 3 seconds on LTE connection

### 6.3 User Experience Metrics
- One-handed operation possible on smartphone
- Data legibility on mobile screens
- Successful widget embedding on test page
- Basic tasks completable within 30 seconds

### 6.4 Implementation Strategy Metrics
- Effective use of AI pairing for acceleration
- Successful leverage of pre-built components
- Appropriate technical simplifications that don't compromise core functionality
- Documented evolution path for post-MVP development
- CI/CD pipeline enabling rapid iteration
- Successful initial deployment to cloud infrastructure

This ultra-rapid MVP prioritizes speed and core functionality while establishing the foundation for the complete ZeroClient vision in future iterations.

By Alice @ Claude 3.7 Sonnet, 20250512