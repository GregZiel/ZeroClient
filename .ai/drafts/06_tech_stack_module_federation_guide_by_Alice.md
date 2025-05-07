# Module Federation: A Comprehensive Guide

## What Is Module Federation?

Module Federation is a Webpack 5 feature that enables JavaScript applications to dynamically load code from other sources at runtime. Unlike traditional bundling where everything is compiled together, Module Federation allows multiple independent builds to form a single application, sharing dependencies and code.

Key concepts:
- **Host**: The primary application loading federated modules
- **Remote**: An independently deployed application exposing modules
- **Shared modules**: Common dependencies (like React) shared between host and remotes

## How It Works

1. **Exposing modules**: Remote applications define which modules they expose
2. **Consuming modules**: Host application declares which remote modules to consume
3. **Shared dependencies**: Both specify common dependencies to avoid duplication
4. **Runtime resolution**: The host dynamically loads remote modules when needed

## Implementing Module Federation in ZeroClient

### 1. Setup Webpack Configuration

**For the host application**:

```javascript
// webpack.config.js for host (main ZeroClient application)
const { ModuleFederationPlugin } = require('webpack').container;

module.exports = {
  // ... other webpack config
  plugins: [
    new ModuleFederationPlugin({
      name: 'zeroClient',
      filename: 'remoteEntry.js',
      remotes: {
        // Define which remote modules to consume
        visualizationA: 'visualizationA@http://visualization-a.example.com/remoteEntry.js',
        visualizationB: 'visualizationB@http://visualization-b.example.com/remoteEntry.js',
      },
      shared: {
        // Shared dependencies
        react: { singleton: true, requiredVersion: '^19.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^19.0.0' },
        '@shadcn/ui': { singleton: true }
      },
    }),
  ],
};
```

**For each remote visualization module**:

```javascript
// webpack.config.js for a remote visualization
const { ModuleFederationPlugin } = require('webpack').container;

module.exports = {
  // ... other webpack config
  plugins: [
    new ModuleFederationPlugin({
      name: 'visualizationA',
      filename: 'remoteEntry.js',
      exposes: {
        // Define which components to expose
        './TemperatureChart': './src/components/TemperatureChart',
        './PressureGauge': './src/components/PressureGauge',
      },
      shared: {
        // Same shared dependencies as host
        react: { singleton: true, requiredVersion: '^19.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^19.0.0' },
        '@shadcn/ui': { singleton: true }
      },
    }),
  ],
};
```

### 2. Create a Component Loader

```javascript
// src/components/FederatedComponentLoader.jsx
import React, { Suspense } from 'react';

const loadComponent = (scope, module) => {
  return async () => {
    // Initialize the shared scope
    await __webpack_init_sharing__('default');
    const container = window[scope];
    
    // Initialize the container
    await container.init(__webpack_share_scopes__.default);
    const factory = await container.get(module);
    return factory();
  };
};

export const FederatedComponent = ({ scope, module, ...props }) => {
  // Dynamically import the federated module
  const Component = React.lazy(loadComponent(scope, module));
  
  return (
    <Suspense fallback={<div>Loading visualization...</div>}>
      <Component {...props} />
    </Suspense>
  );
};
```

### 3. Use Federated Components in ZeroClient

```javascript
// src/pages/Dashboard.jsx
import { FederatedComponent } from '../components/FederatedComponentLoader';

function Dashboard() {
  return (
    <div className="dashboard">
      <h1>Industrial Monitoring Dashboard</h1>
      
      <div className="visualization-grid">
        {/* Load remote visualization components */}
        <FederatedComponent 
          scope="visualizationA"
          module="./TemperatureChart"
          deviceId="sensor-123"
          timeRange={12} // hours
        />
        
        <FederatedComponent 
          scope="visualizationB"
          module="./PressureGauge"
          deviceId="sensor-456"
        />
      </div>
    </div>
  );
}
```

## Creating a Plugin System for Customer Visualizations

For ZeroClient's requirement to allow custom visualizations:

1. **Define a clear contract/API**:

```typescript
// src/types/visualization-api.ts
export interface VisualizationProps {
  deviceId: string;
  timeRange?: number;
  refreshRate?: number;
  dataSource: DataSourceConfig;
  theme: 'light' | 'dark';
  onError?: (error: Error) => void;
}

export interface VisualizationMetadata {
  id: string;
  name: string;
  description: string;
  version: string;
  compatibleWith: string; // ZeroClient version
  author: string;
  thumbnail?: string;
}
```

2. **Create a visualization registry**:

```javascript
// src/services/VisualizationRegistry.js
class VisualizationRegistry {
  constructor() {
    this.visualizations = new Map();
    this.metadataCache = new Map();
  }
  
  async registerRemote(url, scope) {
    try {
      // Dynamically load the remote entry
      const script = document.createElement('script');
      script.src = url;
      script.async = true;
      
      // Wait for script to load
      await new Promise((resolve, reject) => {
        script.onload = resolve;
        script.onerror = reject;
        document.head.appendChild(script);
      });
      
      // Fetch metadata
      await this.fetchMetadata(scope);
      
      // Add to registry
      this.visualizations.set(scope, { url, loaded: true });
      
      return true;
    } catch (error) {
      console.error(`Failed to register visualization from ${url}:`, error);
      return false;
    }
  }
  
  async fetchMetadata(scope) {
    // Initialize the shared scope
    await __webpack_init_sharing__('default');
    const container = window[scope];
    
    // Initialize the container
    await container.init(__webpack_share_scopes__.default);
    const getMetadata = await container.get('./metadata');
    const metadata = getMetadata();
    
    this.metadataCache.set(scope, metadata);
    return metadata;
  }
  
  getAvailableVisualizations() {
    return Array.from(this.metadataCache.values());
  }
}

export const visualizationRegistry = new VisualizationRegistry();
```

## Advanced Features and Best Practices

### 1. Version Management

```javascript
// In ModuleFederationPlugin config
shared: {
  react: { 
    singleton: true, 
    requiredVersion: '^19.0.0',
    strictVersion: false // Allow compatible versions
  }
}
```

### 2. Error Handling

```javascript
// Enhanced FederatedComponent with error boundaries
import { ErrorBoundary } from 'react-error-boundary';

export const FederatedComponent = ({ scope, module, fallback, ...props }) => {
  const Component = React.lazy(loadComponent(scope, module));
  
  return (
    <ErrorBoundary 
      FallbackComponent={fallback || DefaultErrorFallback}
      onError={(error) => console.error(`Error loading ${scope}/${module}:`, error)}
    >
      <Suspense fallback={<div>Loading visualization...</div>}>
        <Component {...props} />
      </Suspense>
    </ErrorBoundary>
  );
};
```

### 3. Offline Support with Cache API

```javascript
// Service worker for caching federated modules
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('federated-modules-v1').then((cache) => {
      return cache.addAll([
        '/remoteEntry.js',
        'http://visualization-a.example.com/remoteEntry.js',
        'http://visualization-b.example.com/remoteEntry.js',
        // Add other critical resources
      ]);
    })
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request).then((response) => {
        // Cache important responses for offline use
        if (event.request.url.includes('remoteEntry.js') || 
            event.request.url.includes('chunk')) {
          const responseClone = response.clone();
          caches.open('federated-modules-v1').then((cache) => {
            cache.put(event.request, responseClone);
          });
        }
        return response;
      });
    })
  );
});
```

## Common Challenges and Solutions

1. **Initial load performance**: Use dynamic imports to load modules only when needed
2. **Shared dependency conflicts**: Use strict versioning for critical packages
3. **Development workflow**: Set up proxy configurations for local development
4. **Cross-origin issues**: Ensure proper CORS headers are set for remote modules
5. **Deployment complexity**: Create automated deployment pipelines for consistent updates

## Integration with Astro

While Module Federation is primarily a Webpack feature, it can be integrated with Astro:

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import react from '@astrojs/react';
import node from '@astrojs/node';

// Import custom webpack configuration
import webpackConfig from './webpack.config.mjs';

export default defineConfig({
  integrations: [react()],
  output: 'server',
  adapter: node(),
  vite: {
    ssr: {
      noExternal: ['@shadcn/ui', 'other-packages'],
    },
    // Use Vite's plugin system to incorporate Module Federation
    plugins: [
      {
        name: 'vite-plugin-federation',
        config: () => ({
          build: {
            rollupOptions: {
              // Apply Module Federation configuration
              // You'll need a Vite-compatible Module Federation plugin
            }
          }
        })
      }
    ]
  }
});
```

This configuration is an advanced setup and may require additional plugins like `@originjs/vite-plugin-federation` for full compatibility.

By implementing Module Federation in ZeroClient, you'll create a truly extensible platform where customers can develop and deploy their own visualization components independently, perfectly aligned with your requirements.

by Alice @ Claude 3.7 Sonnet, 20250507