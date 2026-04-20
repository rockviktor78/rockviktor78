# Coding Rules

## CSS Rules

### BEM Naming Convention

**Structure:**

```css
.block {
} /* Independent component */
.block__element {
} /* Part of block */
.block--modifier {
} /* Variant of block */
.block__element--modifier {
} /* Variant of element */
```

**NEVER:**

- `.block__element__subelement` (nested elements)
- `.block-element` (single dash)
- `.blockElement` (camelCase)

### Mobile-First Responsive Design

**Always start with mobile base styles, then enhance:**

```css
.header {
  /* Mobile base (no media query) */
  height: 5rem;
  padding: 0 var(--spacing-md);
}

@media (min-width: 769px) {
  .header {
    height: 6rem;
    padding: 0 var(--spacing-xl);
  }
}
```

**Standard Breakpoints:**

- `640px` - Small tablets
- `768px` - Tablets (mobile → desktop switch)
- `769px` - Desktop start
- `1025px` - Large desktop

### Media Queries Placement

**✅ CORRECT: Inline with component**

```css
.button {
  padding: 0.5rem 1rem;

  @media (min-width: 768px) {
    padding: 0.75rem 1.5rem;
  }
}
```

**❌ WRONG: Collected at bottom**

```css
.button {
  padding: 0.5rem 1rem;
}

/* Don't do this */
@media (min-width: 768px) {
  .button {
    padding: 0.75rem 1.5rem;
  }
}
```

### CSS File Structure

```css
/* ========== Block Name ========== */

/* Base Styles */
.block {
  /* Mobile-first base styles */
}

/* Elements */
.block__element {
  /* Element styles */
}

/* Modifiers */
.block--modifier {
  /* Variant styles */
}

/* State Classes */
.block.is-active {
}
.block.is-disabled {
}
```

### CSS Variables

**Use design tokens from `variables.css`:**

```css
/* ✅ Good */
color: var(--color-primary);
padding: var(--spacing-lg);
border-radius: var(--radius-md);

/* ❌ Bad */
color: #007bff;
padding: 20px;
border-radius: 8px;
```

## JavaScript Rules

### Functional Programming

**Prefer pure functions:**

```javascript
// ✅ Good
function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}

// ❌ Bad
let total = 0;
function addToTotal(item) {
  total += item.price; // Side effect
}
```

### Function Length

- **Max 30 lines per function**
- If longer → extract helper functions
- One function = One responsibility

### Naming Conventions

```javascript
// Functions: camelCase, verb prefix
function getUserData() {}
function setActiveMenuItem() {}
function isExternalPage() {}

// Constants: UPPER_SNAKE_CASE
const API_ENDPOINT = "...";
const MAX_FILE_SIZE = 5000000;

// DOM elements: prefix with $
const $header = document.querySelector(".header");
const $menuButton = document.getElementById("menuBtn");
```

### Avoid Deep Nesting

**Max 3 levels:**

```javascript
// ✅ Good
function processUser(user) {
  if (!user) return null;
  if (!user.isActive) return null;

  return transformUserData(user);
}

// ❌ Bad
function processUser(user) {
  if (user) {
    if (user.isActive) {
      if (user.data) {
        // Too deep!
      }
    }
  }
}
```

### Error Handling

**Always handle Firebase errors:**

```javascript
// ✅ Good
async function fetchTasks() {
  try {
    const response = await getData("/tasks");
    return response || [];
  } catch (error) {
    console.error("Failed to fetch tasks:", error);
    return [];
  }
}
```

### DOM Manipulation

**Cache selectors:**

```javascript
// ✅ Good
const $taskList = document.querySelector(".task-list");
tasks.forEach((task) => {
  $taskList.appendChild(createTaskElement(task));
});

// ❌ Bad
tasks.forEach((task) => {
  document.querySelector(".task-list").appendChild(createTaskElement(task));
});
```

### Event Listeners

**Always use named functions for cleanup:**

```javascript
// ✅ Good
function handleClick(event) {
  // logic
}
button.addEventListener("click", handleClick);

// ❌ Bad (can't remove listener)
button.addEventListener("click", (e) => {
  // logic
});
```

## General Rules

### Code Style

- **2 spaces** for indentation
- **Semicolons** required
- **Single quotes** for strings
- **Trailing commas** in multiline objects/arrays

### Comments

**When to comment:**

- Complex logic (WHY, not WHAT)
- Workarounds or hacks
- Public API functions (JSDoc)

**DON'T comment:**

- Obvious code
- Dead code (delete it)

### File Organization

**One file = One responsibility:**

```
✅ board.js          → Board logic only
✅ contacts.js       → Contacts logic only
✅ firebase.js       → Firebase wrapper only

❌ utils.js          → Don't mix unrelated helpers
```

### Imports

**Order matters:**

```html
<!-- 1. Reset/Base -->
<link rel="stylesheet" href="../styles/base/reset.css" />
<link rel="stylesheet" href="../styles/base/variables.css" />

<!-- 2. Components -->
<link rel="stylesheet" href="../styles/components/header.css" />

<!-- 3. Page-specific -->
<link rel="stylesheet" href="../styles/board.css" />

<!-- 4. Utilities first, then page logic -->
<script src="../scripts/shared/utilities.js" defer></script>
<script src="../scripts/board.js" defer></script>
```

## Anti-Patterns

### CSS Anti-Patterns

❌ `!important` (except for utility classes)
❌ Deep nesting (`.a .b .c .d`)
❌ ID selectors (`#header`)
❌ Type selectors (`.header div`)
❌ Magic numbers (`width: 347px`)

### JavaScript Anti-Patterns

❌ `var` (use `const`/`let`)
❌ Global variables
❌ Modifying prototypes
❌ Synchronous operations on large data
❌ `eval()`, `with`, `arguments.caller`

## Testing Checklist

Before committing:

- [ ] Mobile view tested (320px, 375px, 428px, 768px)
- [ ] Desktop view tested (1024px, 1440px, 1920px)
- [ ] No console errors
- [ ] BEM naming consistent
- [ ] Functions under 30 lines
- [ ] No hardcoded values (use CSS variables)
