---
prompt: coding-standards
project: join
category: standards
---

# Coding Standards - Join Project

## Language & Naming Conventions

### Code Language

- **ALL code must be in English** (variables, functions, comments, documentation)
- **User-facing content**: German (UI text, labels, messages)

### Naming Conventions

#### JavaScript

```javascript
// ✅ CORRECT: camelCase for variables and functions
const userName = "Max";
const taskList = [];

function getUserById(id) {}
function createNewTask(data) {}
function validateFormInput(input) {}

// ❌ WRONG: snake_case, PascalCase (except for constructors)
const user_name = "Max"; // ❌ WRONG
const TaskList = []; // ❌ WRONG
function GetUserById() {} // ❌ WRONG
```

#### CSS

- **BEM naming convention** - See [BEM-CONVENTIONS.md](../../skills/BEM-CONVENTIONS.md)
- Block: `.block-name`
- Element: `.block__element`
- Modifier: `.block--modifier`

#### File Naming

```
JavaScript:
  - Vanilla JS modules: feature-based (e.g., auth.service.js, task.service.js)
  - BEM-inspired for components: board__drag.js, contact__validation.js

CSS:
  - Feature-based: board.css, add-task.css, contacts.css

HTML:
  - Descriptive page names: board.html, contacts.html, add-task.html
```

---

## Function Rules

### Maximum Function Length: 14 Lines

**Every function must be ≤ 14 lines of code (excluding JSDoc comments)**

```javascript
// ✅ CORRECT: Function within 14 lines
/**
 * Creates a new task in Firestore
 * @param {Object} taskData - Task data
 * @returns {Promise<string>} Task ID
 */
async function createTask(taskData) {
  try {
    const docRef = await addDoc(collection(db, "tasks"), {
      ...taskData,
      createdAt: serverTimestamp(),
    });
    showToast("Task created successfully");
    return docRef.id;
  } catch (error) {
    handleError(error);
  }
}

// ❌ WRONG: Function exceeds 14 lines - REFACTOR!
async function createTaskAndAssign(taskData) {
  try {
    const docRef = await addDoc(collection(db, "tasks"), taskData);
    const taskId = docRef.id;
    const contacts = await getContacts();
    const assignedContacts = contacts.filter((c) =>
      taskData.assignedTo.includes(c.id),
    );
    for (let contact of assignedContacts) {
      await updateDoc(doc(db, "contacts", contact.id), {
        assignedTasks: arrayUnion(taskId),
      });
    }
    await sendNotifications(assignedContacts, taskId);
    showToast("Task created and assigned");
    return taskId;
  } catch (error) {
    handleError(error);
  }
}

// ✅ CORRECT: Refactored into smaller functions
async function createTaskAndAssign(taskData) {
  try {
    const taskId = await createTask(taskData);
    await assignContactsToTask(taskId, taskData.assignedTo);
    await notifyAssignedContacts(taskData.assignedTo, taskId);
    showToast("Task created and assigned");
    return taskId;
  } catch (error) {
    handleError(error);
  }
}
```

### One Task Per Function

Each function should do **one thing only** and do it well.

```javascript
// ✅ CORRECT: Separate functions for different tasks
function validateEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

function showValidationError(message) {
  const errorElement = document.getElementById("error-message");
  errorElement.textContent = message;
  errorElement.classList.add("error--visible");
}

function clearValidationError() {
  const errorElement = document.getElementById("error-message");
  errorElement.textContent = "";
  errorElement.classList.remove("error--visible");
}

// ❌ WRONG: Function does multiple things
function validateAndShowEmailError(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  const isValid = regex.test(email);
  const errorElement = document.getElementById("error-message");

  if (!isValid) {
    errorElement.textContent = "Invalid email";
    errorElement.classList.add("error--visible");
  } else {
    errorElement.textContent = "";
    errorElement.classList.remove("error--visible");
  }

  return isValid;
}
```

---

## File Size Limits

### Maximum Lines of Code per File: 400 LOC

**Each file must be ≤ 400 lines of code**

**If a file exceeds 400 LOC:**

- Split into multiple files
- Extract utilities into separate modules
- Create feature-specific submodules

```
Example:
Instead of:
  board.js (600 lines)

Use:
  board/
    ├── board__render.js (200 lines)
    ├── board__drag.js (150 lines)
    └── board__search.js (100 lines)
```

---

## JSDoc Documentation

**ALL functions must have JSDoc comments**

### Required for all Functions

```javascript
/**
 * Loads user from Firestore by ID
 * @param {string} userId - User ID
 * @returns {Promise<Object|null>} User object or null if not found
 */
async function loadUserById(userId) {
  // Implementation
}

/**
 * Validates task form inputs
 * @param {Object} formData - Form data object
 * @returns {boolean} True if valid, false otherwise
 */
function validateTaskForm(formData) {
  // Implementation
}

/**
 * Renders task card HTML
 * @param {Object} task - Task object
 * @param {string} task.id - Task ID
 * @param {string} task.title - Task title
 * @param {string} task.category - Task category
 * @returns {string} HTML string
 */
function renderTaskCard(task) {
  // Implementation
}
```

### JSDoc Structure

- **Description**: Brief summary of what the function does
- **@param**: All parameters with type and description
- **@returns**: Return type and description
- **@throws** (optional): If function can throw errors

---

## ES6 Modules

### ✅ YES: ES6 Modules with import/export

```javascript
// task.service.js

/**
 * Creates a new task
 * @param {Object} taskData - Task data
 * @returns {Promise<string>} Task ID
 */
async function createTask(taskData) {
  // Implementation
}

/**
 * Deletes a task by ID
 * @param {string} taskId - Task ID
 * @returns {Promise<void>}
 */
async function deleteTask(taskId) {
  // Implementation
}

/**
 * Updates task data
 * @param {string} taskId - Task ID
 * @param {Object} updates - Updated data
 * @returns {Promise<void>}
 */
async function updateTask(taskId, updates) {
  // Implementation
}

// Export at the END of the file
export { createTask, deleteTask, updateTask };
```

```javascript
// In another file
import { createTask, deleteTask, updateTask } from "./services/task.service.js";

await createTask({ title: "New Task" });
```

### ❌ NO: ES6 Classes

**Do NOT use ES6 Classes. Use factory functions and pure functions instead.**

```javascript
// ❌ WRONG: ES6 Classes
class TaskManager {
  constructor(tasks) {
    this.tasks = tasks;
  }

  addTask(task) {
    this.tasks.push(task);
  }
}

// ✅ CORRECT: Factory functions and pure functions
function createTaskManager(tasks = []) {
  return {
    tasks,
    addTask(task) {
      tasks.push(task);
    },
    getTasks() {
      return tasks;
    },
  };
}

// OR: Pure functions
function addTaskToList(tasks, newTask) {
  return [...tasks, newTask];
}
```

### ❌ NO: window globals

```javascript
// ❌ WRONG: window globals
window.createTask = createTask;
window.deleteTask = deleteTask;

// ✅ CORRECT: ES6 imports
import { createTask, deleteTask } from "./services/task.service.js";
```

---

## Error Handling

### All async functions must have try-catch

```javascript
// ✅ CORRECT: Error handling
async function loadUserData(userId) {
  try {
    const user = await getUserById(userId);
    renderUserProfile(user);
  } catch (error) {
    handleError(error);
    showToast("Failed to load user data");
  }
}

// ❌ WRONG: No error handling
async function loadUserData(userId) {
  const user = await getUserById(userId);
  renderUserProfile(user);
}
```

### Prefer async/await over .then() chains

```javascript
// ✅ CORRECT: async/await
async function loadData() {
  const tasks = await getTasks();
  const contacts = await getContacts();
  return { tasks, contacts };
}

// ❌ WRONG: .then() chains
function loadData() {
  return getTasks().then((tasks) =>
    getContacts().then((contacts) => ({ tasks, contacts })),
  );
}
```

---

## Code Formatting

### General Rules

- **2 empty lines** between functions
- **Indentation**: 2 spaces (no tabs)
- **Line length**: Max 100 characters (recommendation)
- **Trailing commas**: Use for multi-line objects/arrays

```javascript
/**
 * Function 1
 */
function firstFunction() {
  // Implementation
}

/**
 * Function 2
 */
function secondFunction() {
  // Implementation
}
```

### Object/Array Formatting

```javascript
// ✅ CORRECT: Multi-line with trailing commas
const task = {
  title: "New Task",
  description: "Task description",
  priority: "medium",
  assignedTo: ["user1", "user2"],
};

const columns = ["todo", "in-progress", "awaiting-feedback", "done"];

// ✅ CORRECT: Single line for short objects
const user = { id: "123", name: "Max" };
```

---

## Common Mistakes to Avoid

### ❌ Don't hardcode values

```javascript
// ❌ WRONG
if (priority === "high") {
  color = "#FF0000";
}

// ✅ CORRECT: Use constants
const PRIORITY_COLORS = {
  high: "#FF0000",
  medium: "#FFA800",
  low: "#00FF00",
};

if (priority === "high") {
  color = PRIORITY_COLORS.high;
}
```

### ❌ Don't use magic numbers

```javascript
// ❌ WRONG
if (taskList.length > 4) {
  showLoadMore();
}

// ✅ CORRECT
const MAX_VISIBLE_TASKS = 4;

if (taskList.length > MAX_VISIBLE_TASKS) {
  showLoadMore();
}
```

### ❌ Don't mutate parameters

```javascript
// ❌ WRONG
function updateTask(task, updates) {
  task.title = updates.title;
  task.description = updates.description;
  return task;
}

// ✅ CORRECT
function updateTask(task, updates) {
  return {
    ...task,
    ...updates,
  };
}
```

---

## Summary Checklist

Before committing code, verify:

- [ ] All functions ≤ 14 lines
- [ ] All files ≤ 400 lines of code
- [ ] All functions have JSDoc comments
- [ ] camelCase naming for JS variables/functions
- [ ] BEM naming for CSS classes
- [ ] ES6 modules (no classes, no window globals)
- [ ] Exports at end of file
- [ ] All async functions have try-catch
- [ ] No hardcoded values (use constants)
- [ ] 2 empty lines between functions
- [ ] Code in English, UI text in German

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
