---
agent: code-reviewer
---

# Automated Code Review Agent

## Purpose

Automatically review code changes in pull requests and commits against Join coding standards.

## Scope

Review the following aspects:

- Function length compliance (≤ 14 lines)
- File size limits (≤ 400 LOC)
- ES6 module compliance (no classes, no window globals)
- BEM naming in CSS files
- JSDoc documentation presence
- Firebase error handling
- Form validation patterns
- Proper responsive design (mobile-first)

## Review Checklist

### JavaScript Files

- [ ] All functions ≤ 14 lines
- [ ] No ES6 classes used
- [ ] No window globals (use ES6 imports/exports)
- [ ] Explicit JSDoc comments on all functions
- [ ] Exports at end of file
- [ ] Proper error handling (try-catch for async)
- [ ] camelCase naming for variables and functions
- [ ] Async/await (no .then() chains)

### Service Files

- [ ] File size ≤ 400 LOC
- [ ] Modular structure if complex
- [ ] CRUD operations properly implemented
- [ ] serverTimestamp() used for dates
- [ ] Error handling in all async functions

### Page Files (HTML)

- [ ] Semantic HTML5 elements used
- [ ] Proper form structure and validation
- [ ] Templates loaded with include-html.js
- [ ] No inline styles

### CSS Files

- [ ] BEM naming convention followed
- [ ] CSS custom properties used (no hardcoded values)
- [ ] Mobile-first (media queries with min-width)
- [ ] Media queries inline (not at end of file)
- [ ] Max 3 levels of nesting
- [ ] No !important (unless justified)
- [ ] Transitions 75ms-125ms

### Firebase Integration

- [ ] serverTimestamp() used for dates
- [ ] Error handling for all operations
- [ ] No sensitive data in Firestore

## Actions

### On Violation Found

1. **Comment on the specific line** with:
   - Clear description of the violation
   - Reference to the relevant guideline (link to prompt file)
   - Suggested fix with code example
2. **Example Comment:**

   ````
   ⚠️ Function length violation

   This function has 18 lines, exceeding the 14-line limit.

   Guideline: .github/prompts/angular/01-coding-standards.md
   join/01-coding-standards.md

   Suggestion: Extract helper functions:

   ```javascript
   // Instead of:
   async function loadUserData(id) {
     this.isLoading = true;
     try {
       const user = await getUserById(id);
       const profile = await getProfile(user.profileId);
       const settings = await getSettings(profile.settingsId);
       const userData = {
         ...user,
         profile,
         settings
       };
       renderUserProfile(userData);
     } catch (error) {
       handleError(error);
     } finally {
       this.isLoading = false;
     }
   }

   // Refactor to:
   async function loadUserData(id) {
     this.isLoading = true;
     try {
       const userData = await fetchUserWithDetails(id);
       renderUserProfile(userData);
     } catch (error) {
       handleError(error);
     } finally {
       this.isLoading = false;
     }
   }

   async function fetchUserWithDetails(id) {
     const user = await getUserById(id);
     const profile = await getProfile(user.profileId);
     const settings = await ngs };
   }
   ````

   ```

   ```

### On Approval

If all checks pass:

```
✅ Code review passed

All coding standards met:
- Function lengths within limits
- ES6 modules properly used
- BEM naming conventions followed
- JSDoc documentation present
- Firebase error handlingnt
- Firebase cleanup patterns implemented
```

## Review Priority

1. \*\*JavaScript errors
   - Missing Firebase error handling
   - Security vulnerabilities
   - Breaking changes without migration

2. **High** (request changes):
   - Function length > 20 lines
   - File size > 500 LOC
   - Missing error handling
   - No JSDoc on functions
   - ES6 classes used
   - window globals used

3. **Medium** (suggest improvements):
   - Function length 15-20 lines
   - File size 400-500 LOC
   - Minor BEM violations
   - Missing form validation
   - Missing type exports

4. **Low** (optional):
   - Code style preferences
   - Performance optimizations
   - Additional documentation

## Reference Links

Automatically include rel./prompts/join/01-coding-standards.md)

- [Architecture](../../prompts/join/02-architecture.md)
- [Page Structure](../../prompts/join/03-page-structure.md)
- [Styling BEM](../../prompts/join/04-styling-bem.md)
- [Firebase Integration](../../prompts/join/05-firebase-integration.md)
- [Feature: Kanban Board](../../prompts/join/06-feature-kanban-board.md)
- [Feature: Task Management](../../prompts/join/07-feature-task-management.md)
- [Feature: User Auth](../../prompts/join/08-feature-user-auth.md)
- [Feature: Contact Management](../../prompts/join/09-feature-contact-management.md)
- [Quality Checklist](../../prompts/join/10-quality-checklist.md)

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
**Last Updated:** February 2026
**Project:** DABubble
