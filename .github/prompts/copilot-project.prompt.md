---
agent: agent
---

# GitHub Copilot Instructions - Join Vanilla JavaScript Project

> **⚠️ Note:** This prompt file has been replaced by a structured set of prompts in the `join/` folder.
> **See:** [join/README.md](join/README.md) for the complete documentation structure.

---

## 📁 New Documentation Structure

For detailed guidelines, refer to the following structured prompts:

1. **[Coding Standards](join/01-coding-standards.md)** - Function rules, JSDoc, ES6 modules
2. **[Architecture](join/02-architecture.md)** - MPA structure, service layer, project organization
3. **[Page Structure](join/03-page-structure.md)** - Semantic HTML, templates, forms
4. **[Styling & BEM](join/04-styling-bem.md)** - BEM methodology, responsive design
5. **[Firebase Integration](join/05-firebase-integration.md)** - Firestore, Auth, services
6. **[Feature: Kanban Board](join/06-feature-kanban-board.md)** - Board layout, drag & drop
7. **[Feature: Task Management](join/07-feature-task-management.md)** - CRUD operations, subtasks
8. **[Feature: User Auth](join/08-feature-user-auth.md)** - Login, register, summary
9. **[Feature: Contact Management](join/09-feature-contact-management.md)** - Contact list, CRUD
10. **[Quality Checklist](join/10-quality-checklist.md)** - Definition of Done, testing

---

## Quick Reference (Legacy)

### Technologie-Stack

### Frontend

- **Language**: Vanilla JavaScript (ES6+)
- **HTML**: Semantic HTML5
- **CSS**: Custom CSS (siehe BEM-CONVENTIONS.md)
- **Templating**: `include-html.js` für HTML-Template-Imports (w3-include-html)
- **Module**: ES6 Modules (import/export)
- **Architecture**: MPA (Multi-Page Application) - NO State Management!

### Backend/Database

- **Backend**: Firebase Firestore
- **Authentication**: Firebase Auth

### Build Tools

- **Documentation**: JSDoc
- **Version Control**: Git

### Code Conventions

- **Language**: English (code, comments, documentation)
- **Naming**: camelCase für Variables und Functions
- **CSS**: Siehe BEM-CONVENTIONS.md
- **No ES6 Classes**: Factory Functions, Pure Functions
- **Modules**: ES6 Modules mit `import`/`export`

---

## 🚫 Verboten

```javascript
// ❌ WRONG: Keine ES6 Classes
class TaskManager {}

// ❌ WRONG: Keine window globals
window.setItem = setItem;

// ❌ WRONG: Export bei jeder Funktion
export function createTask(data) {}

// ✅ CORRECT: ES6 Modules - Exports am Ende
// task.service.js
function createTask(data) {
  return { ...data, id: generateId() };
}

function deleteTask(id) {
  // ...
}

export { createTask, deleteTask };

// In anderen Files
import { createTask, deleteTask } from "./services/task.service.js";
```

---

## 🎯 Function Rules

- **Maximum 14 lines** per function
- **One task** per function
- **camelCase naming:** `getUserById`
- **Async/await** statt .then() chains
- **Error handling:** Try-catch für alle async functions
- **JSDoc comments:** Required für alle functions
- **Export am Ende:** `export { func1, func2 };`

---

## 📝 JSDoc Required

```javascript
/**
 * Loads current user from Firestore
 * @returns {Promise<Object|null>} User object or null
 */
async function loadCurrentUser() {
  // Implementation
}

/**
 * Creates new task in Firestore
 * @param {Object} taskData - Task data
 * @returns {Promise<string>} Task ID
 */
async function createTask(taskData) {
  // Implementation
}
```

## 📁 File Structure

```
js/
├── auth/
│   ├── auth__login.js
│   └── auth__register.js
├── layout/
│   ├── header__init.js
│   └── menu__navigation.js
├── shared/
│   ├── include-html.js
│   └── ui-helpers.js
└── services/
    ├── auth.service.js
    ├── data.service.js
    └── firestore.service.js
```

## 🔥 Firebase

- **Firestore**: Collections/Documents
- **Auth**: Firebase Authentication
- **Services**: auth.service.js, firestore.service.js, data.service.js

Weitere Details: Siehe Projektdokumentation

---
