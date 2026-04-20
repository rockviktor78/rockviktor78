---
prompt: styling-bem
project: join
category: styling
---

# Styling & BEM - Join Project

## BEM Methodology

Join uses **BEM (Block Element Modifier)** naming convention for CSS classes.

**Full BEM documentation**: [BEM-CONVENTIONS.md](../../skills/BEM-CONVENTIONS.md)

---

## Quick BEM Reference

### Naming Pattern

```
.block              → Component (e.g., .card, .button, .menu)
.block__element     → Part of block (e.g., .card__title, .button__icon)
.block--modifier    → Variant of block (e.g., .button--primary, .card--highlighted)
```

### Examples

```html
<!-- Task Card Component -->
<article class="task-card task-card--urgent">
  <div class="task-card__category">User Story</div>
  <h3 class="task-card__title">Implement Login</h3>
  <p class="task-card__description">Create login form with validation</p>

  <div class="task-card__footer">
    <div class="task-card__assigned">
      <span class="task-card__avatar">MK</span>
      <span class="task-card__avatar">JS</span>
    </div>
    <svg class="task-card__priority-icon"><!-- icon --></svg>
  </div>
</article>
```

```css
/* ========== Task Card Block ========== */

/* Block */
.task-card {
  background: var(--color-white);
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 100ms ease;
}

@media (min-width: 768px) {
  .task-card {
    padding: 20px;
  }
}

.task-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

/* Elements */
.task-card__category {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 600;
  background: var(--color-primary);
  color: var(--color-white);
}

.task-card__title {
  font-size: 18px;
  font-weight: 600;
  margin: 12px 0 8px;
  color: var(--color-text-primary);
}

.task-card__description {
  font-size: 14px;
  color: var(--color-text-secondary);
  line-height: 1.5;
}

.task-card__footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 16px;
}

/* Modifiers */
.task-card--urgent {
  border-left: 4px solid var(--color-urgent);
}

.task-card--dragging {
  opacity: 0.5;
  transform: rotate(2deg);
}
```

---

## CSS Custom Properties (Variables)

### ❌ NO: Hardcoded Values

```css
/* ❌ WRONG: Hardcoded colors and values */
.button {
  background: #4589ff;
  color: #ffffff;
  padding: 12px 24px;
  border-radius: 8px;
}
```

### ✅ YES: CSS Variables

```css
/* ✅ CORRECT: CSS custom properties */
.button {
  background: var(--color-primary);
  color: var(--color-white);
  padding: var(--spacing-md) var(--spacing-lg);
  border-radius: var(--border-radius-md);
}
```

### Variable Definition

```css
/* css/base/variables.css */

:root {
  /* Colors */
  --color-primary: #4589ff;
  --color-primary-hover: #2a6fdb;
  --color-secondary: #29abe2;
  --color-success: #4caf50;
  --color-error: #f44336;
  --color-warning: #ff9800;

  --color-white: #ffffff;
  --color-black: #000000;
  --color-gray-light: #f6f7f8;
  --color-gray: #cdcdcd;
  --color-gray-dark: #2a3647;

  --color-text-primary: #2a3647;
  --color-text-secondary: #a8a8a8;

  --color-urgent: #ff3d00;
  --color-medium: #ffa800;
  --color-low: #7ae229;

  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 12px;
  --spacing-lg: 16px;
  --spacing-xl: 24px;
  --spacing-xxl: 32px;

  /* Border Radius */
  --border-radius-sm: 4px;
  --border-radius-md: 8px;
  --border-radius-lg: 16px;
  --border-radius-full: 50%;

  /* Shadows */
  --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 4px 8px rgba(0, 0, 0, 0.15);
  --shadow-lg: 0 8px 16px rgba(0, 0, 0, 0.2);

  /* Transitions */
  --transition-fast: 75ms ease;
  --transition-normal: 125ms ease;
  --transition-slow: 300ms ease;

  /* Font Sizes */
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-md: 16px;
  --font-size-lg: 18px;
  --font-size-xl: 24px;
  --font-size-xxl: 32px;

  /* Font Weights */
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
}
```

---

## Responsive Design

### Mobile-First Approach

**Start with mobile styles, add desktop enhancements with `min-width` media queries.**

```css
/* ✅ CORRECT: Mobile-first */
.board {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

@media (min-width: 768px) {
  .board {
    padding: 32px;
    flex-direction: row;
  }
}

@media (min-width: 1025px) {
  .board {
    max-width: 1440px;
    margin: 0 auto;
  }
}

/* ❌ WRONG: Desktop-first (using max-width) */
.board {
  padding: 32px;
  flex-direction: row;
}

@media (max-width: 768px) {
  .board {
    padding: 16px;
    flex-direction: column;
  }
}
```

### Standard Breakpoints

```css
/* Mobile: 320px - 639px (base, no media query) */

/* Small tablets: 640px+ */
@media (min-width: 640px) {
  /* ... */
}

/* Tablets: 768px+ (Mobile → Desktop switch) */
@media (min-width: 768px) {
  /* ... */
}

/* Desktop: 1025px+ */
@media (min-width: 1025px) {
  /* ... */
}

/* Large screens: 1440px+ */
@media (min-width: 1440px) {
  /* ... */
}
```

### ❌ NO: Responsive Section at End of File

```css
/* ❌ WRONG: Collected media queries at end */
.header {
  padding: 16px;
}

.menu {
  display: none;
}

.board {
  padding: 16px;
}

/* Responsive Design */
@media (min-width: 768px) {
  .header {
    padding: 24px;
  }
  .menu {
    display: block;
  }
  .board {
    padding: 32px;
  }
}
```

### ✅ YES: Inline Media Queries

```css
/* ✅ CORRECT: Media queries inline with blocks */

/* ========== Header Block ========== */

.header {
  padding: 16px;
}

@media (min-width: 768px) {
  .header {
    padding: 24px;
  }
}

/* ========== Menu Block ========== */

.menu {
  display: none;
}

@media (min-width: 768px) {
  .menu {
    display: block;
  }
}

/* ========== Board Block ========== */

.board {
  padding: 16px;
}

@media (min-width: 768px) {
  .board {
    padding: 32px;
  }
}
```

---

## CSS File Structure

### BEM Section Order

Each CSS file follows this structure:

```css
/* ========== Block Name ========== */

/* 1. Block Styles */
.block {
  /* Base mobile styles */
}

@media (min-width: 768px) {
  .block {
    /* Desktop styles */
  }
}

/* 2. Element Styles */
.block__element {
  /* Base mobile styles */
}

@media (min-width: 768px) {
  .block__element {
    /* Desktop styles */
  }
}

.block__another-element {
  /* ... */
}

/* 3. Modifier Styles */
.block--modifier {
  /* ... */
}

.block__element--modifier {
  /* ... */
}
```

### Example: Complete CSS File

```css
/* ========== Button Block ========== */

/* Block */
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-sm);
  padding: var(--spacing-md) var(--spacing-lg);
  border: none;
  border-radius: var(--border-radius-md);
  font-size: var(--font-size-md);
  font-weight: var(--font-weight-semibold);
  cursor: pointer;
  transition: all var(--transition-normal);
  background: var(--color-gray);
  color: var(--color-text-primary);
}

.button:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.button:active {
  transform: translateY(0);
}

.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Elements */
.button__icon {
  width: 20px;
  height: 20px;
}

/* Modifiers */
.button--primary {
  background: var(--color-primary);
  color: var(--color-white);
}

.button--primary:hover {
  background: var(--color-primary-hover);
}

.button--secondary {
  background: transparent;
  border: 2px solid var(--color-primary);
  color: var(--color-primary);
}

.button--small {
  padding: var(--spacing-sm) var(--spacing-md);
  font-size: var(--font-size-sm);
}

.button--large {
  padding: var(--spacing-lg) var(--spacing-xl);
  font-size: var(--font-size-lg);
}
```

---

## Design Requirements

### Figma Compliance

**All UI elements must match the Figma design:**

- Colors (exact hex values)
- Spacing (padding, margin)
- Font sizes and weights
- Border radius
- Shadows

### Hover Effects

**All interactive elements require hover feedback:**

```css
.button {
  cursor: pointer;
  transition: all 100ms ease;
}

.button:hover {
  background: var(--color-primary-hover);
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.task-card {
  cursor: pointer;
  transition: transform 100ms ease;
}

.task-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}
```

### Transitions

**Use transitions between 75ms and 125ms:**

```css
/* ✅ CORRECT: Transitions in acceptable range */
.element {
  transition: all 100ms ease;
}

.another {
  transition:
    transform 75ms ease,
    opacity 125ms ease;
}

/* ❌ WRONG: Too slow */
.slow {
  transition: all 500ms ease;
}
```

### Cursor Styles

```css
/* ✅ CORRECT: Pointer for clickable elements */
.button,
.link,
.card,
.menu__item {
  cursor: pointer;
}

/* ✅ CORRECT: Default for text content */
.text,
.description {
  cursor: default;
}

/* ✅ CORRECT: Move for draggable items */
.task-card--draggable {
  cursor: move;
}
```

---

## Form Styling

### Input & Button Resets

```css
/* ✅ CORRECT: Remove default borders */
input,
textarea,
select,
button {
  border: unset;
  outline: none;
}

input:focus,
textarea:focus,
select:focus {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

### Custom Input Styles

```css
.form__input {
  width: 100%;
  padding: var(--spacing-md);
  border: 1px solid var(--color-gray);
  border-radius: var(--border-radius-md);
  font-size: var(--font-size-md);
  background: var(--color-white);
  color: var(--color-text-primary);
  transition: border-color var(--transition-normal);
}

.form__input:focus {
  border-color: var(--color-primary);
  outline: none;
}

.form__input--error {
  border-color: var(--color-error);
}

.form__input::placeholder {
  color: var(--color-text-secondary);
}
```

---

## Content Constraints

### Max Width for Large Screens

```css
/* ✅ CORRECT: Limit content width on large screens */
.board {
  width: 100%;
  max-width: 1920px;
  margin: 0 auto;
  padding: 16px;
}
```

### No Horizontal Scrollbars

```css
/* ✅ CORRECT: Prevent horizontal overflow */
body {
  overflow-x: hidden;
}

.container {
  width: 100%;
  max-width: 100vw;
  box-sizing: border-box;
}
```

---

## Common Mistakes to Avoid

### ❌ NO !important (unless absolutely necessary)

```css
/* ❌ WRONG */
.button {
  background: red !important;
}

/* ✅ CORRECT: Use specific selectors */
.form__button.button--primary {
  background: var(--color-primary);
}
```

### ❌ NO Deep Nesting (Max 3 levels)

```css
/* ❌ WRONG: Too deeply nested */
.board .column .card .header .title .text {
  color: red;
}

/* ✅ CORRECT: BEM flattens structure */
.card__title {
  color: red;
}
```

### ❌ NO Inline Styles in HTML

```html
<!-- ❌ WRONG -->
<div style="color: red; padding: 10px;">Content</div>

<!-- ✅ CORRECT -->
<div class="content">Content</div>
```

---

## Checklist for Styling

- [ ] BEM naming convention used
- [ ] CSS variables for all values (colors, spacing, etc.)
- [ ] Mobile-first responsive design
- [ ] Media queries inline with blocks/elements
- [ ] Standard breakpoints (640px, 768px, 1025px)
- [ ] Hover effects on interactive elements (75ms-125ms)
- [ ] `cursor: pointer` on clickable elements
- [ ] Form inputs: `border: unset`
- [ ] No horizontal scrollbars
- [ ] Max width set for large screens
- [ ] No `!important` (unless justified)
- [ ] Max 3 levels of nesting
- [ ] Matches Figma design exactly

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
