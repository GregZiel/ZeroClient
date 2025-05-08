# ZeroClient Embeddable Widget Technology Stack Analysis

## 1. Overview

The ZeroClient system requires embeddable widgets that can be placed on third-party websites, displaying real-time industrial data with minimal setup. This document analyzes the technology stack considerations for implementing these widgets with focus on reliability, minimal footprint, and connectivity options.

## 2. Core Architecture Recommendations

### 2.1 Self-Contained Widget Approach

A standalone, self-contained widget package is recommended over a Module Federation architecture for embeddable components:

- **Single JavaScript file**: Package the widget as a lightweight, standalone script
- **Minimal dependencies**: Include only essential runtime libraries
- **Simple embedding**: Enable installation via a single script tag
- **Content isolation**: Ensure the widget doesn't interfere with host page styles/scripts

This approach provides the simplest developer experience for third parties while maintaining optimal performance characteristics.

### 2.2 Communication Layer

The communication stack should implement a dual-protocol approach:

**Primary: MQTT over WebSockets**
- Connect to Mosquitto server via secured WebSockets (wss://)
- Subscribe to device-specific topics for real-time updates
- Benefits: lower latency, reduced bandwidth for continuous monitoring
- Challenges: may be blocked by corporate firewalls, requires WebSocket support

**Fallback: HTTP REST API**
- Implement automatic fallback when WebSockets/MQTT is unavailable
- Use polling with configurable intervals
- Benefits: universal compatibility, simpler debugging
- Challenges: higher bandwidth consumption, potential server load with many widgets

## 3. Technical Implementation Considerations

### 3.1 Widget Construction

- **Framework**: Minimal React runtime (~40KB gzipped) offers good balance of features vs size
- **Build process**: Use Webpack with aggressive tree-shaking and code splitting
- **CSS approach**: Shadow DOM or CSS-in-JS with prefix namespacing to prevent style leakage
- **Versioning**: Explicit versioning in file paths to support multiple concurrent versions

### 3.2 Widget Loading Flow

1. Host page includes minimal loader script (~2KB)
2. Loader detects widget containers with data attributes
3. Loader fetches the appropriate widget bundle
4. Widget self-initializes and establishes data connection
5. Widget renders and begins updating with live data

### 3.3 Data Management

- **Caching**: Implement IndexedDB storage for offline viewing
- **State management**: Minimal state library or React context
- **Reconnection**: Implement exponential backoff for connection retry
- **Data transformation**: Process data client-side to minimize payload size

## 4. Security Considerations

- **Authentication**: Use short-lived, scoped JWT tokens for API authorization
- **Data access**: Widget tokens should have read-only access to specific devices only
- **CORS configuration**: Ensure API endpoints properly implement CORS for browser security
- **Content Security Policy**: Design widgets to work with strict CSP implementations
- **Parameter sanitization**: Validate all configuration parameters to prevent injection attacks

## 5. Performance Optimization

- **Initial payload**: Target <100KB initial download (gzipped)
- **Lazy loading**: Load visualization libraries only when needed
- **Connection pooling**: Reuse MQTT connections across multiple widgets on same page
- **Transfer minimization**: Implement delta updates when possible
- **Resource loading**: Use preconnect and dns-prefetch hints

## 6. Integration with ZeroClient Core

While widgets are deployed separately from the main ZeroClient application, they should:

- Share core visualization components with the main application
- Maintain consistent styling and branding
- Use the same data processing libraries
- Be configurable through the main ZeroClient admin interface
- Report telemetry back to the main system for monitoring

## 7. Implementation Recommendations

1. **Create widget configuration UI** in main ZeroClient application
2. **Develop SDK package** for widget development with testing tools
3. **Implement versioned API contracts** between widgets and backend
4. **Establish performance benchmarks** for widget loading and rendering
5. **Create comprehensive embedding documentation** for customers
6. **Build demonstration page** showcasing various widget configurations

## 8. Example Embedding Code

```html
<!-- Minimal embedding example -->
<div 
  data-zeroclient-widget 
  data-api-key="abc123xyz"
  data-device-id="pump-station-42"
  data-refresh="5"
  data-protocol="auto">
</div>
<script src="https://widgets.zeroclient.com/v1/loader.js" async></script>
```

This technology stack ensures ZeroClient widgets will function reliably across diverse hosting environments while maintaining performance, security, and user experience standards.

by Alice @ Claude 3.7 Sonnet, 20250508