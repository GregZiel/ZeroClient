# UI Architecture for ZeroClient MVP

## 1. UI Structure Overview

The ZeroClient MVP UI is designed with a mobile-first approach, optimized for one-handed operation while providing comprehensive industrial monitoring capabilities. The architecture follows a hierarchical structure with a bottom navigation bar on mobile and side navigation on desktop, organizing content into logical sections based on user needs.

The interface prioritizes clear status indicators, efficient data presentation, and offline functionality, with special attention to usability in challenging field conditions. The design leverages Shadcn/UI components extended with custom elements specific to industrial monitoring needs.

## 2. View List

### Authentication View

- **Path**: `/auth`
- **Primary Purpose**: Allow users to authenticate into the system
- **Key Information**: Login form, authentication status
- **Key Components**:
  - Email/password login form
  - Authentication status indicator
  - Error messaging for invalid credentials
  - Loading states
- **Considerations**: 
  - Leverages Supabase Auth UI components
  - Maintains security while minimizing friction
  - Supports touch-friendly input on mobile
  - Provides clear error feedback

### Dashboard View

- **Path**: `/dashboard`
- **Primary Purpose**: Provide overview of all connected backends
- **Key Information**: Connection status cards, primary variables for URHydro stations
- **Key Components**:
  - Backend connection cards with status indicators
  - Last contact timestamp for each connection
  - Primary variable display for URHydro stations
  - Quick-action buttons for common operations
  - Add new connection button
- **Considerations**:
  - Card-based layout adapts to screen sizes (1 column on mobile, 2+ on larger screens)
  - Touch-friendly cards with clear visual hierarchy
  - Status indicators use both color and iconography
  - Timestamps use relative format for recency with ISO format on demand

### Backend Connection Management View

- **Path**: `/connections`
- **Primary Purpose**: Add and manage backend connections
- **Key Information**: Connection list, connection details, status
- **Key Components**:
  - Add connection form with URL and API key inputs
  - Test connection button with feedback
  - Connection list with status indicators
  - Edit/delete options for existing connections
- **Considerations**:
  - Form inputs optimized for touch with clear validation
  - Secure handling of API keys
  - Clear success/error feedback for connection testing
  - Confirmation for destructive actions

### Data Visualization View

- **Path**: `/data/:connectionId`
- **Primary Purpose**: Display tabular device data with status
- **Key Information**: Device measurements, status indicators, timestamps
- **Key Components**:
  - Auto-generated data table with device measurements
  - Column sorting controls
  - Polling frequency selector
  - Last updated timestamps
  - Connection status indicator
- **Considerations**:
  - Horizontally scrollable tables with fixed headers
  - Touch-optimized controls for sorting and filtering
  - Collapsible rows for additional details on mobile
  - Clear indication of data freshness via timestamps
  - Pull-to-refresh gesture support

### Device Detail View

- **Path**: `/data/:connectionId/:deviceId`
- **Primary Purpose**: Show detailed information for a specific device
- **Key Information**: All device parameters, status, historical data
- **Key Components**:
  - Comprehensive parameter table
  - Last updated timestamps
  - Device status indicator
  - Quick actions specific to device
- **Considerations**:
  - Organized presentation of potentially numerous parameters
  - Clear grouping of related data points
  - Touch-optimized controls
  - Efficient navigation back to device list

### Cache Management View

- **Path**: `/cache`
- **Primary Purpose**: Control offline data availability
- **Key Information**: Cache status, storage usage, download progress
- **Key Components**:
  - Cache status overview
  - Manual cache controls (refresh/clear)
  - Download progress indicators
  - Storage usage visualization
  - "Prepare for field work" action
- **Considerations**:
  - Clear visual feedback for cache operations
  - Large, touch-friendly control buttons
  - Progress indicators for long-running operations
  - Confirmation for destructive actions

### Widget Configuration View

- **Path**: `/widgets`
- **Primary Purpose**: Create embeddable widgets for external sites
- **Key Information**: Widget settings, preview, embed code
- **Key Components**:
  - Widget configuration form
  - Live preview panel
  - Generated embed code with copy button
  - Backend connection selector
  - Device/parameter selection
- **Considerations**:
  - Two-panel layout (form/preview) on desktop
  - Tab-based switching between configuration and preview on mobile
  - One-click code copying with confirmation
  - Clear preview that accurately represents embedded result

### Settings View

- **Path**: `/settings`
- **Primary Purpose**: Manage user preferences and application settings
- **Key Information**: User profile, preferences, defaults
- **Key Components**:
  - User profile information
  - Default polling frequency selector
  - Notification preferences
  - Theme settings (if applicable)
- **Considerations**:
  - Clear organization of settings by category
  - Touch-friendly toggle controls
  - Immediate feedback for preference changes
  - Sensible defaults for all settings

## 3. User Journey Map

### Primary User Journey: Field Technician Workflow

1. **Authentication**
   - User opens application on mobile device
   - Enters credentials and authenticates
   - System validates credentials and redirects to Dashboard

2. **Dashboard Overview**
   - User views status cards for all connected backends
   - Identifies the specific URHydro station to monitor
   - Taps on the station card to access detailed data

3. **Data Monitoring**
   - User views auto-generated table of device data
   - Checks last updated timestamps for freshness
   - Adjusts polling frequency if needed
   - Monitors critical parameters

4. **Offline Preparation**
   - User navigates to Cache Management view
   - Selects specific installation to cache
   - Initiates "Prepare for field work" action
   - Monitors download progress
   - Receives confirmation when caching is complete

5. **Field Work with Offline Access**
   - User travels to location with poor connectivity
   - Application automatically switches to offline mode
   - User continues to access cached data
   - System clearly indicates offline status and data age

6. **Reconnection**
   - When connectivity returns, system indicates online status
   - User can manually refresh cache if needed
   - Application resumes normal polling operations

### Secondary User Journey: Widget Creation

1. **Authentication**
   - Administrator logs into system
   - System redirects to Dashboard

2. **Widget Configuration**
   - Administrator navigates to Widget Configuration view
   - Selects backend connection and specific parameters to display
   - Configures refresh rate and appearance options
   - Views live preview of widget

3. **Widget Deployment**
   - Administrator copies generated embed code
   - Pastes code into external website
   - Tests widget functionality
   - Widget displays selected parameters with auto-updating

## 4. Navigation and Layout Structure

### Mobile Navigation

- **Primary Navigation**: Bottom tab bar with 5 key sections
  - Dashboard (home icon)
  - Data (table/chart icon)
  - Cache (download icon)
  - Widgets (code icon)
  - Settings (gear icon)

- **Secondary Navigation**:
  - Back button in header for nested views
  - Breadcrumb display showing current location in hierarchy
  - Pull-to-refresh for data updates
  - Swipe gestures for horizontal scrolling in tables

### Desktop Navigation

- **Primary Navigation**: Side navigation panel with same 5 key sections
  - Expanded labels with icons
  - Collapsible for more screen space when needed

- **Secondary Navigation**:
  - Breadcrumb navigation for hierarchy
  - Tabbed interfaces for related content
  - Keyboard shortcuts for common actions

### Layout Structure

- **Mobile Layout**:
  - Single column card-based layout
  - Full-width components
  - Bottom-aligned action buttons in thumb zone
  - Collapsible sections for dense information

- **Desktop Layout**:
  - Multi-column grid layout
  - Side-by-side panels for configuration/preview
  - Expanded tables with more visible columns
  - Persistent side navigation

## 5. Key Components

### Status Indicator

- Visual component displaying connection or cache status
- Uses color coding (green/amber/red/gray) with icons
- Provides additional details on hover/tap
- Consistent appearance across all views

### Data Table

- Displays tabular device data with sortable columns
- Features fixed headers with horizontal scrolling
- Supports touch-friendly sorting and filtering
- Provides collapsible rows for additional details on mobile
- Includes last updated timestamps for each value

### Connection Card

- Card component displaying backend connection information
- Shows connection name, status, last contact time
- For URHydro: displays primary variable prominently
- Includes quick-action buttons
- Serves as navigation to detailed data view

### Form Controls

- Touch-optimized input components
- Large touch targets (minimum 44×44px)
- Clear validation feedback
- Consistent styling across the application
- Secure input handling for sensitive data

### Cache Controls

- Interface elements for managing offline data
- Includes manual refresh and clear buttons
- Features progress indicators for operations
- Displays storage usage visualization
- Provides clear feedback on cache status

### Timestamp Display

- Consistently formatted time information
- Uses relative format for recent events ("2 mins ago")
- Provides full ISO format on hover/tap
- Includes visual differentiation for different timestamp types
- Supports user's preferred timezone

### Toast Notifications

- Transient feedback system for operations and errors
- Appears briefly without requiring user dismissal
- Uses color coding for different message types
- Provides action buttons when applicable
- Stacks appropriately when multiple notifications occur

### Loading States

- Consistent loading indicators across the application
- Skeleton screens for content loading
- Progress bars for operations with known duration
- Clear messaging for longer operations
- Fallbacks for offline mode

By Alice @ Claude 3.7 Sonnet, 20250516
