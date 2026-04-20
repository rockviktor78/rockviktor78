---
agent: refactoring-assistant
---

# Module Cleanup & Optimization Agent

## Purpose

Identify and refactor JavaScript modules that violate size limits, have inefficient patterns, or can be optimized.

## Detection Criteria

### Oversized Modules (> 400 LOC)

Scan for JavaScript files exceeding 400 lines:

```bash
find scripts -name "*.js" -exec wc -l {} \; | awk '$1 > 400 {print $2, ":", $1, "lines"}'
```

**Action:** Suggest extraction strategies

### Oversized Service Modules (> 400 LOC)

Scan for service files exceeding recommended size:

```bash
find scripts -name "*-service.js" -o -name "firebase.js" -exec wc -l {} \; | awk '$1 > 400 {print $2, ":", $1, "lines"}'
```

**Action:** Suggest modular refactoring

### Inefficient Patterns

Look for:

- Functions > 14 lines
- Missing JSDoc comments
- Duplicate function logic
- window.\* global usage (should use modules)
- Missing error handling in async functions
- Complex nested callbacks (use async/await)

## Refactoring Strategies

### Strategy 1: Extract Module

**When:**

- File > 400 LOC
- Distinct feature groups
- Reusable functions

**How:**

```javascript
// Before: contacts.js (500 LOC)
// Contact rendering (100 LOC)
// Contact CRUD operations (200 LOC)
// Contact form handling (100 LOC)
// Utilities (100 LOC)

// After: Split into focused modules
scripts/contacts/
├── contacts.js (150 LOC)           // Main orchestrator
├── contacts-service.js (200 LOC)   // CRUD operations
├── contacts-templates.js (100 LOC)  // HTML templates
└── contacts-utils.js (100 LOC)      // Helper functions
```

### Strategy 2: Extract Helper Functions

**When:**

- Functions > 14 lines
- Complex logic in main module
- Reusable calculations

**How:**

```javascript
// Before
export function processTaskData(task) {
  // 20 lines of complex logic
  const validated = validateTask(task);
  const formatted = formatTaskData(validated);
  const enriched = addMetadata(formatted);
  return enriched;
}

// After: Extract helper functions
// task-utils.js
/**
 * Validates task data
 * @param {Object} task - Task object
 * @returns {Object} Validated task
 */
export function validateTask(task) {
  // Validation logic
  return task;
}

/**
 * Formats task data for display
 * @param {Object} task - Task object
 * @returns {Object} Formatted task
 */
export function formatTaskData(task) {
  // Format logic
  return task;
}

// Main module imports
import { validateTask, formatTaskData } from "./task-utils.js";
```

### Strategy 3: Modularize Service

**When:**

- Service file > 400 LOC
- Multiple feature groups
- Complex CRUD logic

**How:**

```javascript
// Before: firebase.js (450 LOC)
export async function postData(path, data) { /* ... */ }
export async function fetchData(path) { /* ... */ }
export async function updateData(path, data) { /* ... */ }
export async function deleteData(path) { /* ... */ }
// + 350 lines more...

// After: Modular structure
scripts/services/
├── firebase.js (100 LOC)          // Main exports
├── firebase-crud.js (150 LOC)    // CRUD operations
├── firebase-auth.js (100 LOC)    // Auth operations
└── firebase-utils.js (100 LOC)   // Helper functions
```

### Strategy 4: Extract Computed Logic

**When:**

- Complex computed signals
- Duplicate logic across components
- Reusable calculations

**How:**

```typescript
// Before
export class ComponentA {
  displayName = computed(() => {
    const user = this.user();
    if (!user) return "Guest";
    return user.firstName && user.lastName
      ? `${user.firstName} ${user.lastName}`
      : user.email;
  });
}

export class ComponentB {
  // Same logic duplicated
}

// After: shared/utils/user.utils.ts
export function getUserDisplayName(user: User | null): string {
  if (!user) return "Guest";
  return hasFullName(user) ? formatFullName(user) : user.email;
}

function hasFullName(user: User): boolean {
  return Boolean(user.firstName && user.lastName);
}

function formatFullName(user: User): string {
  return `${user.firstName} ${user.lastName}`;
}

// Both components
import { getUserDisplayName } from "@/shared/utils/user.utils";

displayName = computed(() => getUserDisplayName(this.user()));
```

## Optimization Checklist

### Component Level

- [ ] Component ≤ 400 LOC
- [ ] Functions ≤ 14 lines
- [ ] No nested subscriptions
- [ ] trackBy used in all \*ngFor
- [ ] OnPush change detection (if applicable)
- [ ] Lazy loading for heavy components
- [ ] No memory leaks

### Template Level

- [ ] No complex logic in templates
- [ ] Use computed signals for transformations
- [ ] Minimal \*ngIf nesting (< 3 levels)
- [ ] async pipe for observables
- [ ] trackBy functions for lists

### Signal & State Level

- [ ] No unused signals
- [ ] Computed signals for derived state
- [ ] effect() only for side effects
- [ ] No signal reads in templates (use computed)
- [ ] Proper cleanup in destroy

### Performance Level

- [ ] No blocking operations in constructor
- [ ] Debounced user inputs
- [ ] Virtual scrolling for long lists (> 100 items)
- [ ] Image lazy loading
- [ ] Code splitting for large modules

## Refactoring Process

1. **Identify** violations via automated scans
2. **Analyze** component structure and dependencies
3. **Plan** extraction strategy (child components, helpers, etc.)
4. **Extract** code into focused files
5. **Test** that functionality remains intact
6. **Verify** size limits met
7. **Document** changes in commit message

## Output Format

```markdown
## Component Cleanup Report

### Oversized Components

- `src/app/features/dashboard/dashboard.component.ts` (520 LOC)
  - **Issue:** Exceeds 400 LOC limit
  - **Suggestion:** Extract header, sidebar, and main content into child components
  - **Impact:** Split into 4 files @ ~130 LOC each

### Inefficient Patterns

- `src/app/features/chat/message-list.component.html` (Line 45)
  - **Issue:** Missing trackBy in \*ngFor
  - **Fix:** Add `trackByMessageId` function

### Optimization Opportunities

- `src/app/stores/user.store.ts` (180 LOC)
  - **Issue:** Exceeds store limit
  - **Suggestion:** Split into modular structure (user/ folder)
  - **Impact:** 5 files @ ~40 LOC each
```

---

**Version:** 1.0
**Last Updated:** Februar 2026
**Project:** Join
