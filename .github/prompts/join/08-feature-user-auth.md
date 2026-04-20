---
prompt: feature-user-auth
project: join
category: feature
---

# Feature: User Authentication & Summary - Join Project

## Overview

User authentication enables users to register, log in (including guest login), and log out. The summary page (dashboard) shows key metrics about tasks.

---

## User Stories: Authentication

### User Story 1: User Registration

**As a new user, I want to register to access Join.**

#### Acceptance Criteria

- [x] Registration form with fields:
  - **Name** (required)
  - **Email** (required)
  - **Password** (required)
  - **Repeat Password** (required)
- [x] User must accept **Privacy Policy** before registering
- [x] Email validation (valid email format)
- [x] Password validation (min 6 characters)
- [x] Passwords must match
- [x] Error messages shown for invalid inputs
- [x] "Register" button disabled until all fields valid and privacy policy accepted

#### Implementation

```javascript
// js/auth/auth__register.js

import { registerUser } from "../services/auth.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Initializes registration form
 */
function initializeRegisterForm() {
  const form = document.getElementById("register-form");
  const submitButton = document.getElementById("register-btn");
  const privacyCheckbox = document.getElementById("privacy-policy");

  form.addEventListener("input", () => {
    submitButton.disabled = !isFormValid();
  });

  privacyCheckbox.addEventListener("change", () => {
    submitButton.disabled = !isFormValid();
  });

  form.addEventListener("submit", handleRegisterSubmit);
}

/**
 * Checks if registration form is valid
 * @returns {boolean} True if valid
 */
function isFormValid() {
  const name = document.getElementById("register-name").value.trim();
  const email = document.getElementById("register-email").value.trim();
  const password = document.getElementById("register-password").value;
  const repeatPassword = document.getElementById(
    "register-repeat-password",
  ).value;
  const privacyAccepted = document.getElementById("privacy-policy").checked;

  return (
    name.length > 0 &&
    isValidEmail(email) &&
    password.length >= 6 &&
    password === repeatPassword &&
    privacyAccepted
  );
}

/**
 * Handles registration form submission
 * @param {Event} event - Submit event
 */
async function handleRegisterSubmit(event) {
  event.preventDefault();

  const name = document.getElementById("register-name").value.trim();
  const email = document.getElementById("register-email").value.trim();
  const password = document.getElementById("register-password").value;

  try {
    await registerUser(email, password, name);
    showToast("Registration successful!");
    window.location.href = "summary.html";
  } catch (error) {
    handleFirebaseError(error);
  }
}

/**
 * Validates email format
 * @param {string} email - Email address
 * @returns {boolean} True if valid
 */
function isValidEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

export { initializeRegisterForm };
```

---

### User Story 2: User Login

**As a user, I want to log in to access my dashboard and the Kanban board.**

#### Acceptance Criteria

- [x] Login form with **Email** and **Password** fields
- [x] Error message shown for incorrect email/password
- [x] **Guest Login** option available (tests all functionality)
- [x] Unauthenticated visitors redirected to login page

#### Implementation

```javascript
// js/auth/auth__login.js

import { loginUser, loginAsGuest } from "../services/auth.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Initializes login form
 */
function initializeLoginForm() {
  const form = document.getElementById("login-form");
  const guestButton = document.getElementById("guest-login-btn");

  form.addEventListener("submit", handleLoginSubmit);
  guestButton.addEventListener("click", handleGuestLogin);
}

/**
 * Handles login form submission
 * @param {Event} event - Submit event
 */
async function handleLoginSubmit(event) {
  event.preventDefault();

  const email = document.getElementById("login-email").value.trim();
  const password = document.getElementById("login-password").value;

  try {
    await loginUser(email, password);
    showToast("Login successful!");
    window.location.href = "summary.html";
  } catch (error) {
    handleFirebaseError(error);
  }
}

/**
 * Handles guest login
 */
async function handleGuestLogin() {
  try {
    await loginAsGuest();
    showToast("Logged in as Guest");
    window.location.href = "summary.html";
  } catch (error) {
    handleFirebaseError(error);
  }
}

export { initializeLoginForm };
```

---

### User Story 3: User Logout

**As a user, I want to log out so no one can access my account without my permission.**

#### Acceptance Criteria

- [x] "Logout" option visible in UI (header)
- [x] After logout, user redirected to login page
- [x] After logout, user data not accessible without re-login

#### Implementation

```javascript
// js/auth/auth__logout.js

import { logoutUser } from "../services/auth.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Initializes logout button
 */
function initializeLogoutButton() {
  const logoutButton = document.getElementById("logout-btn");

  if (logoutButton) {
    logoutButton.addEventListener("click", handleLogout);
  }
}

/**
 * Handles logout
 */
async function handleLogout() {
  const confirmed = confirm("Are you sure you want to log out?");

  if (!confirmed) return;

  try {
    await logoutUser();
    showToast("Logged out successfully");
    window.location.href = "index.html";
  } catch (error) {
    showToast("Logout failed", "error");
  }
}

export { initializeLogoutButton };
```

---

## User Story: Summary Dashboard

### User Story 4: Summary Dashboard

**As a user, I want to see key task metrics on the dashboard when I log in.**

#### Acceptance Criteria

- [x] Dashboard shows:
  - Number of tasks with next deadline
  - Number of tasks in **To Do**
  - Number of tasks in **In Progress**
  - Number of tasks in **Awaiting Feedback**
  - Number of tasks **Done**
  - **Total tasks** on board
  - **Urgent tasks** count
- [x] Greeting message based on time of day (e.g., "Good morning, [Name]")

#### Implementation

```javascript
// js/summary/summary__init.js

import { loadTasks } from "../services/task.service.js";
import { getCurrentUser } from "../services/auth.service.js";

/**
 * Initializes summary page
 */
async function initializeSummary() {
  const tasks = await loadTasks();
  const user = await getCurrentUser();

  renderGreeting(user);
  renderTaskStats(tasks);
}

/**
 * Renders greeting message
 * @param {Object} user - Current user
 */
function renderGreeting(user) {
  const greetingElement = document.getElementById("greeting");
  const timeOfDay = getTimeOfDay();
  const userName = user?.displayName || "Guest";

  greetingElement.textContent = `Good ${timeOfDay}, ${userName}`;
}

/**
 * Gets time of day
 * @returns {string} "morning", "afternoon", or "evening"
 */
function getTimeOfDay() {
  const hour = new Date().getHours();

  if (hour < 12) return "morning";
  if (hour < 18) return "afternoon";
  return "evening";
}

/**
 * Renders task statistics
 * @param {Array} tasks - All tasks
 */
function renderTaskStats(tasks) {
  const stats = {
    todo: tasks.filter((t) => t.status === "todo").length,
    inProgress: tasks.filter((t) => t.status === "in-progress").length,
    awaitingFeedback: tasks.filter((t) => t.status === "awaiting-feedback")
      .length,
    done: tasks.filter((t) => t.status === "done").length,
    urgent: tasks.filter((t) => t.priority === "urgent").length,
    total: tasks.length,
  };

  const nextDeadline = getNextDeadline(tasks);

  document.getElementById("stat-todo").textContent = stats.todo;
  document.getElementById("stat-in-progress").textContent = stats.inProgress;
  document.getElementById("stat-awaiting-feedback").textContent =
    stats.awaitingFeedback;
  document.getElementById("stat-done").textContent = stats.done;
  document.getElementById("stat-urgent").textContent = stats.urgent;
  document.getElementById("stat-total").textContent = stats.total;
  document.getElementById("stat-next-deadline").textContent = nextDeadline;
}

/**
 * Gets next deadline date
 * @param {Array} tasks - All tasks
 * @returns {string} Formatted date or "No upcoming deadlines"
 */
function getNextDeadline(tasks) {
  const upcomingTasks = tasks
    .filter((t) => t.status !== "done")
    .sort((a, b) => new Date(a.dueDate) - new Date(b.dueDate));

  if (upcomingTasks.length === 0) {
    return "No upcoming deadlines";
  }

  return formatDate(upcomingTasks[0].dueDate);
}

initializeSummary();
```

---

## Summary Page HTML

```html
<!-- pages/summary.html -->
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Summary - Join</title>
    <link rel="stylesheet" href="../css/pages/summary.css" />
  </head>
  <body>
    <div w3-include-html="../assets/templates/header.html"></div>
    <div w3-include-html="../assets/templates/menu.html"></div>

    <main class="summary">
      <div class="summary__header">
        <h1 id="greeting">Good morning</h1>
      </div>

      <section class="summary__stats">
        <div class="stat-card">
          <div class="stat-card__icon">📋</div>
          <div class="stat-card__content">
            <span class="stat-card__number" id="stat-todo">0</span>
            <span class="stat-card__label">To Do</span>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-card__icon">⏳</div>
          <div class="stat-card__content">
            <span class="stat-card__number" id="stat-in-progress">0</span>
            <span class="stat-card__label">In Progress</span>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-card__icon">⏸️</div>
          <div class="stat-card__content">
            <span class="stat-card__number" id="stat-awaiting-feedback">0</span>
            <span class="stat-card__label">Awaiting Feedback</span>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-card__icon">✅</div>
          <div class="stat-card__content">
            <span class="stat-card__number" id="stat-done">0</span>
            <span class="stat-card__label">Done</span>
          </div>
        </div>

        <div class="stat-card stat-card--urgent">
          <div class="stat-card__icon">🔥</div>
          <div class="stat-card__content">
            <span class="stat-card__number" id="stat-urgent">0</span>
            <span class="stat-card__label">Urgent</span>
          </div>
        </div>

        <div class="stat-card stat-card--deadline">
          <div class="stat-card__content">
            <span class="stat-card__label">Next Deadline</span>
            <span class="stat-card__date" id="stat-next-deadline">-</span>
          </div>
        </div>
      </section>
    </main>

    <script type="module" src="../js/summary/summary__init.js"></script>
  </body>
</html>
```

---

## Login Page HTML

```html
<!-- pages/index.html -->
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Login - Join</title>
    <link rel="stylesheet" href="../css/pages/login.css" />
  </head>
  <body>
    <main class="login">
      <div class="login__container">
        <h1 class="login__title">Login</h1>

        <form id="login-form" class="form">
          <div class="form__group">
            <label for="login-email" class="form__label">Email</label>
            <input
              type="email"
              id="login-email"
              class="form__input"
              placeholder="Enter your email"
              required
            />
          </div>

          <div class="form__group">
            <label for="login-password" class="form__label">Password</label>
            <input
              type="password"
              id="login-password"
              class="form__input"
              placeholder="Enter your password"
              required
            />
          </div>

          <div class="form__actions">
            <button type="submit" class="button button--primary">Login</button>
            <button
              type="button"
              id="guest-login-btn"
              class="button button--secondary"
            >
              Guest Login
            </button>
          </div>
        </form>

        <p class="login__footer">
          Don't have an account?
          <a href="register.html" class="login__link">Sign up</a>
        </p>
      </div>
    </main>

    <script type="module" src="../js/auth/auth__login.js"></script>
  </body>
</html>
```

---

## Checklist for Authentication & Summary

### Authentication

- [ ] Registration form with name, email, password, repeat password
- [ ] Privacy policy checkbox required before registration
- [ ] Email validation (valid format)
- [ ] Password validation (min 6 characters)
- [ ] Passwords must match
- [ ] Register button disabled until valid
- [ ] Login form with email and password
- [ ] Guest login option available
- [ ] Logout button in header
- [ ] After logout, redirected to login page
- [ ] Unauthenticated users redirected to login

### Summary Dashboard

- [ ] Greeting message based on time of day
- [ ] Shows task count for each status (To Do, In Progress, etc.)
- [ ] Shows urgent tasks count
- [ ] Shows next deadline date
- [ ] Shows total tasks
- [ ] Summary page loads after successful login

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
