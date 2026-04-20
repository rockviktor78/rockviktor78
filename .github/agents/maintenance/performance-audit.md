---
agent: maintenance-assistant
---

# Performance Audit Agent

## Purpose

Identify and report performance issues, inefficiencies, and optimization opportunities.

## Audit Categories

### 1. Memory Leaks

**Detection:**

```typescript
// ❌ Memory leak
export class MyComponent {
  ngOnInit() {
    onSnapshot(this.docRef, (snapshot) => {
      // Never unsubscribed!
    });
  }
}

// ✅ Proper cleanup
export class MyComponent {
  private unsubscribe: (() => void) | null = null;

  ngOnInit() {
    this.unsubscribe = onSnapshot(this.docRef, (snapshot) => {
      // ...
    });
  }

  ngOnDestroy() {
    if (this.unsubscribe) this.unsubscribe();
  }
}
```

**Checks:**

- [ ] Firebase listeners without cleanup
- [ ] RxJS subscriptions without unsubscribe
- [ ] Event listeners without removal
- [ ] Intervals/timeouts without clear

### 2. Missing trackBy

**Detection:**

```html
<!-- ❌ No trackBy -->
<div *ngFor="let message of messages()">{{ message.content }}</div>

<!-- ✅ With trackBy -->
<div *ngFor="let message of messages(); trackBy: trackByMessageId">
  {{ message.content }}
</div>
```

```typescript
trackByMessageId(index: number, message: Message): string {
  return message.id;
}
```

**Impact:** Re-renders entire list on any change instead of just affected items.

### 3. Inefficient Computed Signals

**Detection:**

```typescript
// ❌ Recomputes entire array on every access
filteredItems = computed(() => {
  return this.allItems().filter(item =>
    item.name.toLowerCase().includes(this.searchTerm().toLowerCase())
  );
});

// ✅ Optimized with memoization
private lastSearchTerm = '';
private cachedResults: Item[] = [];

filteredItems = computed(() => {
  const term = this.searchTerm();
  const items = this.allItems();

  if (term === this.lastSearchTerm) {
    return this.cachedResults;
  }

  this.lastSearchTerm = term;
  this.cachedResults = items.filter(item =>
    item.name.toLowerCase().includes(term.toLowerCase())
  );

  return this.cachedResults;
});
```

### 4. Large Component Files

**Detection:**

```bash
# Find large components
find src/app -name "*.component.ts" -exec wc -l {} \; | sort -rn | head -10
```

**Issues:**

- Slow compilation
- Hard to maintain
- Potential performance impact

**Thresholds:**

- > 400 LOC → Review
- > 600 LOC → Refactor required
- > 1000 LOC → Critical

### 5. Bundle Size

**Detection:**

```bash
# Build and analyze
ng build --configuration production
npx webpack-bundle-analyzer dist/stats.json
```

**Checks:**

- [ ] Total bundle size < 500KB (initial)
- [ ] Lazy loading implemented for routes
- [ ] Tree-shaking enabled
- [ ] Unused dependencies removed

### 6. Change Detection Issues

**Detection:**

```typescript
// ❌ Default change detection
@Component({
  selector: 'app-my-component',
  // Uses Default strategy - checks on every event
})

// ✅ OnPush optimization
@Component({
  selector: 'app-my-component',
  changeDetection: ChangeDetectionStrategy.OnPush,
  // Only checks when inputs change or events emit
})
```

**When to use OnPush:**

- Component receives data via inputs
- Component uses immutable data patterns
- Component with many child components

### 7. Network Request Optimization

**Detection:**

```typescript
// ❌ Sequential requests
async loadUserData(userId: string) {
  const user = await this.getUser(userId);
  const profile = await this.getProfile(user.profileId);
  const settings = await this.getSettings(user.settingsId);
}

// ✅ Parallel requests
async loadUserData(userId: string) {
  const user = await this.getUser(userId);
  const [profile, settings] = await Promise.all([
    this.getProfile(user.profileId),
    this.getSettings(user.settingsId),
  ]);
}
```

### 8. Unnecessary Re-renders

**Detection:**

```typescript
// ❌ Function in template
<div>{{ formatDate(message.timestamp) }}</div>

// Called on every change detection cycle!
formatDate(date: Date): string {
  return date.toLocaleDateString();
}

// ✅ Computed signal
formattedDate = computed(() =>
  this.message().timestamp.toLocaleDateString()
);

<div>{{ formattedDate() }}</div>
```

---

## Performance Metrics

### Target Metrics

- **First Contentful Paint (FCP):** < 1.8s
- **Largest Contentful Paint (LCP):** < 2.5s
- **Total Blocking Time (TBT):** < 200ms
- **Cumulative Layout Shift (CLS):** < 0.1
- **Time to Interactive (TTI):** < 3.5s

### Measurement Tools

```bash
# Lighthouse audit
npx lighthouse http://localhost:4200 --view

# Bundle analysis
npm run build -- --stats-json
npx webpack-bundle-analyzer dist/stats.json

# Angular performance profiling
# Open DevTools → Performance → Record
```

---

## Audit Checklist

### Component Level

- [ ] No memory leaks (listeners cleaned up)
- [ ] trackBy used in all \*ngFor
- [ ] OnPush change detection where applicable
- [ ] No functions called in templates
- [ ] Lazy loading for heavy components
- [ ] Virtual scrolling for long lists (> 100 items)

### State Management

- [ ] No unnecessary signal updates
- [ ] Computed signals for derived state
- [ ] effect() only for side effects
- [ ] Proper cleanup on destroy

### Network

- [ ] Parallel requests where possible
- [ ] Request caching implemented
- [ ] Debouncing for user input
- [ ] Pagination for large datasets

### Build & Bundle

- [ ] Production build optimized
- [ ] Tree-shaking enabled
- [ ] Lazy loading for routes
- [ ] Code splitting implemented
- [ ] Unused dependencies removed

### Images & Assets

- [ ] Images optimized (WebP format)
- [ ] Lazy loading for images
- [ ] Appropriate image sizes
- [ ] SVG for icons

---

## Report Format

````markdown
## Performance Audit Report

**Date:** [Date]
**Build:** Production
**Total Score:** 78/100

### Metrics

| Metric | Current | Target  | Status  |
| ------ | ------- | ------- | ------- |
| FCP    | 1.2s    | < 1.8s  | ✅ Pass |
| LCP    | 3.1s    | < 2.5s  | ❌ Fail |
| TBT    | 150ms   | < 200ms | ✅ Pass |
| CLS    | 0.05    | < 0.1   | ✅ Pass |
| TTI    | 4.2s    | < 3.5s  | ❌ Fail |

### Critical Issues (2)

**1. Missing trackBy in message list**

- **File:** `src/app/features/chat/message-list.component.html:15`
- **Impact:** High - Re-renders 100+ items on any change
- **Fix:**
  ```typescript
  trackByMessageId(index: number, message: Message): string {
    return message.id;
  }
  ```
````

**2. Memory leak in ThreadComponent**

- **File:** `src/app/features/dashboard/thread/thread.component.ts:25`
- **Impact:** Critical - Memory grows over time
- **Fix:** Add ngOnDestroy with unsubscribe

### Performance Opportunities (5)

**1. Large component file**

- **File:** `dashboard.component.ts` (520 LOC)
- **Impact:** Medium - Slow compilation
- **Suggestion:** Split into child components

**2. Unnecessary re-renders**

- **File:** `message-card.component.html:8`
- **Impact:** Medium - Function called in template
- **Fix:** Use computed signal

### Bundle Analysis

| Chunk        | Size  | Lazy | Status    |
| ------------ | ----- | ---- | --------- |
| main.js      | 380KB | No   | ✅ Good   |
| dashboard.js | 120KB | Yes  | ✅ Good   |
| auth.js      | 80KB  | Yes  | ✅ Good   |
| vendor.js    | 450KB | No   | ⚠️ Review |

**Suggestions:**

- vendor.js is large - review dependencies
- Consider splitting dashboard module further

````

---

## Automated Checks

```bash
#!/bin/bash
# performance-audit.sh

echo "Running Performance Audit..."

# 1. Check for missing trackBy
grep -rn "*ngFor" src/app --include="*.html" | grep -v "trackBy" > /tmp/missing-trackby.txt
if [ -s /tmp/missing-trackby.txt ]; then
  echo "❌ Found *ngFor without trackBy:"
  cat /tmp/missing-trackby.txt
fi

# 2. Check for large files
echo "Checking file sizes..."
find src/app -name "*.ts" -exec wc -l {} \; | awk '$1 > 400 {print "⚠️", $2, "-", $1, "lines"}'

# 3. Build size check
echo "Analyzing bundle size..."
npm run build -- --configuration production 2>&1 | grep "Initial Chunk Files"

# 4. Find template functions
echo "Checking for functions in templates..."
grep -rn "{{ .*(" src/app --include="*.html" | grep -v "async" > /tmp/template-functions.txt
if [ -s /tmp/template-functions.txt ]; then
  echo "⚠️ Found function calls in templates:"
  cat /tmp/template-functions.txt
fi

echo "Audit complete!"
````

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** DABubble
