# PRD Planning Summary for ZeroClient MVP

## Decisions Made

1. The MVP focuses on basic monitoring functionality with automatically generated tabular views.
2. The system will operate in a cache-first mode with manual cache control.
3. Data polling every second with configurable frequency.
4. Deployment on Mikrus 1.0 Pro as the target environment for small installations.
5. Authentication initially through an administrative system (manual user addition).
6. ISO date format in the interface.
7. Architecture based on microfrontends with scalability options.
8. Support for at least two backends: IBI Lab and URHydro.

## Key Recommendations

1. Implement retry mechanisms and error handling for unstable connections.
2. Create basic data transfer monitoring.
3. Develop a simple POC with a single microfrontend.
4. Design the cache structure with future scalability in mind.
5. Document APIs and data formats.
6. Implement a basic login system.
7. Create a mock backend for testing.

## Requirements Summary

### Functional Requirements

- Authentication system with four access levels.
- Automatically generated tabular views for devices.
- Basic data caching system.
- Configurable data polling frequency.
- Support for at least two backends.
- Mobile-friendly interface.

### User Stories

1. As a technician, I want to quickly see the current status of a device in the field.
2. As an administrator, I want to add new users to the system.
3. As a user, I want offline data access through caching.
4. As a monitor, I want to see the measurement history.

### Success Criteria

- Successful data subscription from two objects.
- Ability to operate in poor GSM signal conditions.
- Effective widget display on an external site.
- Efficient operation on mobile devices.

## Unresolved Issues

1. Choice of technology for cache implementation (IndexedDB vs localStorage).
2. Detailed format of the API schema for autodiscovery.
3. Exact specification of the authentication mechanism and token storage.
4. Detailed system performance metrics.
5. Final versioning strategy for APIs and microfrontends.

---
Created on 2025-05-03 by Grzegorz Zieliński with assistance from Alice (Gemini 2.5 Pro, then Claude 3.5 Sonnet).
