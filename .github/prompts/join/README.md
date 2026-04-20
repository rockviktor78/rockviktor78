---
prompt: index
project: join
category: documentation
---

# Join Project - AI Prompts & Guidelines

Structured prompts and guidelines for the Join project to assist AI assistants (GitHub Copilot, etc.) in understanding project requirements and standards.

---

## 📁 Prompt Structure

The prompts are organized by topic for clarity:

### 1. [Coding Standards](01-coding-standards.md)

- Function rules (max 14 lines)
- File size limits (max 400 LOC)
- JSDoc documentation requirements
- ES6 modules (no classes)
- Error handling
- Naming conventions (camelCase)

### 2. [Architecture](02-architecture.md)

- Multi-Page Application (MPA) structure
- ES6 modules (import/export)
- Service layer pattern
- Template loading system
- Project folder structure

### 3. [Page Structure](03-page-structure.md)

- Semantic HTML5
- Standard page templates
- Template system (include-html.js)
- Dynamic content rendering
- Form structure and validation
- UI feedback (toast messages)

### 4. [Styling & BEM](04-styling-bem.md)

- BEM methodology
- CSS custom properties (variables)
- Mobile-first responsive design
- Standard breakpoints
- Inline media queries
- Design requirements (Figma compliance)

### 5. [Firebase Integration](05-firebase-integration.md)

- Firestore database structure
- Service layer (CRUD operations)
- Authentication (login, register, guest, logout)
- Error handling
- Best practices (serverTimestamp, etc.)

### 6. [Feature: Kanban Board](06-feature-kanban-board.md)

- Display tasks in four columns
- Subtask progress visualization
- Search functionality
- Drag & drop (desktop & mobile)

### 7. [Feature: Task Management](07-feature-task-management.md)

- Add tasks with all details
- Manage subtasks
- Edit and delete tasks
- Form validation
- Assigned contacts dropdown

### 8. [Feature: User Auth & Summary](08-feature-user-auth.md)

- User registration
- Login (email/password & guest)
- Logout
- Summary dashboard with task statistics

### 9. [Feature: Contact Management](09-feature-contact-management.md)

- Alphabetically sorted contact list
- View contact details
- Add, edit, delete contacts
- Contact form validation

### 10. [Quality Checklist](10-quality-checklist.md)

- Definition of Done (DoD)
- Testing requirements
- GitHub guidelines
- User experience requirements
- Technical requirements
- Common mistakes to avoid
- Pre-submission checklist

---

## 🎯 Quick Reference

### Core Rules

```javascript
// ✅ CORRECT
function createTask(data) {
  return { ...data, id: generateId() };
}

// Export at end
export { createTask };
```

```javascript
// ❌ WRONG
export class TaskManager {} // No classes!
window.createTask = createTask; // No globals!
```

### Function Limits

- **Max 14 lines** per function (excluding JSDoc)
- **Max 400 lines** per file
- **One task** per function

### File Structure

```
join-app/
├── config/          # Firebase config
├── services/        # Backend services
├── assets/          # Static resources
├── css/             # Stylesheets (BEM)
├── js/              # JavaScript modules
└── pages/           # HTML pages (MPA)
```

---

## 🤖 Using with AI Assistants

### For GitHub Copilot

1. **Open relevant prompt file** before coding
2. **Reference in comments**: `// See: .github/prompts/join/01-coding-standards.md`
3. **Ask specific questions**: "Create a task service following 05-firebase-integration.md"

### For Custom Agents

See [.github/agents/code-review/agent.md](../../agents/code-review/agent.md) for automated code review setup.

---

## 📝 Checklist Format

All prompts include:

- ✅ **Acceptance Criteria** - What must be done
- 💻 **Code Examples** - ✅ Correct and ❌ Wrong patterns
- ☐ **Checklists** - Verify implementation completeness

---

## 🔄 Updates

When project requirements change:

1. Update the relevant prompt file
2. Update this README if structure changes
3. Test with AI assistant to verify understanding

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
**Tech Stack:** Vanilla JS, Firebase, BEM, MPA
