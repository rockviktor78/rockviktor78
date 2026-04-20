# Architecture

## System Overview

Join is a **Multi-Page Application (MPA)** with template-based HTML includes and vanilla JavaScript modules.

## Layered Architecture

```
┌─────────────────────────────────┐
│   UI Layer (HTML/CSS)           │
│   - Templates (header, sidebar) │
│   - Pages (board, summary, etc.)│
└─────────────────────────────────┘
          ↓
┌─────────────────────────────────┐
│   Application Layer (JS)        │
│   - Page-specific logic         │
│   - Event handlers              │
│   - Template initialization     │
└─────────────────────────────────┘
          ↓
┌─────────────────────────────────┐
│   Domain Layer                  │
│   - Business logic              │
│   - Data transformations        │
│   - Validation                  │
└─────────────────────────────────┘
          ↓
┌─────────────────────────────────┐
│   Infrastructure Layer          │
│   - Firebase API wrapper        │
│   - Authentication              │
│   - Session management          │
└─────────────────────────────────┘
```

## Core Principles

### 1. Separation of Concerns

- **NO business logic in HTML**
- **NO direct Firebase calls in UI components**
- **NO CSS in JavaScript** (except class toggling)

### 2. Template System

- Header and Sidebar are shared templates
- Loaded via `w3-include-html` attribute
- Initialized through `init-template.js`

### 3. Module Structure

```
scripts/
├── shared/          # Shared utilities (menu, templates, view-transitions)
├── auth/            # Authentication modules
└── [page].js        # Page-specific logic (board.js, contacts.js, etc.)
```

### 4. CSS Architecture

- **BEM naming convention** (Block\_\_Element--Modifier)
- **Mobile-first responsive design**
- **CSS Custom Properties** for theming (variables.css)
- **Component-based styling** (header.css, sidebar.css)

## Data Flow

```
User Interaction
    ↓
Event Handler (page-specific JS)
    ↓
Domain Logic (validation, transformation)
    ↓
Firebase API (firebase.js)
    ↓
Firebase Realtime Database
    ↓
Response Processing
    ↓
UI Update (DOM manipulation)
```

## Rules

### File Organization

- One component = One CSS file
- One page = One JS file
- Shared logic → `scripts/shared/`
- Auth logic → `scripts/auth/`

### Dependencies

- Templates must load **before** JavaScript initialization
- `utilities.js` must load first (provides helpers)
- Firebase calls **only** through `firebase.js` wrapper

### State Management

- Session state in `sessionStorage`
- User data in Firebase Realtime Database
- No global state objects (functional approach)

## Anti-Patterns (NEVER DO)

❌ Inline styles in JavaScript
❌ Business logic in template files
❌ Direct Firebase SDK calls (use wrapper)
❌ Nested BEM elements (`.block__element__subelement`)
❌ Mixed responsibilities in one file
❌ Deep nesting (max 3 levels)

## Performance Rules

- Minimize DOM queries (cache selectors)
- Use event delegation where possible
- Lazy-load images
- Defer non-critical JavaScript
