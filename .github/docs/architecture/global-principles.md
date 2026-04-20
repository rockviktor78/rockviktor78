# Global Principles

All decisions and code generation must prioritize:

## 1. Simplicity

**Over complexity**

- Prefer obvious solutions over clever ones
- Write code that's easy to read, not just easy to write
- Delete code rather than comment it out
- One function = One responsibility
- Extract complex logic into named functions

**Ask:** Can a junior developer understand this in 2 minutes?

## 2. Maintainability

**Over quick fixes**

- Consistent naming conventions (BEM, camelCase)
- Clear file structure (one file = one purpose)
- No magic numbers (use CSS variables)
- Predictable patterns (same problem = same solution)
- Self-documenting code (clear names over comments)

**Ask:** Can this be changed without breaking other parts?

## 3. Scalability

**Over premature optimization**

- Modular architecture (components, not monoliths)
- Separation of concerns (UI ≠ logic ≠ data)
- Reusable patterns (templates, utilities)
- Extensible structure (easy to add features)
- Performance-conscious (but not at cost of readability)

**Ask:** Will this work with 10x more data/users?

## 4. Standards Compliance

**Over shortcuts**

- Web Standards (HTML5, CSS3, ES6+)
- Accessibility (ARIA, semantic HTML, keyboard nav)
- BEM CSS methodology
- Mobile-first responsive design
- No deprecated APIs or hacks

**Ask:** Does this follow established best practices?

## 5. User Experience

**Over developer convenience**

- Mobile-first (320px → 1920px+)
- Fast loading (minimal dependencies)
- Clear feedback (loading, error, success states)
- Intuitive navigation (no hidden features)
- Accessible (keyboard, screen readers)

**Ask:** Is this delightful to use?

---

## Core Values

### Clarity > Cleverness

```javascript
// ❌ Clever but unclear
const u = d.filter((x) => x.a).map((x) => ({ ...x, b: x.c * 2 }));

// ✅ Clear and obvious
const activeUsers = users
  .filter((user) => user.isActive)
  .map((user) => ({
    ...user,
    doubledScore: user.score * 2,
  }));
```

### Explicit > Implicit

```javascript
// ❌ Implicit behavior
function save(data) {
  // Secretly mutates global state
}

// ✅ Explicit behavior
function saveTask(taskData) {
  return postData("/tasks", taskData);
}
```

### Consistency > Perfection

```javascript
// ✅ Consistent naming
function getUser() {}
function getUserTasks() {}
function getUserContacts() {}

// ❌ Inconsistent naming
function fetchUser() {}
function retrieveUserTasks() {}
function loadUserContacts() {}
```

### Functional > Imperative

```javascript
// ❌ Imperative with side effects
let sum = 0;
function addToSum(value) {
  sum += value;
}

// ✅ Functional without side effects
function calculateSum(values) {
  return values.reduce((sum, value) => sum + value, 0);
}
```

---

## Decision Framework

When faced with a choice, ask:

1. **Is it simple?** → Can it be simpler?
2. **Is it maintainable?** → Will future-me understand this?
3. **Is it scalable?** → Does it work with more data?
4. **Is it standard?** → Does it follow conventions?
5. **Is it user-friendly?** → Does it improve UX?

If you can't answer "yes" to at least 4/5 → **Refactor**

---

## Code Review Checklist

Before committing, verify:

- [ ] **Simple** - No unnecessary complexity
- [ ] **Maintainable** - Clear naming, clear structure
- [ ] **Scalable** - Modular, reusable, performant
- [ ] **Standard** - Follows BEM, mobile-first, web standards
- [ ] **User-friendly** - Mobile tested, accessible, fast

---

## Quotes to Remember

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand."
> — Martin Fowler

> "Simplicity is prerequisite for reliability."
> — Edsger W. Dijkstra

> "Make it work, make it right, make it fast."
> — Kent Beck

---

## Red Flags

If you see any of these, **stop and refactor**:

🚩 Function longer than 30 lines
🚩 Nested more than 3 levels deep
🚩 Copy-pasted code (extract to function)
🚩 Magic numbers (use named constants)
🚩 Global variables (use parameters)
🚩 Direct Firebase calls (use wrapper)
🚩 Inline styles (use CSS classes)
🚩 `!important` (fix specificity)
🚩 Hardcoded values (use CSS variables)
🚩 No error handling (add try-catch)

---

## Remember

**The best code is no code.**

Before adding:

1. Can I reuse existing code?
2. Can I simplify existing code?
3. Do I really need this?

If yes to any → **Don't add more code**
