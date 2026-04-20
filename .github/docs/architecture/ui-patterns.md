# UI Patterns

## Layout Patterns

### App Layout Structure

```html
<body class="app-layout">
  <!-- Header Template (included) -->
  <header class="header__content">
    <!-- Header content -->
  </header>

  <!-- Sidebar Template (included) -->
  <nav class="menu">
    <!-- Navigation -->
  </nav>

  <!-- Main Content -->
  <main class="content-area">
    <div class="content-limit">
      <!-- Page content -->
    </div>
  </main>
</body>
```

**Layout Modes:**

**Mobile (≤768px):**

```css
.app-layout {
  display: flex;
  flex-direction: column;
  height: 100vh;
}
```

**Desktop (≥769px):**

```css
.app-layout {
  display: grid;
  grid-template-columns: 14.5rem 1fr;
  grid-template-rows: var(--spacing-5xl) 1fr;
}
```

## Header Pattern

### Desktop Header

```html
<header class="header__content">
  <div class="header__center-wrapper">
    <h1 class="header__title">Kanban Project Management Tool</h1>

    <div class="header__actions">
      <button class="header__help-btn">
        <img src="../assets/img/header/help-default.svg" alt="" />
      </button>

      <div class="header__user">
        <button class="header__avatar">
          <span class="header__user-initials">VW</span>
        </button>

        <div class="header__dropdown">
          <!-- Dropdown menu -->
        </div>
      </div>
    </div>
  </div>
</header>
```

**Key Features:**

- Fixed height: `5rem` (mobile), `6rem` (desktop)
- Max-width: `calc(90rem - 14.5rem)` to align with 1440px content
- User avatar with dropdown menu
- Help button (desktop only)
- Logo (mobile only)

### Mobile Header (≤768px)

```html
<header class="header__content">
  <div class="header__center-wrapper">
    <img class="header__logo" src="../assets/img/shared/join-logo-blue.svg" />
    <!-- Title hidden on mobile -->
    <div class="header__actions">
      <!-- Help button hidden on mobile -->
      <div class="header__user">
        <button class="header__avatar">...</button>
      </div>
    </div>
  </div>
</header>
```

### External Mode Header (Not Logged In)

```css
/* Mobile: Avatar hidden, back-button visible */
.external .header__user {
  display: none;
}

.external .back-btn {
  display: flex;
}
```

**Back Button:**

- Position: `fixed`
- Top: `calc(5rem + var(--spacing-2xl))` (mobile)
- Top: `calc(6rem + var(--spacing-5xl))` (desktop)
- Right: `var(--spacing-lg)` (mobile), `var(--spacing-5xl)` (desktop)
- Links to: `../index.html` (external mode) or `history.back()` (help page)

## Sidebar/Navigation Pattern

### Desktop Sidebar (≥769px)

```html
<nav class="menu">
  <div class="menu__logo">
    <img src="../assets/img/shared/join-logo-white.svg" alt="Join" />
  </div>

  <div class="menu__nav">
    <button class="menu__link" id="navSummary">
      <img src="../assets/img/menu/summary-icon.svg" />
      <span>Summary</span>
    </button>
    <!-- More links -->
  </div>

  <div class="menu__footer">
    <a href="privacy-policy.html">Privacy Policy</a>
    <a href="legal-notice.html">Legal Notice</a>
  </div>
</nav>
```

### Mobile Navigation (≤768px)

```html
<nav class="menu">
  <!-- Desktop logo hidden -->

  <div class="menu__nav">
    <!-- Main navigation buttons (4 max) -->
  </div>

  <!-- Mobile footer links -->
  <div class="menu__mobile">
    <a href="privacy-policy.html" class="menu__mobile__link">
      Privacy Policy
    </a>
    <a href="legal-notice.html" class="menu__mobile__link"> Legal Notice </a>
  </div>
</nav>
```

**Mobile Navigation:**

- Position: `fixed` at bottom
- Background: White with shadow
- Height: `5rem`
- Icons centered, no text labels
- Max 4 main navigation items

### External Mode Navigation

```css
/* External: Login button visible, internal nav hidden */
.external .menu__nav {
  justify-content: flex-start;
}

.external .login-button {
  display: flex;
}

.external #navSummary,
.external #navAddTask,
.external #navBoard,
.external #navContacts {
  display: none;
}
```

**Login Button (Mobile External Mode):**

- Positioned left in navigation
- Width adjusts per breakpoint:
  - 320px: centered
  - 359px+: `margin-left: 0.75rem`
  - 428px+: `margin-left: 1.5rem`

## Content Area Pattern

### Content Limit Container

```html
<main class="content-area">
  <div class="content-limit">
    <!-- Page content here -->
  </div>
</main>
```

**Responsive Container:**

```css
.content-limit {
  width: 100%;
  max-width: calc(var(--content-max-width) - var(--sidebar-width));
  /* Mobile */
  padding: var(--spacing-2xl) var(--spacing-lg);
}

@media (min-width: 769px) {
  .content-limit {
    padding: var(--spacing-5xl) var(--spacing-5xl);
  }
}
```

## Button Patterns

### Primary Button

```html
<button class="btn btn--primary">Create Task</button>
```

### Icon Button

```html
<button class="icon-btn">
  <img src="../assets/img/shared/plus-icon.svg" alt="Add" />
</button>
```

### Avatar Button

```html
<button class="header__avatar">
  <span class="header__user-initials">VW</span>
</button>
```

## Form Patterns

### Input Field

```html
<div class="form-input">
  <label for="taskTitle">Title</label>
  <input type="text" id="taskTitle" placeholder="Enter task title" required />
</div>
```

### Dropdown

```html
<div class="header__dropdown">
  <a href="help.html" class="header__dropdown-link">Help</a>
  <a href="legal-notice.html" class="header__dropdown-link">Legal Notice</a>
  <a href="../index.html" class="header__dropdown-link">Log out</a>
</div>
```

## Card Patterns

### Task Card

```html
<div class="task-card">
  <div class="task-card__header">
    <span class="task-card__category">Design</span>
  </div>
  <h3 class="task-card__title">Create mockups</h3>
  <p class="task-card__description">...</p>
  <div class="task-card__footer">
    <div class="task-card__assignees">...</div>
    <span class="task-card__priority">High</span>
  </div>
</div>
```

## Modal Patterns

### Overlay Pattern

```html
<div class="overlay" id="taskModal">
  <div class="modal">
    <button class="modal__close">&times;</button>
    <div class="modal__content">
      <!-- Modal content -->
    </div>
  </div>
</div>
```

**Modal Behavior:**

- Overlay: `position: fixed`, `z-index: 1000`
- Modal: centered with `transform: translate(-50%, -50%)`
- Close on overlay click or close button
- Trap focus inside modal

## State Classes

### Common States

```css
.is-active {
} /* Active navigation item */
.is-open {
} /* Open dropdown/modal */
.is-disabled {
} /* Disabled button/input */
.is-loading {
} /* Loading state */
.is-error {
} /* Error state */
.is-hidden {
} /* Hidden element */
```

### BEM Modifiers

```css
.menu__link--active {
}
.button--disabled {
}
.dropdown--open {
}
```

## Utility Classes

```css
.d-none {
  display: none;
}
.no-select {
  user-select: none;
}
.no-scroll {
  overflow: hidden;
}
```

## Animation Patterns

### Hover Effects

```css
.button {
  transition: transform var(--transition-fast);
}

.button:hover {
  transform: scale(1.05);
}
```

### View Transitions

```css
::view-transition-old(root) {
  animation: fade-out 0.3s ease-out;
}

::view-transition-new(root) {
  animation: fade-in 0.3s ease-in;
}
```

## Responsive Breakpoints

| Breakpoint | Min Width | Target Devices |
| ---------- | --------- | -------------- |
| xs         | 320px     | Small phones   |
| sm         | 359px     | Medium phones  |
| md         | 428px     | Large phones   |
| lg         | 768px     | Tablets        |
| xl         | 769px     | Desktop        |
| 2xl        | 1440px    | Large screens  |

## Accessibility Patterns

### ARIA Labels

```html
<button aria-label="Close modal" class="modal__close">&times;</button>
```

### Focus Management

```css
.button:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

### Skip Links

```html
<a href="#main-content" class="skip-link"> Skip to main content </a>
```
