---
prompt: quality-checklist
project: join
category: quality
---

# Quality Checklist - Join Project

## Definition of Done (DoD)

Complete this checklist before submitting the project.

---

## 📋 General Requirements

### Project Completion

- [ ] All **User Stories** completed
- [ ] All **Acceptance Criteria** met
- [ ] All features work **error-free** as expected
- [ ] Before submission: **Minimum 5 realistic tasks** and **10 contacts** added to database

### Testing

- [ ] All functionalities **manually tested** by all team members
- [ ] Tested on latest versions of:
  - [ ] Google Chrome
  - [ ] Mozilla Firefox
  - [ ] Safari
  - [ ] Microsoft Edge
- [ ] Tested on **mobile devices** (iOS and Android)
- [ ] Tested on **desktop** (Windows, macOS, Linux)

---

## 🐙 GitHub Requirements

### Repository Setup

- [ ] GitHub used **from the beginning**
- [ ] Repository is **public**
- [ ] Link to GitHub repository provided with submission
- [ ] `.gitignore` file excludes unnecessary files (e.g., `node_modules/`, `.env`, `.DS_Store`)

### Commit History

- [ ] **Regular commits** from each team member (minimum 1 commit per work session)
- [ ] **Descriptive commit messages** (e.g., "Add task creation form", "Fix drag and drop bug")
- [ ] Commits distributed across development timeline (not all on last day)

### Post-Collaboration

- [ ] After group work completion, **each member forks** the repository

---

## 🎨 User Experience (UX)

### User Feedback

- [ ] Users receive **intuitive feedback** for interactions:
  - [ ] **Hover effects** on clickable elements
  - [ ] **Toast messages** for success/error notifications
  - [ ] **Loading states** (disabled buttons, spinners)
  - [ ] **Form validation** messages

### Design Consistency

- [ ] All UI elements match **Figma design**:
  - [ ] Colors (exact hex values)
  - [ ] Spacing (padding, margins)
  - [ ] Shadows
  - [ ] Border radius
  - [ ] Font sizes and weights

### Transitions

- [ ] Transitions on clickable elements between **75ms and 125ms**
- [ ] No jarring animations (smooth, subtle effects)

### Mobile Optimization

- [ ] Join works on **mobile devices**
- [ ] Kanban columns display **vertically** on mobile
- [ ] Touch-friendly UI (large tap targets)

### Interactive Elements

- [ ] All buttons have `cursor: pointer`
- [ ] All inputs and buttons have `border: unset` (no default borders)
- [ ] Focus states visible for accessibility

---

## 🛠️ Technical Requirements

### Architecture

- [ ] Join is a **Multi-Page Application (MPA)**
- [ ] No SPA frameworks (React, Vue, Angular, etc.)
- [ ] No client-side routing

### File Structure

- [ ] Structured and **consistent file names** and folder structure
- [ ] **Start page** is named `index.html`
- [ ] Feature-based organization (e.g., `js/board/`, `js/tasks/`)

### Code Quality

- [ ] **No console errors** in browser console
- [ ] **No console logs** left in production code (use for debugging only)
- [ ] **Max 400 lines** per file
- [ ] **Max 14 lines** per function
- [ ] All functions have **JSDoc comments**
- [ ] **camelCase** naming for JavaScript
- [ ] **BEM** naming for CSS

### Content Display

- [ ] Created content is **immediately visible** (no page reload required, unless intentional)

---

## 🎨 Design

### Buttons

- [ ] All buttons have `cursor: pointer`
- [ ] No default button borders (`border: unset`)

### Inputs

- [ ] All inputs have `border: unset`
- [ ] Custom focus styles applied

### Form Validation

- [ ] What happens when inputs are empty? (Error messages shown)
- [ ] Submit buttons disabled until form is valid
- [ ] Validation errors clearly displayed

---

## 📱 Responsiveness

### Viewport Testing

- [ ] Every page works at **every resolution down to 320px** (drag browser window smaller)
- [ ] Test on actual mobile devices (iOS and Android)

### Content Constraints

- [ ] **Content max-width** set for large monitors (e.g., 1920px, left-aligned)
- [ ] Design elements (backgrounds, headers) can be full-width

### Mobile and Desktop

- [ ] Every page works on **mobile** and **desktop**
- [ ] Landscape mode **disabled on mobile** (unless specifically optimized)

### Scrollbars

- [ ] **No horizontal scrollbars** at any resolution

---

## 💻 Technical Implementation

### Architecture

- [ ] Join is a **Multi-Page Application (MPA)**
- [ ] Each page is a separate HTML file
- [ ] Navigation causes full page reloads

### File Naming

- [ ] File names are **descriptive** and **consistent**
- [ ] JavaScript: `feature__action.js` or `service.js`
- [ ] CSS: `feature.css` or `component.css`

### JavaScript File Structure

- [ ] **Minimum one JS file per page** (e.g., `board__init.js`)
- [ ] **One shared JS file** for common utilities (e.g., `ui-helpers.js`)

### CSS File Structure

- [ ] Base styles (reset, variables, typography)
- [ ] Layout styles (header, menu, footer)
- [ ] Component styles (button, card, form)
- [ ] Page-specific styles (board, contacts, etc.)

---

## 📝 Forms

### Validation

- [ ] **Form validation** implemented for all forms
- [ ] Error messages shown for invalid inputs

### User Feedback

- [ ] Created content **directly visible** after submission
- [ ] Success toast messages shown

### Submit Buttons

- [ ] Submit buttons **disabled during loading** (prevent duplicate submissions)

### Assigned-to Field

- [ ] Dropdown menu **automatically closes** when clicking outside
- [ ] **Contacts** can be selected (not users) - easier for testing

### Subtask Field

- [ ] Pressing **Enter** creates subtask (does NOT submit main task)
- [ ] Subtask input field clears after adding

---

## 🧩 JavaScript / Clean Code

### Function Rules

- [ ] Each function has **one task only**
- [ ] Each function is **max 14 lines** (excluding JSDoc)
- [ ] **Descriptive function names** (e.g., `createTask`, `validateEmail`)

### Naming Conventions

- [ ] **camelCase** for file names, variables, and functions
  - ✅ Correct: `shoppingCart`, `createTask`
  - ❌ Wrong: `Shopping_Cart`, `create_task`
- [ ] First letter of functions/variables is **lowercase**

### Code Style

- [ ] **2 empty lines** between functions
- [ ] **Max 400 lines** per file
- [ ] Files named correctly: `index.html`, `script.js`, `style.css`

### HTML in JavaScript

- [ ] HTML code extracted into **separate render functions** (not inline)
- [ ] **Extra folders** for templates and images (`assets/templates/`, `assets/img/`)

### Static vs. Dynamic HTML

- [ ] **Static HTML** not generated via JavaScript (use real HTML files)
- [ ] **Dynamic content** (tasks, contacts) rendered via JavaScript

### Documentation

- [ ] All functions documented with **JSDoc** standard
- [ ] See: https://jsdoc.app/about-getting-started.html

---

## ⚠️ Avoid Common Mistakes

### UI Issues

- [ ] Menu items **don't shift** when hovered
- [ ] No visual glitches or layout shifts

### Drag & Drop

- [ ] Tasks **don't disappear** when dragged
- [ ] Visual feedback during drag (opacity, rotation)

### User Feedback

- [ ] User feedback shown when something is **saved/changed** (toast messages)

### Board Columns

- [ ] Columns **don't extend too far down** (min-height, not height: 100vh)
- [ ] Columns have proper spacing and alignment

### Form Validation

- [ ] Form validation on **Add Contact / Edit Contact**
- [ ] Error messages shown for invalid inputs

### Content Overflow

- [ ] **No content "running out"** of containers (subtasks, contacts, etc.)
- [ ] Proper overflow handling (`overflow: auto`, max-height)

---

## ✅ Functionality - User Stories Checklist

### 1. User Account & Administration

- [ ] User registration with email, password, name
- [ ] Privacy policy must be accepted before registration
- [ ] Login with email and password
- [ ] Guest login available
- [ ] Unauthenticated users redirected to login page
- [ ] Logout functionality
- [ ] After logout, redirected to login page
- [ ] Summary dashboard shows task statistics
- [ ] Greeting message based on time of day

### 2. Kanban Board & Task Management

- [ ] Board displays four columns: To Do, In Progress, Awaiting Feedback, Done
- [ ] Empty columns show "No tasks" message
- [ ] Task cards show category, title, description, assigned users, priority
- [ ] Clicking task opens full details
- [ ] "+" icon in each column to add task
- [ ] Subtask progress visualized on task cards
- [ ] Search functionality filters tasks in real-time
- [ ] Add task form with all required fields (title, due date, category, priority, etc.)
- [ ] Medium priority pre-selected by default
- [ ] Submit button disabled until all required fields filled
- [ ] Subtasks can be added by pressing Enter or clicking checkmark
- [ ] Subtasks can be edited and deleted
- [ ] Task detail view shows all information
- [ ] Edit task (all fields except category)
- [ ] Delete task (with confirmation)
- [ ] Drag & drop tasks between columns (desktop)
- [ ] Visual feedback during drag (rotation, opacity, column highlight)
- [ ] Mobile: Columns displayed vertically
- [ ] Mobile: Alternative drag method (long press or popup menu)

### 3. Contact Management

- [ ] Contacts displayed alphabetically
- [ ] Contacts grouped by first letter (A, B, C, etc.)
- [ ] Clicking contact shows detail view
- [ ] Detail view shows name, email, phone
- [ ] Add contact form with validation
- [ ] Edit contact (pre-filled form)
- [ ] Delete contact (with confirmation)
- [ ] Deleted contact removed from all tasks
- [ ] Own user account visible and editable in contact list

### 4. Legal Information

- [ ] **Legal Notice** page with realistic information (no Lorem Ipsum)
- [ ] **Privacy Policy** page with realistic information (no Lorem Ipsum)
- [ ] Links to legal pages accessible from all pages (footer)

---

## 🚀 Pre-Submission Checklist

### Final Testing

- [ ] Test all features on **all browsers** (Chrome, Firefox, Safari, Edge)
- [ ] Test on **mobile devices** (iOS, Android)
- [ ] Test on **desktop** (Windows, macOS, Linux)
- [ ] Clear browser cache and test again
- [ ] Test with **guest login**
- [ ] Test with **new user registration**

### Data Preparation

- [ ] **Minimum 5 realistic tasks** in database
- [ ] **Minimum 10 contacts** in database
- [ ] Tasks have realistic titles, descriptions, due dates
- [ ] Contacts have realistic names, emails, phone numbers

### Code Review

- [ ] No console errors
- [ ] No console logs in production code
- [ ] All functions ≤ 14 lines
- [ ] All files ≤ 400 LOC
- [ ] All functions have JSDoc
- [ ] BEM naming in CSS
- [ ] camelCase naming in JavaScript

### Documentation

- [ ] README.md updated with project description
- [ ] README.md includes setup instructions
- [ ] README.md includes tech stack information

### GitHub

- [ ] Repository is public
- [ ] All team members have commits
- [ ] Commit messages are descriptive
- [ ] `.gitignore` properly configured

---

## 📊 Success Criteria Summary

| Category            | Criteria                                  | Status |
| ------------------- | ----------------------------------------- | ------ |
| **Functionality**   | All user stories completed                | ☐      |
| **Code Quality**    | Max 14 lines/function, max 400 lines/file | ☐      |
| **Documentation**   | All functions have JSDoc                  | ☐      |
| **Design**          | Matches Figma exactly                     | ☐      |
| **Responsiveness**  | Works 320px - 1920px+                     | ☐      |
| **Testing**         | Tested on all main browsers               | ☐      |
| **GitHub**          | Regular commits, public repo              | ☐      |
| **User Experience** | Intuitive feedback, smooth transitions    | ☐      |

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
