# Styling Guidelines für Join Project

## BEM Methodik für CSS-Klassennamen

Dieses Projekt verwendet die **BEM (Block Element Modifier)** Namenskonvention für CSS-Klassen.

### BEM Grundlagen

#### Block

Ein eigenständiger, wiederverwendbarer Komponente:

```css
.header {
}
.menu {
}
.task-card {
}
.contact-list {
}
```

#### Element

Ein Teil eines Blocks, der keine eigenständige Bedeutung hat (mit `__`):

```css
.header__title {
}
.header__actions {
}
.header__help-btn {
}
.task-card__title {
}
.task-card__description {
}
.contact-list__item {
}
```

#### Modifier

Eine Variation oder ein Status eines Blocks oder Elements (mit `--`):

```css
.header--fixed {
}
.button--primary {
}
.button--disabled {
}
.task-card--urgent {
}
.task-card--completed {
}
```

---

## Namenskonventionen für dieses Projekt

### 1. Komponenten-Blöcke

```css
.main-header {
}
.main-sidebar {
}
.task-form {
}
.contact-card {
}
.board-column {
}
```

### 2. Elemente innerhalb von Komponenten

```css
.main-header__content {
}
.main-header__title {
}
.main-header__actions {
}
.main-header__help-btn {
}
.main-header__user-avatar {
}

.task-form__field {
}
.task-form__label {
}
.task-form__input {
}
.task-form__submit-btn {
}

.board-column__header {
}
.board-column__title {
}
.board-column__body {
}
.board-column__add-btn {
}
```

### 3. Modifier für Zustände und Varianten

```css
.button--primary {
}
.button--secondary {
}
.button--danger {
}
.button--disabled {
}

.task-card--urgent {
}
.task-card--low-priority {
}
.task-card--completed {
}

.nav-item--active {
}
.input--error {
}
.modal--open {
}
```

---

## Best Practices

### ✅ DO (Richtig)

```html
<!-- Block mit Elementen -->
<header class="main-header">
  <div class="main-header__content">
    <h1 class="main-header__title">Kanban Project Management Tool</h1>
    <div class="main-header__actions">
      <button class="main-header__help-btn">Help</button>
    </div>
  </div>
</header>

<!-- Task Card mit Modifier -->
<div class="task-card task-card--urgent">
  <h3 class="task-card__title">Task Title</h3>
  <p class="task-card__description">Description</p>
  <div class="task-card__footer">
    <button class="task-card__edit-btn">Edit</button>
  </div>
</div>
```

### ❌ DON'T (Falsch)

```html
<!-- Verschachtelte Klassen vermeiden -->
<div class="header">
  <div class="header__content__wrapper">
    ❌ Keine Mehrfachverschachtelung
    <h1 class="headerTitle">...</h1>
    ❌ Kein camelCase
  </div>
</div>

<!-- Zu allgemeine Klassen -->
<div class="left">...</div>
❌ Nicht aussagekräftig
<div class="item">...</div>
❌ Zu generisch
```

---

## JavaScript Integration

Bei JavaScript-Interaktionen verwende `id` oder `data-*` Attribute, nicht BEM-Klassen:

```html
<!-- Gut -->
<button class="task-card__delete-btn" id="deleteTask" data-task-id="123">
  Delete
</button>

<div class="user-avatar" id="userAvatar" data-user="SM">SM</div>
```

```javascript
// JavaScript
document.getElementById('deleteTask').addEventListener('click', ...);
document.querySelector('[data-task-id="123"]').remove();
```

---

## Projekt-spezifische Komponenten

### Header

```css
.main-header {
}
.main-header__content {
}
.main-header__title {
}
.main-header__actions {
}
.main-header__help-btn {
}
.main-header__user-avatar {
}
```

**Beispiel:**

```html
<header class="main-header">
  <div class="main-header__content">
    <h1 class="main-header__title">Kanban Project Management Tool</h1>
    <div class="main-header__actions">
      <button class="main-header__help-btn">?</button>
      <div class="main-header__user-avatar">SM</div>
    </div>
  </div>
</header>
```

### Sidebar/Menu

```css
.main-sidebar {
}
.main-sidebar__logo {
}
.main-sidebar__nav {
}
.main-sidebar__nav-item {
}
.main-sidebar__nav-item--active {
}
.main-sidebar__footer {
}
```

**Beispiel:**

```html
<aside class="main-sidebar">
  <div class="main-sidebar__logo">JOIN</div>
  <nav class="main-sidebar__nav">
    <a href="#" class="main-sidebar__nav-item main-sidebar__nav-item--active"
      >Summary</a
    >
    <a href="#" class="main-sidebar__nav-item">Board</a>
    <a href="#" class="main-sidebar__nav-item">Add Task</a>
    <a href="#" class="main-sidebar__nav-item">Contacts</a>
  </nav>
  <div class="main-sidebar__footer">
    <a href="#">Privacy Policy</a>
    <a href="#">Legal Notice</a>
  </div>
</aside>
```

### Task Components

```css
/* Task Card */
.task-card {
}
.task-card--urgent {
}
.task-card--medium {
}
.task-card--low {
}
.task-card--completed {
}
.task-card__category {
}
.task-card__title {
}
.task-card__description {
}
.task-card__assignees {
}
.task-card__assignee {
}
.task-card__priority-badge {
}
.task-card__progress {
}
.task-card__footer {
}

/* Task Form */
.task-form {
}
.task-form__field {
}
.task-form__label {
}
.task-form__input {
}
.task-form__textarea {
}
.task-form__select {
}
.task-form__dropdown {
}
.task-form__subtasks {
}
.task-form__subtask-item {
}
.task-form__priority-buttons {
}
.task-form__submit-btn {
}
.task-form__submit-btn--disabled {
}
```

**Beispiel Task Card:**

```html
<div class="task-card task-card--urgent">
  <span class="task-card__category">Technical Task</span>
  <h3 class="task-card__title">Implement Login</h3>
  <p class="task-card__description">Create login page with validation...</p>

  <div class="task-card__progress">
    <div class="progress-bar">
      <div class="progress-bar__fill" style="width: 60%;"></div>
    </div>
    <span class="progress-text">3/5 Subtasks</span>
  </div>

  <div class="task-card__footer">
    <div class="task-card__assignees">
      <div class="task-card__assignee">SM</div>
      <div class="task-card__assignee">JD</div>
    </div>
    <div class="task-card__priority-badge task-card__priority-badge--urgent">
      <img src="urgent-icon.svg" alt="Urgent" />
    </div>
  </div>
</div>
```

### Board Components

```css
.board {
}
.board__header {
}
.board__search {
}
.board__add-btn {
}
.board__columns {
}
.board__column {
}
.board__column--todo {
}
.board__column--in-progress {
}
.board__column--awaiting-feedback {
}
.board__column--done {
}
.board__column-header {
}
.board__column-title {
}
.board__column-count {
}
.board__column-add-btn {
}
.board__column-body {
}
.board__column-empty {
}
```

**Beispiel:**

```html
<div class="board">
  <div class="board__header">
    <input type="text" class="board__search" placeholder="Find Task" />
    <button class="board__add-btn">Add Task</button>
  </div>

  <div class="board__columns">
    <div class="board__column board__column--todo">
      <div class="board__column-header">
        <h2 class="board__column-title">To do</h2>
        <button class="board__column-add-btn">+</button>
      </div>
      <div class="board__column-body">
        <!-- Task Cards hier -->
      </div>
    </div>

    <div class="board__column board__column--in-progress">
      <!-- ... -->
    </div>
  </div>
</div>
```

### Contact Components

```css
.contact-list {
}
.contact-list__letter-group {
}
.contact-list__letter {
}
.contact-list__items {
}
.contact-list__item {
}
.contact-list__item--active {
}

.contact-card {
}
.contact-card__header {
}
.contact-card__avatar {
}
.contact-card__info {
}
.contact-card__name {
}
.contact-card__email {
}
.contact-card__phone {
}
.contact-card__actions {
}
.contact-card__edit-btn {
}
.contact-card__delete-btn {
}
```

**Beispiel:**

```html
<div class="contact-list">
  <div class="contact-list__letter-group">
    <h3 class="contact-list__letter">S</h3>
    <div class="contact-list__items">
      <div class="contact-list__item contact-list__item--active">
        <div class="contact-card__avatar">SM</div>
        <div class="contact-card__info">
          <h4 class="contact-card__name">Sofia Müller</h4>
          <p class="contact-card__email">sofia@example.com</p>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Detail View -->
<div class="contact-card">
  <div class="contact-card__header">
    <div class="contact-card__avatar">SM</div>
    <div class="contact-card__info">
      <h2 class="contact-card__name">Sofia Müller</h2>
    </div>
  </div>
  <p class="contact-card__email">sofia@example.com</p>
  <p class="contact-card__phone">+49 123 456789</p>
  <div class="contact-card__actions">
    <button class="contact-card__edit-btn">Edit</button>
    <button class="contact-card__delete-btn">Delete</button>
  </div>
</div>
```

### Login/Auth Components

```css
.login-page {
}
.login-form {
}
.login-form__logo {
}
.login-form__title {
}
.login-form__field {
}
.login-form__input {
}
.login-form__input--error {
}
.login-form__error-message {
}
.login-form__checkbox {
}
.login-form__button {
}
.login-form__button--primary {
}
.login-form__button--secondary {
}
.login-form__link {
}
```

### Summary/Dashboard Components

```css
.summary {
}
.summary__greeting {
}
.summary__metrics {
}
.summary__metric-card {
}
.summary__metric-card--urgent {
}
.summary__metric-icon {
}
.summary__metric-value {
}
.summary__metric-label {
}
```

---

## Design Tokens

Verwende CSS Custom Properties für konsistente Styles:

```css
:root {
  /* Colors */
  --color-primary: #4589ff;
  --color-dark: #2a3647;
  --color-light: #cdcdcd;
  --color-white: #ffffff;
  --color-background: #f6f7f8;

  /* Priority Colors */
  --color-urgent: #ff3d00;
  --color-medium: #ffa800;
  --color-low: #7ae229;

  /* Shadows */
  --shadow-card: 0 2px 4px rgba(0, 0, 0, 0.1);
  --shadow-hover: 0 4px 8px rgba(0, 0, 0, 0.15);
  --shadow-modal: 0 8px 16px rgba(0, 0, 0, 0.2);

  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 40px;

  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
  --radius-full: 50%;

  /* Transitions */
  --transition-fast: 75ms ease;
  --transition-base: 100ms ease;
  --transition-slow: 125ms ease;

  /* Font Sizes */
  --font-sm: 12px;
  --font-base: 16px;
  --font-lg: 20px;
  --font-xl: 24px;
  --font-2xl: 32px;
}
```

**Verwendung:**

```css
.task-card {
  background: var(--color-white);
  box-shadow: var(--shadow-card);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  transition: all var(--transition-base);
}

.task-card:hover {
  box-shadow: var(--shadow-hover);
}

.task-card--urgent {
  border-left: 4px solid var(--color-urgent);
}
```

---

## Responsives Design

### Breakpoints

```css
/* Mobile First Approach */
/* Mobile: 320px - 767px (Standard) */
.main-header {
  height: 60px;
  padding: 0 var(--spacing-md);
}

/* Tablet: 768px - 1023px */
@media (min-width: 768px) {
  .main-header {
    height: 70px;
    padding: 0 var(--spacing-lg);
  }
}

/* Desktop: 1024px+ */
@media (min-width: 1024px) {
  .main-header {
    height: 80px;
    padding: 0 var(--spacing-xl);
    left: 200px; /* Space for sidebar */
  }
}

/* Large Desktop: max-width für Content */
@media (min-width: 1440px) {
  .board__columns {
    max-width: 1440px;
    margin: 0 auto;
  }
}
```

### Modifier vermeiden

❌ **Falsch:**

```css
.main-header--mobile {
}
.main-header--tablet {
}
.main-header--desktop {
}
```

✅ **Richtig:**

```css
.main-header {
  /* Mobile First */
  height: 60px;
}

@media (min-width: 768px) {
  .main-header {
    height: 70px;
  }
}

@media (min-width: 1024px) {
  .main-header {
    height: 80px;
  }
}
```

---

## Utility Classes

Für häufig verwendete Styles können Utility-Klassen verwendet werden (ohne BEM):

```css
/* Layout */
.d-flex {
  display: flex;
}
.d-grid {
  display: grid;
}
.d-none {
  display: none;
}

/* Flexbox */
.flex-column {
  flex-direction: column;
}
.flex-wrap {
  flex-wrap: wrap;
}
.justify-center {
  justify-content: center;
}
.justify-between {
  justify-content: space-between;
}
.align-center {
  align-items: center;
}

/* Spacing */
.gap-8 {
  gap: 8px;
}
.gap-16 {
  gap: 16px;
}
.gap-24 {
  gap: 24px;
}

.mt-8 {
  margin-top: 8px;
}
.mt-16 {
  margin-top: 16px;
}
.mb-16 {
  margin-bottom: 16px;
}
.p-16 {
  padding: 16px;
}

/* Text */
.text-center {
  text-align: center;
}
.text-right {
  text-align: right;
}
.font-bold {
  font-weight: 700;
}

/* Colors */
.text-primary {
  color: var(--color-primary);
}
.text-urgent {
  color: var(--color-urgent);
}
.bg-primary {
  background-color: var(--color-primary);
}
```

**Verwendung:**

```html
<div class="task-card d-flex flex-column gap-16">
  <h3 class="task-card__title font-bold">Task Title</h3>
  <p class="task-card__description text-center">Description</p>
</div>
```

Diese sollten in einer separaten `utilities.css` Datei liegen.

---

## UI/UX Requirements

### Interaktive Elemente

```css
/* Buttons */
.button {
  cursor: pointer;
  border: unset;
  transition: all var(--transition-base);
}

.button:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-hover);
}

.button--disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

/* Inputs */
.input {
  border: unset;
  border-bottom: 2px solid var(--color-light);
  transition: border-color var(--transition-base);
}

.input:focus {
  outline: none;
  border-color: var(--color-primary);
}

.input--error {
  border-color: var(--color-urgent);
}

/* Links */
.link {
  transition: color var(--transition-base);
}

.link:hover {
  color: var(--color-primary);
}
```

### Transitions

Alle anklickbaren Elemente: **75ms - 125ms**

```css
.nav-item {
  transition: all 100ms ease;
}

.task-card {
  transition:
    transform 100ms ease,
    box-shadow 100ms ease;
}

.button {
  transition: all 75ms ease;
}
```

### Overflow Handling

```css
/* Text Overflow */
.task-card__title {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.task-card__description {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
}

/* Scrollable Content */
.board__column-body {
  overflow-y: auto;
  max-height: calc(100vh - 200px);
}

/* Hide Scrollbar (optional) */
.board__column-body::-webkit-scrollbar {
  width: 6px;
}

.board__column-body::-webkit-scrollbar-thumb {
  background: var(--color-light);
  border-radius: 3px;
}
```

---

## Quick Reference - BEM Cheat Sheet

```css
/* Login/Auth */
.login-page {
}
.login-form {
}
.login-form__input {
}
.login-form__button--primary {
}

/* Task Card */
.task-card {
}
.task-card--urgent {
}
.task-card__title {
}

/* Board */
.board__column {
}
.board__column--todo {
}
.board__column-empty {
}

/* Contact */
.contact-card {
}
.contact-card__avatar {
}
.contact-list__letter-group {
}

/* Summary */
.summary__metric-card {
}
.summary__metric-card--urgent {
}
```

---

## Zusammenfassung

**Verwende immer:**

1. BEM-Namenskonvention (Block\_\_Element--Modifier)
2. Kebab-case für Klassennamen
3. CSS Custom Properties für Design Tokens
4. Mobile First Media Queries
5. `cursor: pointer` für Buttons
6. `border: unset` für Inputs/Buttons
7. Transitions zwischen 75ms - 125ms
8. IDs/data-attributes für JavaScript
9. Semantische, beschreibende Namen
10. Max. ein Element-Level tief

Bei Unsicherheiten, orientiere dich an existierenden Komponenten im Projekt.
