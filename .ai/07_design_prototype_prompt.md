# Comprehensive Design Prototype Prompt for ZeroClient Ultra-Rapid MVP

## Project Overview

Create an interactive design prototype for ZeroClient, a web-based industrial monitoring system optimized for challenging connectivity environments. The prototype should prioritize mobile usability with one-handed operation while demonstrating the core functionality outlined below.

## Design System Requirements

### Brand Identity
- **Name**: ZeroClient
- **Tagline**: "Reliable monitoring anywhere"

### Color Palette
- **Primary**: #0063B2 (deep blue) - for main actions and navigation
- **Secondary**: #54D1DB (teal) - for secondary elements and highlights
- **Accent**: #FF8C42 (orange) - for alerts, warnings, and critical actions
- **Background**: #F4F7FA (light gray) - for main content areas
- **Text**: #1A2A3A (dark blue-gray) - for primary text content
- **Status Colors**:
  - Online: #2ECC71 (green)
  - Warning: #F39C12 (amber)
  - Error: #E74C3C (red)
  - Offline: #95A5A6 (gray)
  - Cached: #3498DB (blue with pattern)

### Typography
- **Headings**: Inter, sans-serif, semi-bold
- **Body**: Inter, sans-serif, regular
- **Data Values**: Roboto Mono, monospace - for measurements and timestamps
- **Size Scale**: 
  - Mobile-optimized (16px base)
  - Large touch targets (min 44×44px)

## Core Screens

### 1. Landing Page
- **Purpose**: Introduce ZeroClient and provide authentication access
- **Key Elements**:
  - Logo and tagline
  - Brief product description
  - "Sign In" and "Sign Up" buttons
  - Mobile-friendly header with navigation
- **Mobile Considerations**:
  - All interactive elements in thumb zone
  - Clear call-to-action buttons

### 2. Authentication Screens
- **Purpose**: Allow users to sign in or create new accounts
- **Key Elements**:
  - Email/password login form
  - Error states for invalid credentials
  - Loading states
  - "Remember me" option
  - Password reset link
- **Mobile Considerations**:
  - Keyboard-friendly input fields
  - Clear form validation feedback

### 3. Dashboard
- **Purpose**: Overview of all connected backends
- **Key Elements**:
  - Connection status cards for each backend
  - Last contact timestamp
  - Quick-access buttons to detailed views
  - Add new connection button
  - Global cache status indicator
- **Mobile Considerations**:
  - Scrollable card layout
  - Touch-friendly cards

### 4. Backend Connection Management
- **Purpose**: Add and manage backend connections
- **Key Elements**:
  - Form to add new connection (URL and API key)
  - Test connection button
  - List of existing connections
  - Edit/delete options
  - Connection status indicators
- **Mobile Considerations**:
  - Simplified form layout
  - Clear success/error feedback

### 5. Data Visualization
- **Purpose**: Display tabular device data with status
- **Key Elements**:
  - Auto-generated data table
  - Column sorting controls
  - Last value and timestamp for each parameter
  - Status indicators
  - Refresh rate control
  - Cache status per value
- **Mobile Considerations**:
  - Horizontal scrolling for wide tables
  - Fixed header for context

### 6. Cache Management
- **Purpose**: Control offline data availability
- **Key Elements**:
  - Cache status overview
  - Manual cache controls (refresh/clear)
  - Download progress indicators
  - Storage usage visualization
  - "Prepare for field work" action
- **Mobile Considerations**:
  - Large, easy-to-tap control buttons
  - Clear visual feedback on actions

### 7. Widget Configuration
- **Purpose**: Create embeddable widgets for external sites
- **Key Elements**:
  - Widget preview
  - Configuration options
  - Device/parameter selection
  - Generated embed code
  - Test widget button
- **Mobile Considerations**:
  - Simplified configuration on mobile
  - Easy code copying mechanism

## Navigation & Interaction Patterns

### Primary Navigation
- Bottom navigation bar on mobile (thumb-accessible)
- Sidebar navigation on desktop
- Key sections: Dashboard, Data, Cache, Widgets, Settings

### Interaction Patterns
- Pull-to-refresh for data updates
- Swipe gestures for common actions
- One-handed operation with all critical controls in thumb zone
- Clear loading and error states
- Offline mode visual indicators

## Component Specifications

### Data Table
- Sortable columns with clear indicators
- Status indicators using color + icon
- Monospace font for data values
- Last updated timestamp in relative format
- Horizontal scrolling with fixed first column on mobile

### Status Indicators
- Clear visual differentiation between states
- Combination of color and icon for accessibility
- Consistent placement throughout interface
- Tooltip explanations on hover/press

### Form Controls
- Large input fields optimized for touch
- Clear validation feedback
- Prominent action buttons
- Logical tab order

### Cache Controls
- Visual progress indicators
- Clear success/failure states
- Prominent manual controls
- Storage usage visualization

## Deliverables

1. **Interactive Prototype**:
   - Web-based, shareable via URL
   - Demonstrates core user flows
   - Shows responsive behavior

2. **Design Specifications**:
   - Color values (hex/RGB)
   - Typography details (font, size, weight)
   - Component measurements
   - Spacing system

3. **Asset Package**:
   - Icon set in SVG format
   - Logo files
   - UI component examples

4. **Responsive Breakpoints**:
   - Mobile: 320-480px
   - Tablet: 481-768px
   - Desktop: 769px+

## Development Context

This prototype will serve as the foundation for ultra-rapid development, with implementation beginning immediately after design approval. The design must be practical to implement within a 3-day coding timeline using Astro, React, Tailwind CSS, and Shadcn/UI components.

By Alice @ Claude 3.7 Sonnet, 20250513
