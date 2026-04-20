---
prompt: feature-kanban-board
project: join
category: feature
---

# Feature: Kanban Board - Join Project

## Overview

The Kanban board is the central feature of Join, displaying tasks in four columns representing different workflow stages.

---

## User Stories

### User Story 1: Display Tasks on Kanban Board

**As a user, I want to see tasks displayed on a Kanban board.**

#### Acceptance Criteria

- [x] Board has **four columns**: ToDo, In Progress, Awaiting Feedback, Done
- [x] Empty columns show message "No tasks to do" (or similar)
- [x] Each task card displays:
  - Category
  - Title
  - Description preview
  - Assigned users (initials)
  - Priority indicator
- [x] Clicking a task opens full task details
- [x] Each column has a **"+" icon** to add new tasks directly to that column

#### Implementation

```javascript
// js/board/board__render.js

/**
 * Renders the Kanban board with all columns
 * @param {Array} tasks - Array of all tasks
 */
function renderBoard(tasks) {
  const columns = ["todo", "in-progress", "awaiting-feedback", "done"];

  columns.forEach((status) => {
    const columnTasks = tasks.filter((task) => task.status === status);
    renderColumn(status, columnTasks);
  });
}

/**
 * Renders tasks in a specific column
 * @param {string} status - Column status
 * @param {Array} tasks - Tasks for this column
 */
function renderColumn(status, tasks) {
  const columnElement = document.getElementById(`${status}-column`);

  if (tasks.length === 0) {
    columnElement.innerHTML = `
      <p class="board__empty">No tasks in ${formatStatus(status)}</p>
    `;
    return;
  }

  columnElement.innerHTML = tasks.map(renderTaskCard).join("");
}

/**
 * Renders a single task card
 * @param {Object} task - Task object
 * @returns {string} HTML string
 */
function renderTaskCard(task) {
  return `
    <article
      class="task-card"
      data-task-id="${task.id}"
      onclick="openTaskDetails('${task.id}')"
    >
      <div class="task-card__category task-card__category--${task.category}">
        ${task.category}
      </div>

      <h3 class="task-card__title">${task.title}</h3>

      <p class="task-card__description">
        ${truncateText(task.description, 60)}
      </p>

      ${renderSubtaskProgress(task.subtasks)}

      <div class="task-card__footer">
        ${renderAssignedUsers(task.assignedTo)}
        ${renderPriorityIcon(task.priority)}
      </div>
    </article>
  `;
}

export { renderBoard, renderColumn, renderTaskCard };
```

---

### User Story 2: Visualize Subtask Progress

**As a user, I want to see the progress of tasks with subtasks visualized on the board.**

#### Acceptance Criteria

- [x] Tasks with subtasks show a **progress bar** or indicator
- [x] Progress shows completed subtasks vs. total (e.g., "5 of 7 subtasks completed")
- [x] 100% complete tasks show full progress bar or different color
- [x] Hovering shows detailed subtask info

#### Implementation

```javascript
// js/board/board__subtasks.js

/**
 * Renders subtask progress bar
 * @param {Array} subtasks - Array of subtask objects
 * @returns {string} HTML string
 */
function renderSubtaskProgress(subtasks) {
  if (!subtasks || subtasks.length === 0) {
    return "";
  }

  const completed = subtasks.filter((st) => st.completed).length;
  const total = subtasks.length;
  const percentage = (completed / total) * 100;

  return `
    <div class="task-card__progress">
      <div class="task-card__progress-bar">
        <div
          class="task-card__progress-fill"
          style="width: ${percentage}%"
        ></div>
      </div>
      <span class="task-card__progress-text">
        ${completed}/${total} Subtasks
      </span>
    </div>
  `;
}

export { renderSubtaskProgress };
```

---

### User Story 3: Search for Tasks

**As a user, I want to search for specific tasks by title on the Kanban board.**

#### Acceptance Criteria

- [x] Search field filters tasks in **real-time**
- [x] Only tasks matching the search term (title or description) are shown
- [x] Empty search shows all tasks again
- [x] Message shown when no results found ("No results found")

#### Implementation

```javascript
// js/board/board__search.js

import { renderBoard } from "./board__render.js";

let allTasks = [];

/**
 * Initializes search functionality
 * @param {Array} tasks - All tasks
 */
function initializeSearch(tasks) {
  allTasks = tasks;

  const searchInput = document.getElementById("board-search");
  searchInput.addEventListener("input", handleSearch);
}

/**
 * Handles search input
 * @param {Event} event - Input event
 */
function handleSearch(event) {
  const searchTerm = event.target.value.toLowerCase().trim();

  if (searchTerm === "") {
    renderBoard(allTasks);
    return;
  }

  const filteredTasks = filterTasks(searchTerm);

  if (filteredTasks.length === 0) {
    showNoResultsMessage();
    return;
  }

  renderBoard(filteredTasks);
}

/**
 * Filters tasks by search term
 * @param {string} searchTerm - Search term
 * @returns {Array} Filtered tasks
 */
function filterTasks(searchTerm) {
  return allTasks.filter((task) => {
    return (
      task.title.toLowerCase().includes(searchTerm) ||
      task.description.toLowerCase().includes(searchTerm)
    );
  });
}

/**
 * Shows "no results" message
 */
function showNoResultsMessage() {
  const columns = document.querySelectorAll(".board__column");
  columns.forEach((column) => {
    column.innerHTML = '<p class="board__empty">No results found</p>';
  });
}

export { initializeSearch };
```

---

### User Story 7: Drag & Drop Task Movement

**As a user, I want to move tasks between columns using drag & drop (desktop and mobile).**

#### Acceptance Criteria

**Desktop:**

- [x] Tasks are draggable between columns
- [x] Visual feedback during drag (slight rotation, opacity change)
- [x] Columns highlight when task is dragged over them (dashed border)
- [x] Dropping task updates its status in Firestore
- [x] Movement is smooth without delay

**Mobile:**

- [x] Columns are displayed **vertically**
- [x] Long press ("long tap") to grab task OR
- [x] Small arrow icon in top-right opens popup menu to select destination column

#### Implementation (Desktop)

```javascript
// js/board/board__drag.js

import { updateTaskStatus } from "../services/task.service.js";

/**
 * Initializes drag and drop functionality
 */
function initializeDragAndDrop() {
  const taskCards = document.querySelectorAll(".task-card");
  const columns = document.querySelectorAll(".board__column");

  taskCards.forEach((card) => {
    card.setAttribute("draggable", "true");
    card.addEventListener("dragstart", handleDragStart);
    card.addEventListener("dragend", handleDragEnd);
  });

  columns.forEach((column) => {
    column.addEventListener("dragover", handleDragOver);
    column.addEventListener("dragleave", handleDragLeave);
    column.addEventListener("drop", handleDrop);
  });
}

/**
 * Handles drag start
 * @param {Event} event - Drag event
 */
function handleDragStart(event) {
  const taskId = event.target.dataset.taskId;
  event.dataTransfer.setData("taskId", taskId);
  event.target.classList.add("task-card--dragging");
}

/**
 * Handles drag end
 * @param {Event} event - Drag event
 */
function handleDragEnd(event) {
  event.target.classList.remove("task-card--dragging");
}

/**
 * Handles drag over column
 * @param {Event} event - Drag event
 */
function handleDragOver(event) {
  event.preventDefault();
  event.currentTarget.classList.add("board__column--highlight");
}

/**
 * Handles drag leave column
 * @param {Event} event - Drag event
 */
function handleDragLeave(event) {
  event.currentTarget.classList.remove("board__column--highlight");
}

/**
 * Handles drop on column
 * @param {Event} event - Drop event
 */
async function handleDrop(event) {
  event.preventDefault();
  event.currentTarget.classList.remove("board__column--highlight");

  const taskId = event.dataTransfer.getData("taskId");
  const newStatus = event.currentTarget.dataset.status;

  await updateTaskStatus(taskId, newStatus);
  window.location.reload();
}

export { initializeDragAndDrop };
```

#### CSS for Drag & Drop

```css
/* task-card.css */

.task-card {
  cursor: move;
  transition:
    transform 100ms ease,
    opacity 100ms ease;
}

.task-card--dragging {
  opacity: 0.5;
  transform: rotate(2deg);
}

.board__column {
  min-height: 200px;
  transition: background 100ms ease;
}

.board__column--highlight {
  background: var(--color-gray-light);
  border: 2px dashed var(--color-primary);
}
```

---

## Board Page Structure

```html
<!-- pages/board.html -->
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Board - Join</title>
    <link rel="stylesheet" href="../css/pages/board.css" />
  </head>
  <body>
    <div w3-include-html="../assets/templates/header.html"></div>
    <div w3-include-html="../assets/templates/menu.html"></div>

    <main class="board">
      <section class="board__header">
        <h1 class="board__title">Board</h1>
        <input
          type="text"
          id="board-search"
          class="board__search"
          placeholder="Find Task"
        />
        <button class="button button--primary" onclick="openAddTaskModal()">
          <svg class="button__icon"><!-- + icon --></svg>
          Add Task
        </button>
      </section>

      <section class="board__columns">
        <div class="board__column" id="todo-column" data-status="todo">
          <div class="board__column-header">
            <h2>To Do</h2>
            <button class="board__add-task" onclick="openAddTaskModal('todo')">
              <svg><!-- + icon --></svg>
            </button>
          </div>
        </div>

        <div
          class="board__column"
          id="in-progress-column"
          data-status="in-progress"
        >
          <div class="board__column-header">
            <h2>In Progress</h2>
            <button
              class="board__add-task"
              onclick="openAddTaskModal('in-progress')"
            >
              <svg><!-- + icon --></svg>
            </button>
          </div>
        </div>

        <div
          class="board__column"
          id="awaiting-feedback-column"
          data-status="awaiting-feedback"
        >
          <div class="board__column-header">
            <h2>Awaiting Feedback</h2>
            <button
              class="board__add-task"
              onclick="openAddTaskModal('awaiting-feedback')"
            >
              <svg><!-- + icon --></svg>
            </button>
          </div>
        </div>

        <div class="board__column" id="done-column" data-status="done">
          <div class="board__column-header">
            <h2>Done</h2>
            <button class="board__add-task" onclick="openAddTaskModal('done')">
              <svg><!-- + icon --></svg>
            </button>
          </div>
        </div>
      </section>
    </main>

    <script type="module" src="../js/board/board__init.js"></script>
  </body>
</html>
```

---

## Responsive Design (Mobile)

### Vertical Column Layout

```css
/* board.css */

.board__columns {
  display: flex;
  gap: 16px;
  padding: 16px;
}

/* Mobile: Vertical Stack */
@media (max-width: 767px) {
  .board__columns {
    flex-direction: column;
  }
}

/* Desktop: Horizontal Row */
@media (min-width: 768px) {
  .board__columns {
    flex-direction: row;
  }
}
```

---

## Checklist for Kanban Board

- [ ] Four columns: ToDo, In Progress, Awaiting Feedback, Done
- [ ] Empty columns show "No tasks" message
- [ ] Task cards show category, title, description, assigned users, priority
- [ ] Subtask progress visualized with progress bar
- [ ] Search functionality filters tasks in real-time
- [ ] "+" icon in each column adds task to that column
- [ ] Drag & drop works on desktop with visual feedback
- [ ] Columns are vertical on mobile
- [ ] Mobile drag alternative implemented (long press or popup menu)
- [ ] Task status updates in Firestore on drop

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
