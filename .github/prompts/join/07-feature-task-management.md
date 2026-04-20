---
prompt: feature-task-management
project: join
category: feature
---

# Feature: Task Management - Join Project

## Overview

Task management includes creating, editing, and deleting tasks with all necessary details (title, description, due date, priority, category, assigned contacts, subtasks).

---

## User Stories

### User Story 4: Add Tasks

**As a user, I want to add tasks with all necessary details.**

#### Acceptance Criteria

- [x] "Add Task" option in main menu
- [x] Each column has **"+" icon** to add task directly to that column (pre-fills status)
- [x] "Add Task" icon next to search bar
- [x] Form includes:
  - **Title\*** (required)
  - **Description** (optional)
  - **Due Date\*** (required)
  - **Priority** (urgent, medium, low) - **Medium is pre-selected**
  - **Assigned to** (dropdown with contacts)
  - **Category\*** (required) - "Technical Task" or "User Story"
  - **Subtasks** (optional)
- [x] Submit button disabled until all required fields filled
- [x] Form validated before submission

#### Implementation

```javascript
// js/tasks/task__create.js

import { createTask } from "../services/task.service.js";
import {
  validateTaskForm,
  showFormError,
  clearFormError,
} from "./task__validation.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Initializes add task form
 * @param {string} defaultStatus - Pre-selected status (optional)
 */
function initializeAddTaskForm(defaultStatus = "todo") {
  const form = document.getElementById("add-task-form");
  const submitButton = document.getElementById("submit-task-btn");

  // Pre-select medium priority
  document.getElementById("priority-medium").checked = true;

  // Pre-fill status if provided
  if (defaultStatus) {
    document.getElementById("task-status").value = defaultStatus;
  }

  // Enable/disable submit button based on validation
  form.addEventListener("input", () => {
    submitButton.disabled = !isFormValid();
  });

  form.addEventListener("submit", handleFormSubmit);
}

/**
 * Checks if form is valid
 * @returns {boolean} True if valid
 */
function isFormValid() {
  const title = document.getElementById("task-title").value.trim();
  const dueDate = document.getElementById("task-due-date").value;
  const category = document.getElementById("task-category").value;

  return title.length > 0 && dueDate && category;
}

/**
 * Handles form submission
 * @param {Event} event - Submit event
 */
async function handleFormSubmit(event) {
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
    showToast("Failed to create task", "error");
  }
}

/**
 * Gets form data
 * @returns {Object} Form data
 */
function getFormData() {
  return {
    title: document.getElementById("task-title").value.trim(),
    description: document.getElementById("task-description").value.trim(),
    dueDate: document.getElementById("task-due-date").value,
    priority: document.querySelector('input[name="priority"]:checked').value,
    category: document.getElementById("task-category").value,
    assignedTo: getSelectedContacts(),
    subtasks: getSubtasks(),
    status: document.getElementById("task-status").value,
  };
}

export { initializeAddTaskForm };
```

---

### User Story 5: Add and Manage Subtasks

**As a user, I want to add, edit, and manage subtasks within a main task.**

#### Acceptance Criteria

- [x] Subtask input field in task form
- [x] Pressing **Enter** or clicking **checkmark icon** adds subtask
- [x] **"X" icon** clears input field without adding subtask
- [x] After adding subtask, input field clears automatically
- [x] Hovering over subtask shows **edit (pencil) icon** and **delete (trash) icon**
- [x] Pencil icon allows editing subtask text
- [x] Trash icon deletes subtask

#### Implementation

```javascript
// js/tasks/task__subtasks.js

let subtasks = [];

/**
 * Initializes subtask management
 */
function initializeSubtasks() {
  const input = document.getElementById("subtask-input");
  const addButton = document.getElementById("subtask-add-btn");
  const clearButton = document.getElementById("subtask-clear-btn");

  input.addEventListener("keypress", (event) => {
    if (event.key === "Enter") {
      event.preventDefault();
      addSubtask();
    }
  });

  addButton.addEventListener("click", addSubtask);
  clearButton.addEventListener("click", clearSubtaskInput);
}

/**
 * Adds a new subtask
 */
function addSubtask() {
  const input = document.getElementById("subtask-input");
  const text = input.value.trim();

  if (text.length === 0) return;

  subtasks.push({
    text,
    completed: false,
  });

  renderSubtasks();
  clearSubtaskInput();
}

/**
 * Clears subtask input
 */
function clearSubtaskInput() {
  document.getElementById("subtask-input").value = "";
}

/**
 * Renders subtask list
 */
function renderSubtasks() {
  const container = document.getElementById("subtask-list");

  container.innerHTML = subtasks
    .map(
      (subtask, index) => `
    <div class="subtask-item" data-index="${index}">
      <span class="subtask-item__text">${subtask.text}</span>
      <div class="subtask-item__actions">
        <button
          class="subtask-item__edit"
          onclick="editSubtask(${index})"
        >
          <svg><!-- pencil icon --></svg>
        </button>
        <button
          class="subtask-item__delete"
          onclick="deleteSubtask(${index})"
        >
          <svg><!-- trash icon --></svg>
        </button>
      </div>
    </div>
  `,
    )
    .join("");
}

/**
 * Edits a subtask
 * @param {number} index - Subtask index
 */
function editSubtask(index) {
  const newText = prompt("Edit subtask:", subtasks[index].text);

  if (newText && newText.trim().length > 0) {
    subtasks[index].text = newText.trim();
    renderSubtasks();
  }
}

/**
 * Deletes a subtask
 * @param {number} index - Subtask index
 */
function deleteSubtask(index) {
  subtasks.splice(index, 1);
  renderSubtasks();
}

/**
 * Gets all subtasks
 * @returns {Array} Array of subtasks
 */
function getSubtasks() {
  return subtasks;
}

export { initializeSubtasks, getSubtasks };
```

---

### User Story 6: Edit and Delete Tasks

**As a user, I want to edit or delete tasks from the task detail view.**

#### Acceptance Criteria

- [x] Clicking a task opens **detail view** (modal or separate page)
- [x] Detail view shows all task information
- [x] **Pencil icon** activates edit mode
- [x] In edit mode, all fields can be changed **EXCEPT category**
- [x] Changes can be **saved** or **discarded**
- [x] **Trash icon** deletes task permanently
- [x] Deleted tasks no longer appear on board

#### Implementation

```javascript
// js/tasks/task__details.js

import { loadTaskById } from "../services/task.service.js";

/**
 * Opens task detail modal
 * @param {string} taskId - Task ID
 */
async function openTaskDetails(taskId) {
  const task = await loadTaskById(taskId);

  if (!task) {
    showToast("Task not found", "error");
    return;
  }

  renderTaskDetails(task);
  showModal("task-detail-modal");
}

/**
 * Renders task details
 * @param {Object} task - Task object
 */
function renderTaskDetails(task) {
  const modal = document.getElementById("task-detail-modal");

  modal.innerHTML = `
    <div class="modal__content">
      <div class="modal__header">
        <h2>${task.title}</h2>
        <button class="modal__close" onclick="closeModal()">×</button>
      </div>

      <div class="modal__body">
        <div class="task-detail__category">${task.category}</div>
        <p class="task-detail__description">${task.description}</p>

        <div class="task-detail__info">
          <strong>Due Date:</strong> ${formatDate(task.dueDate)}
        </div>

        <div class="task-detail__info">
          <strong>Priority:</strong>
          <span class="priority-badge priority-badge--${task.priority}">
            ${task.priority}
          </span>
        </div>

        <div class="task-detail__info">
          <strong>Assigned to:</strong>
          ${renderAssignedContacts(task.assignedTo)}
        </div>

        ${renderSubtaskList(task.subtasks)}
      </div>

      <div class="modal__actions">
        <button
          class="button button--secondary"
          onclick="deleteTask('${task.id}')"
        >
          <svg><!-- trash icon --></svg>
          Delete
        </button>
        <button
          class="button button--primary"
          onclick="editTask('${task.id}')"
        >
          <svg><!-- pencil icon --></svg>
          Edit
        </button>
      </div>
    </div>
  `;
}

export { openTaskDetails };
```

```javascript
// js/tasks/task__edit.js

import { updateTask } from "../services/task.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Opens task in edit mode
 * @param {string} taskId - Task ID
 */
async function editTask(taskId) {
  const task = await loadTaskById(taskId);
  renderEditForm(task);
}

/**
 * Saves task changes
 * @param {string} taskId - Task ID
 */
async function saveTaskChanges(taskId) {
  const updates = getFormData();

  // Remove category from updates (not editable)
  delete updates.category;

  try {
    await updateTask(taskId, updates);
    showToast("Task updated successfully");
    closeModal();
    window.location.reload();
  } catch (error) {
    showToast("Failed to update task", "error");
  }
}

export { editTask, saveTaskChanges };
```

```javascript
// js/tasks/task__delete.js

import { deleteTask as deleteTaskFromDB } from "../services/task.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Deletes a task
 * @param {string} taskId - Task ID
 */
async function deleteTask(taskId) {
  const confirmed = confirm("Are you sure you want to delete this task?");

  if (!confirmed) return;

  try {
    await deleteTaskFromDB(taskId);
    showToast("Task deleted successfully");
    closeModal();
    window.location.reload();
  } catch (error) {
    showToast("Failed to delete task", "error");
  }
}

export { deleteTask };
```

---

## Form Validation

### Task Form Validation

```javascript
// js/tasks/task__validation.js

/**
 * Validates task form data
 * @param {Object} formData - Form data
 * @returns {Object} Validation result
 */
function validateTaskForm(formData) {
  const errors = {};

  // Title required
  if (!formData.title || formData.title.trim().length === 0) {
    errors.title = "Title is required";
  }

  if (formData.title.length > 100) {
    errors.title = "Title must be less than 100 characters";
  }

  // Due date required
  if (!formData.dueDate) {
    errors.dueDate = "Due date is required";
  }

  // Due date must be in the future
  const dueDate = new Date(formData.dueDate);
  const today = new Date();
  today.setHours(0, 0, 0, 0);

  if (dueDate < today) {
    errors.dueDate = "Due date must be today or in the future";
  }

  // Category required
  if (!formData.category) {
    errors.category = "Category is required";
  }

  // Priority required
  if (!formData.priority) {
    errors.priority = "Priority is required";
  }

  return {
    isValid: Object.keys(errors).length === 0,
    errors,
  };
}

/**
 * Shows validation error
 * @param {string} fieldId - Field ID
 * @param {string} message - Error message
 */
function showFormError(fieldId, message) {
  const errorElement = document.getElementById(`${fieldId}-error`);

  if (errorElement) {
    errorElement.textContent = message;
    errorElement.classList.add("form__error--visible");
  }
}

/**
 * Clears validation error
 * @param {string} fieldId - Field ID
 */
function clearFormError(fieldId) {
  const errorElement = document.getElementById(`${fieldId}-error`);

  if (errorElement) {
    errorElement.textContent = "";
    errorElement.classList.remove("form__error--visible");
  }
}

export { validateTaskForm, showFormError, clearFormError };
```

---

## Assigned Contacts Dropdown

```javascript
// js/tasks/task__assign.js

/**
 * Initializes assigned contacts dropdown
 * @param {Array} contacts - All contacts
 */
function initializeContactDropdown(contacts) {
  const dropdown = document.getElementById("assigned-to-dropdown");
  const button = document.getElementById("assigned-to-btn");

  button.addEventListener("click", () => {
    dropdown.classList.toggle("dropdown--open");
  });

  // Close dropdown when clicking outside
  document.addEventListener("click", (event) => {
    if (!button.contains(event.target) && !dropdown.contains(event.target)) {
      dropdown.classList.remove("dropdown--open");
    }
  });

  renderContactOptions(contacts);
}

/**
 * Renders contact options in dropdown
 * @param {Array} contacts - All contacts
 */
function renderContactOptions(contacts) {
  const dropdown = document.getElementById("assigned-to-dropdown");

  dropdown.innerHTML = contacts
    .map(
      (contact) => `
    <label class="dropdown__option">
      <input
        type="checkbox"
        value="${contact.id}"
        class="dropdown__checkbox"
      >
      <div class="dropdown__contact">
        <div
          class="contact-avatar"
          style="background: ${contact.color}"
        >
          ${contact.initials}
        </div>
        <span>${contact.name}</span>
      </div>
    </label>
  `,
    )
    .join("");
}

/**
 * Gets selected contact IDs
 * @returns {Array} Array of contact IDs
 */
function getSelectedContacts() {
  const checkboxes = document.querySelectorAll(".dropdown__checkbox:checked");
  return Array.from(checkboxes).map((cb) => cb.value);
}

export { initializeContactDropdown, getSelectedContacts };
```

---

## Task Detail View (Example HTML)

```html
<!-- Modal for task details -->
<div id="task-detail-modal" class="modal" style="display: none;">
  <!-- Content dynamically rendered here -->
</div>
```

---

## Checklist for Task Management

- [ ] Add task form with all required fields
- [ ] Medium priority pre-selected by default
- [ ] Submit button disabled until all required fields filled
- [ ] Form validation before submission
- [ ] Subtasks can be added with Enter key
- [ ] Subtasks can be edited and deleted
- [ ] Clicking task opens detail view
- [ ] Edit mode allows changing all fields except category
- [ ] Delete task requires confirmation
- [ ] Deleted tasks removed from Firestore and board
- [ ] Assigned contacts dropdown closes when clicking outside
- [ ] User feedback (toast messages) for create, edit, delete

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
