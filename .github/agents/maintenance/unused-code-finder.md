---
agent: maintenance-assistant
---

# Unused Code Finder Agent

## Purpose

Detect and report unused code, dead imports, unreferenced functions, and orphaned files.

## Detection Categories

### 1. Unused Imports

**Pattern:** Import statements with unused symbols

```bash
# Using TypeScript compiler
tsc --noEmit --noUnusedLocals --noUnusedParameters
```

**Example findings:**

```typescript
// ❌ Unused import
import { Component, signal, computed, inject } from "@angular/core";
//                            ^^^^^^^^ - Never used

// ✅ Cleaned
import { Component, signal, inject } from "@angular/core";
```

### 2. Unreferenced Functions

**Pattern:** Private/public functions never called

```typescript
// ❌ Unreferenced
export class MyComponent {
  private helperFunction(): void {
    // Never called anywhere
  }
}
```

**Detection:**

- Search for function calls in entire codebase
- Flag functions with zero references
- Exclude exported library functions

### 3. Unused Variables/Signals

**Pattern:** Declared but never read

```typescript
// ❌ Unused signal
export class MyComponent {
  private unusedSignal = signal<string>(""); // Never read
  usedSignal = signal<number>(0);

  displayValue = computed(() => this.usedSignal() * 2);
}
```

### 4. Dead Code Paths

**Pattern:** Unreachable code after return/throw

```typescript
// ❌ Dead code
function process(value: number): number {
  if (value < 0) return 0;
  return value * 2;
  console.log("Never reached"); // Dead code
}
```

### 5. Orphaned Files

**Pattern:** Files never imported

```bash
# Find TypeScript files not imported anywhere
for file in $(find src/app -name "*.ts"); do
  filename=$(basename "$file" .ts)
  if ! grep -r "from.*$filename" src/ > /dev/null 2>&1; then
    echo "Orphaned: $file"
  fi
done
```

**Common orphans:**

- Old component versions
- Deprecated services
- Backup files (\*.backup.ts)
- Experimental files (\*.test.ts not in test suite)

### 6. Unused Styles

**Pattern:** CSS classes defined but never used

```scss
// ❌ Unused class
.never-used-class {
  color: red;
}

// ✅ Used class
.message-card {
  padding: 16px;
}
```

**Detection:**

```bash
# Find SCSS classes not referenced in templates
grep -r "\.class-name" src/app/**/*.html
```

### 7. Commented-Out Code

**Pattern:** Large blocks of commented code

```typescript
// ❌ Commented out code (should be removed)
// export class OldComponent {
//   oldMethod() {
//     // 50 lines of old code
//   }
// }

// ✅ Helpful comment
// TODO: Refactor this to use signals instead of BehaviorSubject
```

## Scan Locations

### Priority 1: High Impact

```
src/app/features/       # Feature modules
src/app/stores/         # State stores
src/app/core/services/  # Core services
```

### Priority 2: Medium Impact

```
src/app/shared/         # Shared components
src/app/core/models/    # Data models
src/app/core/guards/    # Route guards
```

### Priority 3: Low Impact

```
src/app/layout/         # Layout components
src/styles/             # Global styles
```

## Exclusions

**Do NOT flag as unused:**

### Exported Public APIs

```typescript
// Part of public API even if not used internally
export interface User {
  // ...
}
```

### Lifecycle Hooks

```typescript
// Not called directly but used by Angular
ngOnInit() {
  // ...
}
```

### Event Handlers (Template-bound)

```typescript
// Called from template
handleClick(event: MouseEvent) {
  // ...
}
```

### Store Methods (May be used later)

```typescript
// Store methods may not all be used yet
export const UserStore = signalStore(
  withMethods((store) => ({
    reset() {
      /* Intended for future use */
    },
  })),
);
```

## Report Format

````markdown
## Unused Code Report

Generated: [Date]
Total Issues: [Count]

### Summary

- Unused Imports: 12
- Unreferenced Functions: 8
- Unused Variables: 5
- Orphaned Files: 3
- Dead Code Paths: 2
- Unused Styles: 15

### Detailed Findings

#### Unused Imports (12)

**src/app/features/auth/login.component.ts:3**

```typescript
import { Component, signal, computed, inject } from "@angular/core";
//                            ^^^^^^^^
// 'computed' is imported but never used
```
````

- **Impact:** Low (increases bundle size slightly)
- **Fix:** Remove unused import
- **Auto-fixable:** Yes

#### Unreferenced Functions (8)

**src/app/core/services/user.service.ts:45**

```typescript
private formatUsername(user: User): string {
  return `${user.firstName} ${user.lastName}`;
}
```

- **Impact:** Medium (dead code in service)
- **References:** 0 in entire codebase
- **Suggestion:** Remove or mark as deprecated
- **Auto-fixable:** No (manual verification needed)

#### Orphaned Files (3)

**src/app/features/dashboard/old-dashboard.component.ts**

- **Last Modified:** 2025-12-10
- **Imports:** None found
- **Impact:** High (unnecessary files)
- **Suggestion:** Delete file or move to archive
- **Auto-fixable:** No (review required)

#### Unused Styles (15)

**src/app/features/chat/chat.component.scss:78**

```scss
.old-message-style {
  /* Not found in any template */
}
```

- **Template References:** 0
- **Suggestion:** Remove unused style
- **Auto-fixable:** Yes (with caution)

````

## Automated Actions

### Safe Auto-Fixes (Apply automatically)
- Remove unused imports
- Remove trailing whitespace
- Remove console.log statements (in production)
- Format code (via Prettier)

### Manual Review Required
- Delete unreferenced functions
- Remove orphaned files
- Delete commented code blocks (> 10 lines)
- Remove unused styles (may be dynamic)

### Report Only
- Potential dead code paths
- Exported but unused public APIs
- Large commented blocks (potential docs)

## Scan Commands

```bash
# Find unused imports
npx eslint src/ --rule 'no-unused-vars: error'

# Find orphaned TypeScript files
find src/app -name "*.ts" | while read file; do
  base=$(basename "$file" .ts)
  if ! grep -r "from.*$base" src/ --exclude="$file" > /dev/null; then
    echo "Orphaned: $file"
  fi
done

# Find unused SCSS classes
# (requires custom script or tool)

# TypeScript compiler unused checks
tsc --noEmit --noUnusedLocals --noUnusedParameters

# Find large commented blocks
grep -n "^\/\/" src/**/*.ts | awk -F: '{print $1}' | uniq -c | awk '$1 > 10'
````

## Integration

Run scans:

- **Pre-commit:** Check for unused imports
- **Weekly:** Full orphaned file scan
- **Monthly:** Comprehensive dead code analysis
- **Before release:** Complete cleanup

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
