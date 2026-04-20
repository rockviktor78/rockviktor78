# Project Overview

## What is Join?

**Join** is a web-based Kanban project management tool designed for teams to organize tasks, manage contacts, and track project progress.

## Main Goals

- **Clean Architecture** - Separation of concerns, modular structure
- **High Performance** - Minimal dependencies, lazy loading
- **Responsive Design** - Mobile-first approach (320px → 1920px+)
- **Accessibility** - Semantic HTML, ARIA labels, keyboard navigation
- **Maintainability** - BEM CSS, functional JS, clear file structure

## Tech Stack

### Core Technologies

- **HTML5** - Semantic markup
- **CSS3** - Custom Properties, Grid, Flexbox
- **Vanilla JavaScript (ES6+)** - No frameworks
- **Firebase Realtime Database** - Backend & Auth

### No Dependencies

- No jQuery
- No CSS frameworks (Bootstrap, Tailwind)
- No JavaScript frameworks (React, Vue, Angular)
- Pure Web Standards

## Key Features

### 1. Authentication System

- Login/Signup with Firebase
- Guest access mode
- Session management
- Password validation

### 2. Kanban Board

- Drag & drop tasks
- Task status: To Do, In Progress, Await Feedback, Done
- Assignee management
- Priority levels
- Due dates

### 3. Task Management

- Create new tasks
- Edit existing tasks
- Delete tasks
- Assign to contacts
- Category labels
- Subtask checklists

### 4. Contact Management

- Add/edit/delete contacts
- Contact details (name, email, phone)
- Color-coded initials
- Contact picker for task assignment

### 5. Summary Dashboard

- Tasks overview
- Urgent tasks counter
- Board status summary
- Next deadline display

### 6. Responsive Navigation

- Desktop: Sidebar navigation
- Mobile: Bottom navigation bar
- External mode: Login + Footer links (Privacy/Legal)

## User Flows

### External User (Not Logged In)

```
Landing Page (index.html)
    ├── Login
    ├── Signup
    └── Guest Access
        ↓
Privacy Policy / Legal Notice (with mobile back button)
```

### Authenticated User

```
Summary Dashboard (summary.html)
    ├── Board (board.html)
    ├── Add Task (add-task.html)
    ├── Contacts (contacts.html)
    └── Help (help.html)
```

## Design System

### Color Palette

- Primary: Blue (`--color-primary`)
- Accent: Light Blue (`--color-accent`)
- Success: Green
- Warning: Orange
- Error: Red

### Typography

- Font Family: Inter (100-900 weights)
- Scale: 12px → 56px
- Base: 16px (rem)

### Spacing System

```
--spacing-xs: 0.25rem  (4px)
--spacing-sm: 0.5rem   (8px)
--spacing-md: 1rem     (16px)
--spacing-lg: 1.5rem   (24px)
--spacing-xl: 2rem     (32px)
--spacing-2xl: 3rem    (48px)
--spacing-5xl: 6rem    (96px)
```

## Project Constraints

### Must Follow

- BEM CSS naming convention
- Mobile-first responsive design
- No inline styles
- No business logic in HTML
- Firebase calls only through wrapper
- Max function length: 30 lines

### Must NOT Do

- Use JavaScript frameworks
- Add CSS frameworks
- Create nested BEM elements
- Use `!important` (except utilities)
- Commit `.ai/` folder to git

## Performance Targets

- First Contentful Paint: < 1.5s
- Time to Interactive: < 3s
- Lighthouse Score: 90+
- Mobile-friendly (Google Test)

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Project Status

**Current Phase:** Production
**Version:** 1.0
**Last Major Update:** Mobile navigation refactor (External mode)

## Team

- Developer: VWilhelm
- Copilot Integration: Active
- AI Context Architecture: Enabled

## Resources

- Firebase Project: `join-7c944-default-rtdb`
- Region: europe-west1
- Assets: Local (no CDN)

## Educational Context

⚠️ **Important Note:**
This project is built as an educational exercise. It is NOT intended for extensive business usage. While we strive for the best possible user experience, we cannot guarantee consistent availability, reliability, or accuracy.
