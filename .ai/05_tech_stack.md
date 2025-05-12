# ZeroClient Ultra-Rapid MVP Technology Stack

## 1. Overview

This document outlines the technology stack for the ZeroClient Ultra-Rapid MVP, designed specifically for completion within a compressed 3-4 day timeline. The stack prioritizes development velocity while ensuring core requirements are met: mobile optimization, offline capabilities, embeddable widgets, and efficient industrial data visualization.

## 2. Frontend Technologies

### Core Framework
- **Astro 5**: Selected for high performance with minimal JavaScript
- **Development Approach**: Static-first with selective hydration for interactive components

### Interactive Components
- **React 19**: Used exclusively for interactive elements requiring client-side functionality
- **Component Library**: Shadcn/UI for pre-built accessible components that accelerate development
- **Styling**: Tailwind CSS 4 for rapid UI development without context switching
- **Mobile Optimization**: Custom utilities for one-handed operation patterns

### Data Management & Caching
- **IndexedDB**: For client-side data storage and offline capabilities
- **State Management**: React Context API for UI state (simplicity over Redux)
- **Polling Implementation**: Lightweight custom solution with configurable intervals

## 3. Backend Services

### Primary Backend
- **Supabase**: Comprehensive BaaS providing:
  - PostgreSQL database for user data and connection metadata
  - Authentication system with JWT tokens
  - Row-level security for data protection
  - RESTful and real-time APIs

### Authentication
- **Implementation**: Supabase Auth with email/password
- **Role System**: Simplified to admin/regular users for MVP
- **UI Components**: Leveraging Supabase Auth UI components for rapid implementation

## 4. Widget Implementation

### Embedding Approach
- **Script Tag Loader**: Simple JavaScript file for external websites to include
- **iFrame Generation**: Script creates an iFrame for proper isolation
- **Configuration**: Via data attributes or URL parameters
- **Authentication**: Read-only API keys for widget data access

### Data Flow
- **HTTP Polling**: Simple approach for initial implementation
- **Shared Codebase**: Widget UI derived from same component library
- **Minimal Customization**: Basic configuration options only

## 5. DevOps & Infrastructure

### CI/CD Pipeline
- **GitHub Repository**: For version control and collaboration
- **GitHub Actions**: Simplified workflow for testing and deployment
- **Testing**: Focused on critical paths only given timeline constraints

### Deployment
- **Initial Target**: DigitalOcean App Platform with Cloudflare integration
- **Containerization**: Docker with single container for simplicity
- **Future Migration**: Prepared for later migration to Mikrus environment

### Monitoring
- **Basic Health Checks**: Simple endpoint monitoring
- **Error Logging**: Console error capturing via basic service

## 6. Development Acceleration

### Tools
- **Vite**: For rapid development and build process
- **TypeScript**: Focused on critical interfaces with minimal overhead elsewhere
- **GitHub Copilot/Cursor.sh**: AI assistance for accelerated development

### Implementation Strategy
- **Monolithic Architecture**: Single codebase approach for simplicity
- **Component-Based**: Well-structured for future evolution despite timeline
- **Selective Testing**: Critical paths only
- **AI Pairing**: Maximizing AI tools for code generation and optimization

## 7. Technology Stack Adaptations for Ultra-Rapid Timeline

### Key Simplifications
1. **Monolithic Over Microfrontends**: Deferring modular architecture
2. **Minimal Type Coverage**: Using TypeScript primarily for API interfaces
3. **Simple State Management**: Context API instead of more sophisticated solutions
4. **Basic Widget Implementation**: iFrame approach for guaranteed isolation with minimal effort
5. **Limited Test Coverage**: Manual testing with automated tests only for critical flows

### Design-First Approach
- **Comprehensive Day 1 Design**: Clear design artifacts to accelerate implementation
- **Component System**: Establishing patterns before coding begins
- **Mobile Patterns**: One-handed operation design from start

## 8. Evolution Path

Despite simplifications for speed, the technology stack maintains compatibility with the long-term vision:

1. **Future Architecture**: Foundation compatible with later microfrontend implementation
2. **Scaling Database**: Schema design allows growth without major restructuring
3. **Widget Enhancement**: Basic approach can be refined in subsequent iterations
4. **Infrastructure Migration**: Containerization facilitates later migration to Mikrus

This technology stack represents a pragmatic balance between development speed and system quality, optimized for the ultra-rapid MVP requirements while establishing a foundation for future evolution.

By Alice @ Claude 3.7 Sonnet, 20250512