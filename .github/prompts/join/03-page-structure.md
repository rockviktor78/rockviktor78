---
prompt: page-structure
project: join
category: structure
---

# Page Structure - Join Project

## HTML Page Requirements

### 1. Entry Point: index.html

**The start page MUST be named `index.html`** so it's loaded by default.

```
✅ CORRECT:
├── index.html          → Login page (default entry point)
├── board.html
├── add-task.html
└── contacts.html

❌ WRONG:
├── login.html          → Won't load by default!
├── board.html
└── contacts.html
```

---

## Semantic HTML5

### Use Semantic Elements

```html
<!-- ✅ CORRECT: Semantic HTML -->
<header>
  <nav>
    <a href="board.html">Board</a>
  </nav>
</header>

<main>
  <section class="board">
    <article class="task-card">
      <h3 class="task-card__title">Task Title</h3>
    </article>
  </section>
</main>

<footer>
  <p>&copy; 2026 Join</p>
</footer>

<!-- ❌ WRONG: Generic divs everywhere -->
<div class="header">
  <div class="nav">
    <a href="board.html">Board</a>
  </div>
</div>

<div class="main">
  <div class="board">
    <div class="task-card">
      <div class="title">Task Title</div>
    </div>
  </div>
</div>
```

### Semantic Element Usage

| Element     | Usage                                              |
| ----------- | -------------------------------------------------- |
| `<header>`  | Page header, navigation                            |
| `<nav>`     | Navigation links                                   |
| `<main>`    | Main content area (one per page)                   |
| `<section>` | Thematic grouping of content                       |
| `<article>` | Self-contained content (task cards, contact cards) |
| `<aside>`   | Sidebar, related content                           |
| `<footer>`  | Page footer                                        |
| `<form>`    | Forms (login, add task, add contact)               |
| `<button>`  | Clickable buttons (NOT `<a>` for actions)          |
| `<a>`       | Links to other pages (NOT for click actions)       |

---

## Standard Page Template

```html
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Board - Join</title>

    <!-- Base Styles -->
    <link rel="stylesheet" href="../css/base/reset.css" />
    <link rel="stylesheet" href="../css/base/variables.css" />
    <link rel="stylesheet" href="../css/base/typography.css" />

    <!-- Layout Styles -->
    <link rel="stylesheet" href="../css/layout/header.css" />
    <link rel="stylesheet" href="../css/layout/menu.css" />

    <!-- Component Styles -->
    <link rel="stylesheet" href="../css/components/button.css" />
    <link rel="stylesheet" href="../css/components/card.css" />

    <!-- Page-Specific Styles -->
    <link rel="stylesheet" href="../css/pages/board.css" />
  </head>
  <body>
    <!-- Header Template -->
    <div w3-include-html="../assets/templates/header.html"></div>

    <!-- Menu Template -->
    <div w3-include-html="../assets/templates/menu.html"></div>

    <!-- Main Content -->
    <main class="board">
      <section class="board__header">
        <h1 class="board__title">Board</h1>
        <input type="text" class="board__search" placeholder="Find Task" />
        <button class="button button--primary">Add Task</button>
      </section>

      <section class="board__columns">
        <!-- Board columns -->
      </section>
    </main>

    <!-- Footer Template -->
    <div w3-include-html="../assets/templates/footer.html"></div>

    <!-- Load Templates First -->
    <script type="module">
      import { includeHTML } from "../js/shared/include-html.js";
      await includeHTML();
    </script>

    <!-- Page-Specific Script -->
    <script type="module" src="../js/board/board__init.js"></script>
  </body>
</html>
```

---

## Template System (include-html.js)

### Purpose

Shared HTML components (header, menu, footer) are loaded into pages using `w3-include-html` attributes.

### Template Files

```
assets/templates/
├── header.html         → Page header with logo, user info
├── menu.html           → Navigation menu
└── footer.html         → Footer with legal links
```

### Example: Header Template

```html
<!-- assets/templates/header.html -->
<header class="header">
  <div class="header__logo">
    <img src="../assets/img/logo.svg" alt="Join Logo" />
  </div>

  <div class="header__user">
    <span class="header__user-name" id="user-name">Guest</span>
    <button class="header__logout" id="logout-btn">Logout</button>
  </div>
</header>
```

### Example: Menu Template

```html
<!-- assets/templates/menu.html -->
<nav class="menu">
  <a href="summary.html" class="menu__link">
    <svg class="menu__icon"><!-- icon --></svg>
    Summary
  </a>
  <a href="board.html" class="menu__link menu__link--active">
    <svg class="menu__icon"><!-- icon --></svg>
    Board
  </a>
  <a href="add-task.html" class="menu__link">
    <svg class="menu__icon"><!-- icon --></svg>
    Add Task
  </a>
  <a href="contacts.html" class="menu__link">
    <svg class="menu__icon"><!-- icon --></svg>
    Contacts
  </a>
</nav>
```

### Loading Templates

```javascript
// js/shared/include-html.js

/**
 * Loads HTML templates into elements with w3-include-html attribute
 */
async function includeHTML() {
  const elements = document.querySelectorAll("[w3-include-html]");

  for (let element of elements) {
    const file = element.getAttribute("w3-include-html");

    try {
      const response = await fetch(file);

      if (response.ok) {
        element.innerHTML = await response.text();
        element.removeAttribute("w3-include-html");
      } else {
        element.innerHTML = "Template not found";
      }
    } catch (error) {
      console.error(`Failed to load template: ${file}`, error);
    }
  }
}

export { includeHTML };
```

### Usage in Pages

```html
<!-- 1. Add template placeholder -->
<div w3-include-html="../assets/templates/header.html"></div>

<!-- 2. Load templates before page initialization -->
<script type="module">
  import { includeHTML } from "../js/shared/include-html.js";
  await includeHTML();
</script>

<!-- 3. Initialize page -->
<script type="module" src="../js/board/board__init.js"></script>
```

---

## Dynamic Content Rendering

### ❌ NO: Static HTML for dynamic content

**Do NOT write static HTML for dynamic data like tasks or contacts.**

```html
<!-- ❌ WRONG: Static task cards in HTML -->
<article class="task-card">
  <h3>Task 1</h3>
  <p>Description</p>
</article>
<article class="task-card">
  <h3>Task 2</h3>
  <p>Description</p>
</article>
```

### ✅ YES: Render dynamic content with JavaScript

```html
<!-- ✅ CORRECT: Container for dynamic content -->
<section class="board__column" id="todo-column">
  <!-- Tasks will be rendered here by JavaScript -->
</section>
```

```javascript
// js/board/board__render.js

/**
 * Renders task card HTML
 * @param {Object} task - Task object
 * @returns {string} HTML string
 */
function renderTaskCard(task) {
  return `
    <article class="task-card" data-task-id="${task.id}">
      <div class="task-card__category task-card__category--${task.category}">
        ${task.category}
      </div>
      <h3 class="task-card__title">${task.title}</h3>
      <p class="task-card__description">${task.description}</p>
      <div class="task-card__footer">
        <span class="task-card__priority task-card__priority--${task.priority}">
          ${task.priority}
        </span>
      </div>
    </article>
  `;
}

/**
 * Renders all tasks in a column
 * @param {Array} tasks - Array of tasks
 * @param {string} columnId - Column ID
 */
function renderTasksInColumn(tasks, columnId) {
  const column = document.getElementById(columnId);

  if (tasks.length === 0) {
    column.innerHTML = '<p class="board__empty">No tasks</p>';
    return;
  }

  column.innerHTML = tasks.map(renderTaskCard).join("");
}

export { renderTaskCard, renderTasksInColumn };
```

---

## Forms

### Form Structure

```html
<form class="form" id="add-task-form">
  <div class="form__group">
    <label for="task-title" class="form__label">Title *</label>
    <input
      type="text"
      id="task-title"
      class="form__input"
      placeholder="Enter a title"
      required
    />
    <span class="form__error" id="title-error"></span>
  </div>

  <div class="form__group">
    <label for="task-description" class="form__label">Description</label>
    <textarea
      id="task-description"
      class="form__textarea"
      placeholder="Enter a description"
      rows="4"
    ></textarea>
  </div>

  <div class="form__group">
    <label for="task-due-date" class="form__label">Due Date *</label>
    <input type="date" id="task-due-date" class="form__input" required />
  </div>

  <div class="form__actions">
    <button type="button" class="button button--secondary">Clear</button>
    <button type="submit" class="button button--primary">Create Task</button>
  </div>
</form>
```

### Form Validation

```javascript
// js/tasks/task__validation.js

/**
 * Validates task form inputs
 * @param {Object} formData - Form data
 * @returns {Object} Validation result
 */
function validateTaskForm(formData) {
  const errors = {};

  if (!formData.title || formData.title.trim().length === 0) {
    errors.title = "Title is required";
  }

  if (!formData.dueDate) {
    errors.dueDate = "Due date is required";
  }

  if (!formData.category) {
    errors.category = "Category is required";
  }

  return {
    isValid: Object.keys(errors).length === 0,
    errors,
  };
}

/**
 * Shows form validation error
 * @param {string} fieldId - Field ID
 * @param {string} message - Error message
 */
function showFormError(fieldId, message) {
  const errorElement = document.getElementById(`${fieldId}-error`);
  errorElement.textContent = message;
  errorElement.classList.add("form__error--visible");
}

/**
 * Clears form validation error
 * @param {string} fieldId - Field ID
 */
function clearFormError(fieldId) {
  const errorElement = document.getElementById(`${fieldId}-error`);
  errorElement.textContent = "";
  errorElement.classList.remove("form__error--visible");
}

export { validateTaskForm, showFormError, clearFormError };
```

### Form Submission

```javascript
// js/tasks/task__create.js

import { createTask } from "../services/task.service.js";
import { validateTaskForm, showFormError } from "./task__validation.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Handles task form submission
 * @param {Event} event - Submit event
 */
async function handleTaskFormSubmit(event) {
  event.preventDefault();

  const formData = getFormData();
  const validation = validateTaskForm(formData);

  if (!validation.isValid) {
    displayValidationErrors(validation.errors);
    return;
  }

  try {
    await createTask(formData);
    showToast("Task created successfully");
    window.location.href = "board.html";
  } catch (error) {
    showToast("Failed to create task");
  }
}

/**
 * Gets form data from inputs
 * @returns {Object} Form data
 */
function getFormData() {
  return {
    title: document.getElementById("task-title").value,
    description: document.getElementById("task-description").value,
    dueDate: document.getElementById("task-due-date").value,
    category: document.getElementById("task-category").value,
  };
}

export { handleTaskFormSubmit };
```

---

## UI Feedback

### User Interactions

**All UI interactions must provide feedback:**

- **Hover**: Change cursor, background, shadow
- **Click**: Visual response (button press effect)
- **Success**: Toast message, modal
- **Error**: Error message, toast
- **Loading**: Loading spinner, disabled button

### Toast Messages

```javascript
// js/shared/ui-helpers.js

/**
 * Shows a toast message
 * @param {string} message - Toast message
 * @param {string} type - Toast type (success, error, info)
 */
function showToast(message, type = "success") {
  const toast = document.createElement("div");
  toast.className = `toast toast--${type}`;
  toast.textContent = message;

  document.body.appendChild(toast);

  setTimeout(() => {
    toast.classList.add("toast--visible");
  }, 10);

  setTimeout(() => {
    toast.classList.remove("toast--visible");
    setTimeout(() => toast.remove(), 300);
  }, 3000);
}

export { showToast };
```

---

## Accessibility

### Required Attributes

- **`alt` text** for all images
- **`aria-label`** for icon-only buttons
- **`for` attribute** on labels matching input IDs
- **`role` attribute** for custom components

```html
<!-- ✅ CORRECT: Accessible form -->
<form>
  <label for="email">Email</label>
  <input type="email" id="email" aria-required="true" />

  <button type="submit" aria-label="Submit form">
    <svg><!-- icon --></svg>
    Submit
  </button>
</form>

<!-- ❌ WRONG: Missing labels and ARIA -->
<form>
  <input type="email" />
  <button>
    <svg><!-- icon --></svg>
  </button>
</form>
```

---

## Checklist for New Pages

- [ ] Page named `*.html` in `pages/` folder
- [ ] Entry point is `index.html`
- [ ] Semantic HTML5 elements used
- [ ] Templates loaded with `w3-include-html`
- [ ] Page-specific CSS in `css/pages/`
- [ ] Page-specific JS in `js/[feature]/`
- [ ] Forms have validation
- [ ] Dynamic content rendered with JavaScript
- [ ] User feedback (toast, loading states)
- [ ] Accessibility attributes (alt, aria-label, for)

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
