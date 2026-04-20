---
prompt: architecture
project: join
category: structure
---

# Architecture - Join Project

## Application Type

### Multi-Page Application (MPA)

Join is built as a **Multi-Page Application (MPA)**, not a Single-Page Application (SPA).

**What this means:**

- Each page is a separate HTML file
- Navigation causes full page reloads
- No client-side routing
- No state management frameworks (Redux, Zustand, etc.)
- Each page handles its own state

```
✅ CORRECT: MPA Structure
pages/
├── index.html          → Login/Start page
├── board.html          → Kanban board
├── add-task.html       → Add task form
├── contacts.html       → Contact list
└── summary.html        → Dashboard

❌ WRONG: SPA with routing
src/
├── App.js
├── Router.js           → No client-side routing!
└── pages/              → No dynamic page loading!
```

---

## Tech Stack

### Frontend

| Technology             | Usage                                             |
| ---------------------- | ------------------------------------------------- |
| **Vanilla JavaScript** | ES6+ (no frameworks like React, Vue, Angular)     |
| **HTML5**              | Semantic HTML for all pages                       |
| **CSS3**               | Custom CSS with BEM methodology                   |
| **ES6 Modules**        | `import`/`export` for code organization           |
| **include-html.js**    | Custom template loader for shared HTML components |

### Backend

| Technology             | Usage                                      |
| ---------------------- | ------------------------------------------ |
| **Firebase Firestore** | NoSQL database for tasks, contacts, users  |
| **Firebase Auth**      | User authentication and session management |
| **Firebase Hosting**   | (Optional) Hosting solution                |

### Code Quality

| Tool      | Usage                                 |
| --------- | ------------------------------------- |
| **JSDoc** | Function documentation                |
| **Git**   | Version control with feature branches |

---

## Project Structure

```
join-mpa/
├── config/                     # Firebase configuration
│   └── firebase-config.js      # Firebase initialization
│
├── services/                   # Backend services
│   ├── auth.service.js         # Authentication logic
│   ├── firestore.service.js    # Firestore CRUD operations
│   └── data.service.js         # Data transformation layer
│
├── assets/                     # Static resources
│   ├── fonts/                  # Custom fonts
│   ├── img/                    # Images, icons, logos
│   └── templates/              # HTML templates (header, menu, etc.)
│       ├── header.html
│       ├── menu.html
│       └── footer.html
│
├── css/                        # Stylesheets
│   ├── base/                   # Global styles
│   │   ├── reset.css
│   │   ├── variables.css       # CSS custom properties
│   │   └── typography.css
│   ├── layout/                 # Layout components
│   │   ├── header.css
│   │   ├── menu.css
│   │   └── footer.css
│   ├── components/             # Reusable components
│   │   ├── button.css
│   │   ├── card.css
│   │   └── form.css
│   └── pages/                  # Page-specific styles
│       ├── board.css
│       ├── add-task.css
│       └── contacts.css
│
├── js/                         # JavaScript modules
│   ├── auth/                   # Authentication features
│   │   ├── auth__login.js
│   │   └── auth__register.js
│   ├── board/                  # Board features
│   │   ├── board__render.js
│   │   ├── board__drag.js
│   │   └── board__search.js
│   ├── tasks/                  # Task management
│   │   ├── task__create.js
│   │   ├── task__edit.js
│   │   └── task__delete.js
│   ├── contacts/               # Contact management
│   │   ├── contact__list.js
│   │   └── contact__form.js
│   ├── layout/                 # Layout components
│   │   ├── header__init.js
│   │   └── menu__navigation.js
│   └── shared/                 # Shared utilities
│       ├── include-html.js     # Template loader
│       ├── ui-helpers.js       # UI utility functions
│       └── validators.js       # Form validation
│
└── pages/                      # HTML pages
    ├── index.html              # Login page (must be named index.html!)
    ├── board.html              # Kanban board
    ├── add-task.html           # Add task page
    ├── contacts.html           # Contacts page
    └── summary.html            # Dashboard
```

---

## File Organization Principles

### 1. Feature-Based Organization

Group files by **feature**, not by file type.

```
✅ CORRECT: Feature-based
board/
├── board__render.js
├── board__drag.js
└── board__search.js

❌ WRONG: Type-based (for large features)
components/
├── BoardRender.js
├── BoardDrag.js
└── BoardSearch.js
```

### 2. Service Layer Pattern

Business logic and data access are separated into services.

```javascript
// services/task.service.js
import { db } from "../config/firebase-config.js";
import { collection, addDoc, getDocs } from "firebase/firestore";

/**
 * Creates a task in Firestore
 * @param {Object} taskData - Task data
 * @returns {Promise<string>} Task ID
 */
async function createTask(taskData) {
  const docRef = await addDoc(collection(db, "tasks"), taskData);
  return docRef.id;
}

/**
 * Loads all tasks from Firestore
 * @returns {Promise<Array>} Array of tasks
 */
async function loadTasks() {
  const snapshot = await getDocs(collection(db, "tasks"));
  return snapshot.docs.map((doc) => ({ id: doc.id, ...doc.data() }));
}

export { createTask, loadTasks };
```

### 3. Page-Specific Scripts

Each page has its own initialization script.

```html
<!-- board.html -->
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <title>Board - Join</title>
    <link rel="stylesheet" href="../css/board.css" />
  </head>
  <body>
    <!-- Page content -->

    <script type="module" src="../js/board/board__init.js"></script>
  </body>
</html>
```

```javascript
// js/board/board__init.js
import { loadTasks } from "../services/task.service.js";
import { renderBoard } from "./board__render.js";
import { initializeDragAndDrop } from "./board__drag.js";

/**
 * Initializes the board page
 */
async function init() {
  const tasks = await loadTasks();
  renderBoard(tasks);
  initializeDragAndDrop();
}

init();
```

---

## Module System

### ES6 Modules (import/export)

**All JavaScript files use ES6 modules.**

#### HTML Integration

```html
<!-- MUST use type="module" -->
<script type="module" src="../js/board/board__init.js"></script>
```

#### Import Statements

```javascript
// Named imports
import { createTask, deleteTask } from "../services/task.service.js";
import { renderBoard } from "./board__render.js";

// Import all
import * as taskService from "../services/task.service.js";
```

#### Export Statements

**Always export at the END of the file**

```javascript
// task.service.js

function createTask(data) {
  // Implementation
}

function deleteTask(id) {
  // Implementation
}

function updateTask(id, data) {
  // Implementation
}

// Export at the end
export { createTask, deleteTask, updateTask };
```

### ❌ NO window globals

```javascript
// ❌ WRONG: Attaching to window
window.createTask = createTask;
window.deleteTask = deleteTask;

// ✅ CORRECT: ES6 exports
export { createTask, deleteTask };
```

---

## Template Loading (include-html.js)

### Purpose

Shared HTML components (header, menu, footer) are loaded dynamically using a custom `include-html.js` script.

### Usage

```html
<!-- page.html -->
<div w3-include-html="../assets/templates/header.html"></div>
<div w3-include-html="../assets/templates/menu.html"></div>

<!-- Main content -->

<div w3-include-html="../assets/templates/footer.html"></div>

<script type="module">
  import { includeHTML } from "../js/shared/include-html.js";
  includeHTML();
</script>
```

### Implementation

```javascript
// js/shared/include-html.js

/**
 * Loads HTML templates into elements with w3-include-html attribute
 */
async function includeHTML() {
  const elements = document.querySelectorAll("[w3-include-html]");

  for (let element of elements) {
    const file = element.getAttribute("w3-include-html");
    const response = await fetch(file);

    if (response.ok) {
      element.innerHTML = await response.text();
    } else {
      element.innerHTML = "Template not found";
    }
  }
}

export { includeHTML };
```

---

## State Management

### ❌ NO State Management Libraries

**Join does NOT use state management frameworks** like Redux, Zustand, MobX, etc.

### ✅ Local State per Page

Each page manages its own state using:

- JavaScript variables
- DOM as source of truth
- LocalStorage for persistence (optional)

```javascript
// board__state.js

let currentTasks = [];
let currentFilter = "all";

/**
 * Sets tasks in local state
 * @param {Array} tasks - Array of tasks
 */
function setTasks(tasks) {
  currentTasks = tasks;
}

/**
 * Gets tasks from local state
 * @returns {Array} Array of tasks
 */
function getTasks() {
  return currentTasks;
}

/**
 * Sets current filter
 * @param {string} filter - Filter value
 */
function setFilter(filter) {
  currentFilter = filter;
}

export { setTasks, getTasks, setFilter };
```

---

## Routing

### ❌ NO Client-Side Routing

**Join uses traditional server-side routing (full page reloads).**

### Navigation

```html
<!-- ✅ CORRECT: Standard HTML links -->
<nav>
  <a href="board.html">Board</a>
  <a href="add-task.html">Add Task</a>
  <a href="contacts.html">Contacts</a>
  <a href="summary.html">Summary</a>
</nav>

<!-- ❌ WRONG: JavaScript routing -->
<nav>
  <a href="#" onclick="navigateTo('board')">Board</a>
</nav>
```

### Protected Routes

```javascript
// js/auth/auth__guard.js

import { getCurrentUser } from "../services/auth.service.js";

/**
 * Checks if user is authenticated, redirects to login if not
 */
async function checkAuth() {
  const user = await getCurrentUser();

  if (!user && window.location.pathname !== "/index.html") {
    window.location.href = "/index.html";
  }
}

checkAuth();
```

---

## Forbidden Patterns

### ❌ NO: React, Vue, Angular, Svelte

Join is **Vanilla JavaScript only**.

### ❌ NO: ES6 Classes

Use factory functions and pure functions instead.

### ❌ NO: Global Variables on window

Use ES6 modules and imports.

### ❌ NO: jQuery

Use native DOM APIs.

```javascript
// ❌ WRONG: jQuery
$("#button").click(() => {});

// ✅ CORRECT: Native DOM
document.getElementById("button").addEventListener("click", () => {});
```

### ❌ NO: Build Tools (Webpack, Vite, Rollup)

Join is served as static files. No bundling required.

### ❌ NO: TypeScript

Join uses **JavaScript** (ES6+).

---

## Checklist for New Features

Before implementing a new feature:

- [ ] Identify the page(s) affected
- [ ] Create feature-specific JS modules (if needed)
- [ ] Create page-specific CSS (if needed)
- [ ] Use ES6 imports/exports
- [ ] Add JSDoc comments
- [ ] Keep functions ≤ 14 lines
- [ ] Keep files ≤ 400 LOC
- [ ] NO frameworks, NO classes, NO window globals
- [ ] Test in latest Chrome, Firefox, Safari, Edge

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
