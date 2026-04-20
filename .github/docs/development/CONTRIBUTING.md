# Contributing to Join

Welcome to the **Join** project! This is a Kanban Project Management Tool built with vanilla JavaScript, HTML, and CSS. This guide will help you get started with development.

---

## 📋 Table of Contents

1. [Getting Started](#getting-started)
2. [Project Architecture](#project-architecture)
3. [Development Workflow](#development-workflow)
4. [Git Workflow](#git-workflow)
5. [Code Style & Best Practices](#code-style--best-practices)
6. [Debugging](#debugging)
7. [Common Issues & Solutions](#common-issues--solutions)

---

## 🚀 Getting Started

### Prerequisites

- **Web Browser**: Chrome, Firefox, or Edge (latest version)
- **Code Editor**: VS Code (recommended) or any modern editor
- **Git**: Version control
- **Live Server**: For local development (VS Code extension or other)

### Initial Setup

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd join
   ```

2. **Install Live Server Extension (VS Code)**
   - Open VS Code
   - Install "Live Server" extension by Ritwick Dey
   - Or use any local HTTP server

3. **Start Development Server**
   - Right-click `index.html` → "Open with Live Server"
   - Or use Python: `python3 -m http.server 8000`
   - Navigate to `http://localhost:5500` (or your server's port)

4. **Firebase Configuration**
   - Firebase Realtime Database is already configured
   - Base URL: `https://join-7c944-default-rtdb.europe-west1.firebasedatabase.app/`
   - Check `scripts/firebase.js` for API functions

### Project Structure

```
join/
├── index.html                  # Login/Landing page
├── pages/                      # Application pages
│   ├── summary.html
│   ├── add-task.html
│   ├── board.html
│   ├── contacts.html
│   ├── help.html
│   ├── legal-notice.html
│   └── privacy-policy.html
├── assets/
│   ├── templates/
│   │   ├── layout.html         # Main desktop template
│   │   └── mobile_template.html
│   ├── img/                    # Images
│   └── fonts/                  # Font files
├── scripts/
│   ├── auth/                   # Authentication
│   │   ├── auth_login.js
│   │   ├── auth_signup.js
│   │   └── auth_utilities.js
│   ├── shared/                 # Shared utilities
│   │   ├── include-html.js     # Template loader
│   │   ├── init-template.js    # Template initialization
│   │   ├── menu.js             # Navigation
│   │   └── view-transitions.js # Page transitions
│   ├── firebase.js             # Firebase API
│   ├── board.js                # Board functionality
│   ├── contacts.js             # Contacts management
│   └── summary.js              # Summary dashboard
└── styles/
    ├── base/                   # Foundation
    │   ├── reset.css
    │   ├── variables.css       # CSS custom properties
    │   ├── fonts.css
    │   └── transitions.css
    ├── components/             # Component styles
    │   └── template.css        # Header, sidebar, layout
    ├── auth.css                # Authentication pages
    ├── board.css               # Board page
    ├── contacts.css            # Contacts page
    ├── summary.css             # Summary page
    └── content-pages.css       # Help, Legal, Privacy
```

---

## 🏗️ Project Architecture

### Technology Stack

- **Frontend**: Vanilla JavaScript (ES6+)
- **Styling**: Pure CSS with CSS Custom Properties
- **Backend**: Firebase Realtime Database
- **State Management**: SessionStorage for user sessions
- **Templating**: Custom w3-include-html system

### Key Architectural Patterns

#### 1. **Template System**

Pages use shared templates (`assets/templates/header.html` & `sidebar.html`) loaded via `w3-include-html`:

```html
<body class="app-layout">
  <div
    w3-include-html="../assets/templates/header.html"
    style="display: contents"
  ></div>
  <div
    w3-include-html="../assets/templates/sidebar.html"
    style="display: contents"
  ></div>
  <main class="content-area">
    <!-- Page content -->
  </main>
</body>
```

**Initialization Flow:**

1. `include-html.js` loads the template
2. `init-template.js` initializes header/menu/user
3. `menu.js` sets up navigation and active states

#### 2. **Firebase API**

All data operations go through `scripts/firebase.js`:

```javascript
// Get data
const users = await getData("users");

// Post data
await postData("tasks", taskData);
```

#### 3. **Authentication Flow**

- Login: `auth_login.js` → validates → saves to `sessionStorage`
- User data stored as JSON: `sessionStorage.setItem('loggedInUser', JSON.stringify(user))`
- All pages check session on load

#### 4. **Navigation System**

- Menu buttons managed by `scripts/shared/menu.js`
- Active state automatically set based on current page
- View Transitions API for smooth page changes (disabled for footer links)

---

## 💻 Development Workflow

### Adding a New Page

1. **Create HTML file** in `pages/`:

   ```html
   <!doctype html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <title>New Page - Join</title>

       <!-- Base styles -->
       <link rel="stylesheet" href="../styles/base/reset.css" />
       <link rel="stylesheet" href="../styles/base/variables.css" />
       <link rel="stylesheet" href="../styles/base/fonts.css" />
       <link rel="stylesheet" href="../styles/components/template.css" />

       <!-- Page-specific styles -->
       <link rel="stylesheet" href="../styles/new-page.css" />

       <!-- Shared scripts -->
       <script src="../scripts/shared/include-html.js" defer></script>
       <script src="../scripts/shared/init-template.js" defer></script>
       <script src="../scripts/shared/view-transitions.js" defer></script>
       <script src="../scripts/shared/menu.js" defer></script>

       <!-- Page-specific script -->
       <script src="../scripts/new-page.js" defer></script>
     </head>

     <body class="app-layout">
       <div
         w3-include-html="../assets/templates/header.html"
         style="display: contents"
       ></div>
       <div
         w3-include-html="../assets/templates/sidebar.html"
         style="display: contents"
       ></div>

       <main class="content-area">
         <div class="content-limit">
           <!-- Your content -->
         </div>
       </main>
     </body>
   </html>
   ```

2. **Create JavaScript file** in `scripts/`:

   ```javascript
   // scripts/new-page.js

   /**
    * Initialize page
    */
   function init() {
     checkUserSession();
     loadData();
   }

   function checkUserSession() {
     const user = sessionStorage.getItem("loggedInUser");
     if (!user) {
       window.location.href = "../index.html";
     }
   }

   document.addEventListener("DOMContentLoaded", init);
   ```

3. **Create CSS file** in `styles/`:

   ```css
   /* styles/new-page.css */

   .new-page-container {
     /* Use CSS variables from base/variables.css */
     padding: var(--spacing-xl);
     background: var(--color-white);
   }
   ```

4. **Add to navigation** in `scripts/shared/menu.js`:
   ```javascript
   const menuButtons = {
     // ... existing
     navNewPage: "../pages/new-page.html",
   };
   ```

### Modifying Styles

**Use CSS Custom Properties:**

```css
/* ✅ Good - Uses variables */
.element {
  color: var(--color-primary);
  padding: var(--spacing-md);
  border-radius: var(--radius-lg);
}

/* ❌ Bad - Hardcoded values */
.element {
  color: #2a3647;
  padding: 16px;
  border-radius: 8px;
}
```

**Available Variables** (see `styles/base/variables.css`):

- Colors: `--color-primary`, `--color-accent`, `--color-gray-*`
- Spacing: `--spacing-xs` to `--spacing-5xl`
- Typography: `--font-size-*`, `--font-weight-*`
- Borders: `--radius-*`

---

## 🔀 Git Workflow

### Branch Strategy

```bash
main                    # Production-ready code
├── develop             # Integration branch
├── feature/xyz         # New features
├── fix/abc             # Bug fixes
└── refactor/xyz        # Code improvements
```

### Working on a Feature

1. **Create a branch from `develop`**:

   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/your-feature-name
   ```

2. **Make changes and commit regularly**:

   ```bash
   git add .
   git commit -m "feat: add user profile page"
   ```

3. **Keep your branch updated**:

   ```bash
   git checkout develop
   git pull origin develop
   git checkout feature/your-feature-name
   git merge develop
   ```

4. **Push your branch**:

   ```bash
   git push origin feature/your-feature-name
   ```

5. **Create a Pull Request**:
   - Go to GitHub/GitLab
   - Create PR from your branch to `develop`
   - Request review from team members

### Commit Message Convention

Use [Conventional Commits](https://www.conventionalcommits.org/):

```bash
feat: add new feature
fix: fix bug in navigation
docs: update README
style: format code
refactor: restructure auth module
test: add unit tests
chore: update dependencies
```

**Examples:**

```bash
git commit -m "feat: implement task drag and drop"
git commit -m "fix: resolve menu active state bug"
git commit -m "refactor: move auth scripts to auth/ folder"
git commit -m "docs: add API documentation"
```

### Before Pushing

1. **Test your code** in the browser
2. **Check for console errors** (F12)
3. **Verify responsive design** (mobile view)
4. **Review your changes**: `git diff`

---

## 📝 Code Style & Best Practices

### JavaScript

#### 1. **File Organization**

```javascript
/**
 * File: board.js
 * Description: Board view management
 */

// Constants at top
const TASK_STATUS = {
  TODO: "todo",
  IN_PROGRESS: "in-progress",
  DONE: "done",
};

// Main functions
function init() {
  // Initialization
}

// Helper functions
function loadTasks() {
  // Implementation
}

// Event handlers
function handleTaskClick(event) {
  // Implementation
}

// Initialize on load
document.addEventListener("DOMContentLoaded", init);
```

#### 2. **Naming Conventions**

```javascript
// ✅ Good
const userName = "John";
const isActive = true;
function getUserById(id) {}
function handleSubmitClick(event) {}

// ❌ Bad
const n = "John";
const flag = true;
function get(id) {}
function click(event) {}
```

#### 3. **Async/Await**

```javascript
// ✅ Good
async function loadUserData() {
  try {
    const data = await getData("users");
    if (!data) {
      console.error("No data found");
      return;
    }
    renderUsers(data);
  } catch (error) {
    console.error("Error loading users:", error);
  }
}

// ❌ Bad
function loadUserData() {
  getData("users").then((data) => {
    renderUsers(data);
  });
}
```

#### 4. **DOM Manipulation**

```javascript
// ✅ Good - Cache selectors
const form = document.getElementById("taskForm");
const submitBtn = document.getElementById("submitBtn");

function handleSubmit() {
  submitBtn.disabled = true;
  // ...
}

// ❌ Bad - Repeated queries
function handleSubmit() {
  document.getElementById("submitBtn").disabled = true;
  document.getElementById("submitBtn").textContent = "Saving...";
}
```

#### 5. **Error Handling**

```javascript
// ✅ Good
async function saveTask(taskData) {
  try {
    const result = await postData("tasks", taskData);
    if (!result) {
      throw new Error("Failed to save task");
    }
    return result;
  } catch (error) {
    console.error("Error saving task:", error);
    showErrorMessage("Could not save task. Please try again.");
    return null;
  }
}
```

### CSS

#### 1. **BEM Naming** (for components)

```css
/* Block */
.menu {
}

/* Element */
.menu__header {
}
.menu__nav {
}
.menu__btn {
}

/* Modifier */
.menu__btn--active {
}
```

#### 2. **CSS Organization**

```css
/* Component: Task Card */

/* Layout */
.task-card {
  display: flex;
  flex-direction: column;
}

/* Spacing */
.task-card {
  padding: var(--spacing-md);
  gap: var(--spacing-sm);
}

/* Typography */
.task-card__title {
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-bold);
}

/* Colors */
.task-card {
  background: var(--color-white);
  border: 1px solid var(--color-gray-200);
}

/* States */
.task-card:hover {
  box-shadow: var(--shadow-md);
}

.task-card--active {
  border-color: var(--color-primary);
}
```

#### 3. **Responsive Design**

```css
/* Mobile first approach */
.container {
  padding: var(--spacing-sm);
}

/* Tablet */
@media (min-width: 768px) {
  .container {
    padding: var(--spacing-md);
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    padding: var(--spacing-lg);
    max-width: 1440px;
  }
}
```

### HTML

#### 1. **Semantic Markup**

```html
<!-- ✅ Good -->
<header class="header">
  <nav class="navigation">
    <ul class="nav-list">
      <li><a href="/">Home</a></li>
    </ul>
  </nav>
</header>

<main class="content">
  <article class="task-card">
    <h2 class="task-title">Task Title</h2>
  </article>
</main>

<!-- ❌ Bad -->
<div class="header">
  <div class="navigation">
    <div class="nav-list">
      <div><a href="/">Home</a></div>
    </div>
  </div>
</div>
```

#### 2. **Accessibility**

```html
<!-- ✅ Good - Accessible -->
<button
  class="menu__btn"
  aria-label="Open menu"
  aria-expanded="false"
  aria-haspopup="menu"
>
  <img src="icon.svg" alt="" />
  <span>Menu</span>
</button>

<div class="dropdown" inert aria-hidden="true">
  <a href="#" tabindex="-1">Item</a>
</div>

<!-- ❌ Bad - Not accessible -->
<div onclick="openMenu()">
  <img src="icon.svg" />
</div>
```

---

## 🐛 Debugging

### Browser DevTools

#### Console

```javascript
// Debug logging
console.log("User data:", userData);
console.error("Error:", error);
console.warn("Warning:", message);
console.table(arrayOfObjects);

// Conditional logging
const DEBUG = true;
if (DEBUG) {
  console.log("Debug info:", data);
}
```

#### Breakpoints

1. Open DevTools (F12)
2. Go to "Sources" tab
3. Find your JavaScript file
4. Click on line number to set breakpoint
5. Reload page, execution will pause at breakpoint

#### Network Tab

- Check Firebase requests
- Verify API responses
- Monitor load times

### Common Debugging Scenarios

#### 1. Template Not Loading

```javascript
// Check if includeHTML is available
if (typeof includeHTML !== "function") {
  console.error("include-html.js not loaded");
}

// Check DOM after template loads
document.addEventListener("DOMContentLoaded", () => {
  setTimeout(() => {
    const menu = document.getElementById("userMenu");
    console.log("Menu element:", menu);
  }, 500);
});
```

#### 2. Navigation Issues

```javascript
// Check current page detection
const currentPage = window.location.pathname.split("/").pop();
console.log("Current page:", currentPage);

// Check if menu exists
const menuBtn = document.getElementById("navSummary");
if (!menuBtn) {
  console.error("Menu button not found - template may not be loaded");
}
```

#### 3. Firebase Data Issues

```javascript
// Test Firebase connection
async function testFirebase() {
  try {
    const test = await getData("test");
    console.log("Firebase connection OK:", test);
  } catch (error) {
    console.error("Firebase error:", error);
  }
}
```

### VS Code Debugging

1. **Install Live Server** extension
2. **Use console.log** strategically
3. **Check Browser Console** for errors
4. **Use VS Code's built-in debugger** for more complex issues

---

## 🔧 Common Issues & Solutions

### Issue: Page Loads But Template Missing

**Symptoms**: Content shows but no header/sidebar

**Solution**:

1. Check browser console for errors
2. Verify template path in HTML: `w3-include-html="../assets/templates/layout.html"`
3. Ensure `include-html.js` is loaded before `init-template.js`
4. Check that template file exists

```html
<!-- Correct order -->
<script src="../scripts/shared/include-html.js" defer></script>
<script src="../scripts/shared/init-template.js" defer></script>
```

### Issue: Menu Active State Not Working

**Symptoms**: When navigating, wrong menu item highlighted

**Solution**:

1. Check filename matches exactly: `add-task.html` (not `add_task.html`)
2. Verify `menu.js` pageToButtonMap:

```javascript
const pageToButtonMap = {
  "summary.html": "navSummary",
  "add-task.html": "navAddTask", // Must match filename exactly
  // ...
};
```

### Issue: User Not Logged In / Redirected

**Symptoms**: Immediately redirected to login page

**Solution**:

```javascript
// Check session storage
const user = sessionStorage.getItem("loggedInUser");
console.log("Logged in user:", user);

// If null, user not logged in
if (!user) {
  console.log("No user session - login required");
}
```

### Issue: View Transition Errors (Fehlercode 5)

**Symptoms**: Error when clicking navigation links quickly

**Solution**: Already fixed! View Transitions are disabled for footer links:

```javascript
// view-transitions.js
if (link.classList.contains("menu__footer-link")) {
  return; // Skip View Transition for these links
}
```

### Issue: Accessibility Warning (aria-hidden)

**Symptoms**: Console warning about `aria-hidden` and focus

**Solution**: Use `inert` attribute instead:

```html
<!-- ✅ Correct -->
<div class="dropdown" inert aria-hidden="true">
  <a href="#">Link</a>
</div>
```

```javascript
// Toggle in JavaScript
menu.removeAttribute("inert"); // Open
menu.setAttribute("inert", ""); // Close
```

---

## 🤝 Team Communication

### Before Starting Work

1. Check existing issues/tasks
2. Discuss with team lead
3. Create/assign issue
4. Update project board

### During Development

- Commit frequently with clear messages
- Push regularly to share progress
- Ask questions in team chat
- Document complex solutions

### Code Review Guidelines

- Review code within 24 hours
- Be constructive and respectful
- Test changes locally
- Request changes if needed
- Approve when ready

---

## 📚 Additional Resources

### Project Documentation

- `NOTES.md` - Development notes
- `README.md` - Project overview
- `git-befehle.md` - Git commands reference

### External Resources

- [MDN Web Docs](https://developer.mozilla.org/) - Web API reference
- [CSS-Tricks](https://css-tricks.com/) - CSS guides
- [Firebase Documentation](https://firebase.google.com/docs) - Firebase guides
- [Conventional Commits](https://www.conventionalcommits.org/) - Commit standards

---

## 🎯 Quick Reference

### Start Development

```bash
# Start Live Server in VS Code
Right-click index.html → "Open with Live Server"
```

### Create Feature

```bash
git checkout develop
git pull origin develop
git checkout -b feature/my-feature
# ... make changes ...
git add .
git commit -m "feat: add my feature"
git push origin feature/my-feature
```

### Common Paths

- Pages: `pages/*.html`
- Scripts: `scripts/*.js`
- Styles: `styles/*.css`
- Template: `assets/templates/layout.html`
- Firebase: `scripts/firebase.js`

### Key Files

- **Template Init**: `scripts/shared/init-template.js`
- **Navigation**: `scripts/shared/menu.js`
- **Template Loader**: `scripts/shared/include-html.js`
- **CSS Variables**: `styles/base/variables.css`
- **Main Layout**: `styles/components/template.css`

---

## 🚀 Ready to Contribute!

You're all set! If you have questions:

1. Check this document first
2. Review existing code
3. Ask team members
4. Update this guide if you learned something new

**Happy coding! 🎉**
