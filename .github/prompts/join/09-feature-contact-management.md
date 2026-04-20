---
prompt: feature-contact-management
project: join
category: feature
---

# Feature: Contact Management - Join Project

## Overview

Contact management allows users to view, add, edit, and delete contacts. Contacts are displayed alphabetically and can be assigned to tasks.

---

## User Stories

### User Story 1: View Contact List

**As a user, I want to see an alphabetically sorted list of all contacts.**

#### Acceptance Criteria

- [x] Contact page displays all contacts
- [x] Contacts sorted **alphabetically** by name
- [x] Email address shown below name
- [x] Contacts grouped by **first letter** (A, B, C, etc.)
- [x] Clicking a contact opens **detail view** with name, email, and phone

#### Implementation

```javascript
// js/contacts/contact__list.js

import { loadContacts } from "../services/contact.service.js";

/**
 * Initializes contact list
 */
async function initializeContactList() {
  const contacts = await loadContacts();
  renderContactList(contacts);
}

/**
 * Renders contact list
 * @param {Array} contacts - All contacts
 */
function renderContactList(contacts) {
  const sortedContacts = sortContactsAlphabetically(contacts);
  const groupedContacts = groupContactsByLetter(sortedContacts);

  const container = document.getElementById("contact-list");
  container.innerHTML = renderGroupedContacts(groupedContacts);
}

/**
 * Sorts contacts alphabetically by name
 * @param {Array} contacts - Contacts array
 * @returns {Array} Sorted contacts
 */
function sortContactsAlphabetically(contacts) {
  return contacts.sort((a, b) =>
    a.name.localeCompare(b.name, "de", { sensitivity: "base" }),
  );
}

/**
 * Groups contacts by first letter
 * @param {Array} contacts - Sorted contacts
 * @returns {Object} Grouped contacts
 */
function groupContactsByLetter(contacts) {
  const grouped = {};

  contacts.forEach((contact) => {
    const letter = contact.name[0].toUpperCase();

    if (!grouped[letter]) {
      grouped[letter] = [];
    }

    grouped[letter].push(contact);
  });

  return grouped;
}

/**
 * Renders grouped contacts HTML
 * @param {Object} groupedContacts - Contacts grouped by letter
 * @returns {string} HTML string
 */
function renderGroupedContacts(groupedContacts) {
  return Object.keys(groupedContacts)
    .sort()
    .map(
      (letter) => `
      <div class="contact-group">
        <h2 class="contact-group__letter">${letter}</h2>
        <div class="contact-group__separator"></div>
        <div class="contact-group__list">
          ${groupedContacts[letter].map(renderContactItem).join("")}
        </div>
      </div>
    `,
    )
    .join("");
}

/**
 * Renders a single contact item
 * @param {Object} contact - Contact object
 * @returns {string} HTML string
 */
function renderContactItem(contact) {
  return `
    <div
      class="contact-item"
      onclick="openContactDetails('${contact.id}')"
    >
      <div
        class="contact-item__avatar"
        style="background: ${contact.color}"
      >
        ${contact.initials}
      </div>
      <div class="contact-item__info">
        <span class="contact-item__name">${contact.name}</span>
        <span class="contact-item__email">${contact.email}</span>
      </div>
    </div>
  `;
}

initializeContactList();

export { initializeContactList };
```

---

### User Story 2: View Contact Details

**As a user, I want to view contact information (email, phone) to get in touch.**

#### Acceptance Criteria

- [x] Clicking a contact opens **detail view**
- [x] Detail view shows:
  - Name
  - Email address
  - Phone number
- [x] Detail view has **Edit** and **Delete** options

#### Implementation

```javascript
// js/contacts/contact__details.js

import { loadContactById } from "../services/contact.service.js";

/**
 * Opens contact detail view
 * @param {string} contactId - Contact ID
 */
async function openContactDetails(contactId) {
  const contact = await loadContactById(contactId);

  if (!contact) {
    showToast("Contact not found", "error");
    return;
  }

  renderContactDetails(contact);
}

/**
 * Renders contact details
 * @param {Object} contact - Contact object
 */
function renderContactDetails(contact) {
  const detailPanel = document.getElementById("contact-details");

  detailPanel.innerHTML = `
    <div class="contact-details">
      <div class="contact-details__header">
        <div
          class="contact-details__avatar"
          style="background: ${contact.color}"
        >
          ${contact.initials}
        </div>
        <h2 class="contact-details__name">${contact.name}</h2>
      </div>

      <div class="contact-details__info">
        <h3>Contact Information</h3>

        <div class="contact-details__field">
          <strong>Email:</strong>
          <a href="mailto:${contact.email}">${contact.email}</a>
        </div>

        <div class="contact-details__field">
          <strong>Phone:</strong>
          <a href="tel:${contact.phone}">${contact.phone}</a>
        </div>
      </div>

      <div class="contact-details__actions">
        <button
          class="button button--secondary"
          onclick="deleteContact('${contact.id}')"
        >
          <svg><!-- trash icon --></svg>
          Delete
        </button>
        <button
          class="button button--primary"
          onclick="editContact('${contact.id}')"
        >
          <svg><!-- pencil icon --></svg>
          Edit
        </button>
      </div>
    </div>
  `;

  detailPanel.style.display = "block";
}

export { openContactDetails };
```

---

### User Story 3: Add New Contact

**As a user, I want to add new contacts to work with them in Join.**

#### Acceptance Criteria

- [x] "Add Contact" button or icon visible
- [x] Form opens with fields:
  - **Name** (required)
  - **Email** (required)
  - **Phone** (required)
- [x] Form validates inputs
- [x] After submission, contact saved and shown in list
- [x] Contact gets **initials** and **random color** automatically

#### Implementation

```javascript
// js/contacts/contact__create.js

import { createContact } from "../services/contact.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Opens add contact modal
 */
function openAddContactModal() {
  const modal = document.getElementById("contact-modal");
  renderContactForm();
  modal.style.display = "block";
}

/**
 * Renders contact form
 * @param {Object} contact - Contact data (for edit mode)
 */
function renderContactForm(contact = null) {
  const modalContent = document.getElementById("contact-modal-content");

  const isEditMode = contact !== null;
  const title = isEditMode ? "Edit Contact" : "Add Contact";
  const buttonText = isEditMode ? "Save" : "Create";

  modalContent.innerHTML = `
    <div class="modal__header">
      <h2>${title}</h2>
      <button class="modal__close" onclick="closeModal()">×</button>
    </div>

    <form id="contact-form" class="form">
      <div class="form__group">
        <label for="contact-name" class="form__label">Name *</label>
        <input
          type="text"
          id="contact-name"
          class="form__input"
          placeholder="Enter name"
          value="${contact?.name || ""}"
          required
        >
        <span class="form__error" id="contact-name-error"></span>
      </div>

      <div class="form__group">
        <label for="contact-email" class="form__label">Email *</label>
        <input
          type="email"
          id="contact-email"
          class="form__input"
          placeholder="Enter email"
          value="${contact?.email || ""}"
          required
        >
        <span class="form__error" id="contact-email-error"></span>
      </div>

      <div class="form__group">
        <label for="contact-phone" class="form__label">Phone *</label>
        <input
          type="tel"
          id="contact-phone"
          class="form__input"
          placeholder="Enter phone number"
          value="${contact?.phone || ""}"
          required
        >
        <span class="form__error" id="contact-phone-error"></span>
      </div>

      <div class="form__actions">
        <button type="button" class="button button--secondary" onclick="closeModal()">
          Cancel
        </button>
        <button type="submit" class="button button--primary">
          ${buttonText}
        </button>
      </div>
    </form>
  `;

  const form = document.getElementById("contact-form");
  form.addEventListener("submit", (event) => {
    if (isEditMode) {
      handleEditContact(event, contact.id);
    } else {
      handleAddContact(event);
    }
  });
}

/**
 * Handles add contact form submission
 * @param {Event} event - Submit event
 */
async function handleAddContact(event) {
  event.preventDefault();

  const formData = getContactFormData();
  const validation = validateContactForm(formData);

  if (!validation.isValid) {
    displayValidationErrors(validation.errors);
    return;
  }

  try {
    await createContact(formData);
    showToast("Contact created successfully");
    closeModal();
    window.location.reload();
  } catch (error) {
    showToast("Failed to create contact", "error");
  }
}

/**
 * Gets contact form data
 * @returns {Object} Form data
 */
function getContactFormData() {
  return {
    name: document.getElementById("contact-name").value.trim(),
    email: document.getElementById("contact-email").value.trim(),
    phone: document.getElementById("contact-phone").value.trim(),
  };
}

export { openAddContactModal };
```

---

### User Story 4: Edit and Delete Contacts

**As a user, I want to edit or delete contacts to keep my contact list up to date.**

#### Acceptance Criteria

- [x] Detail view has **Edit** and **Delete** options
- [x] Edit option opens form with pre-filled data
- [x] Delete option removes contact from list
- [x] **Important:** Deleted contact is also **removed from all tasks** it was assigned to

#### Implementation

```javascript
// js/contacts/contact__edit.js

import { updateContact } from "../services/contact.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Opens edit contact modal
 * @param {string} contactId - Contact ID
 */
async function editContact(contactId) {
  const contact = await loadContactById(contactId);
  renderContactForm(contact);
}

/**
 * Handles edit contact form submission
 * @param {Event} event - Submit event
 * @param {string} contactId - Contact ID
 */
async function handleEditContact(event, contactId) {
  event.preventDefault();

  const formData = getContactFormData();
  const validation = validateContactForm(formData);

  if (!validation.isValid) {
    displayValidationErrors(validation.errors);
    return;
  }

  try {
    await updateContact(contactId, formData);
    showToast("Contact updated successfully");
    closeModal();
    window.location.reload();
  } catch (error) {
    showToast("Failed to update contact", "error");
  }
}

export { editContact };
```

```javascript
// js/contacts/contact__delete.js

import { deleteContact as deleteContactFromDB } from "../services/contact.service.js";
import { loadTasks, updateTask } from "../services/task.service.js";
import { showToast } from "../shared/ui-helpers.js";

/**
 * Deletes a contact
 * @param {string} contactId - Contact ID
 */
async function deleteContact(contactId) {
  const confirmed = confirm(
    "Are you sure you want to delete this contact? " +
      "They will be removed from all tasks.",
  );

  if (!confirmed) return;

  try {
    // Remove contact from all tasks
    await removeContactFromTasks(contactId);

    // Delete contact
    await deleteContactFromDB(contactId);

    showToast("Contact deleted successfully");
    window.location.reload();
  } catch (error) {
    showToast("Failed to delete contact", "error");
  }
}

/**
 * Removes contact from all tasks
 * @param {string} contactId - Contact ID
 */
async function removeContactFromTasks(contactId) {
  const tasks = await loadTasks();

  const tasksWithContact = tasks.filter(
    (task) => task.assignedTo && task.assignedTo.includes(contactId),
  );

  for (const task of tasksWithContact) {
    const updatedAssignedTo = task.assignedTo.filter((id) => id !== contactId);
    await updateTask(task.id, { assignedTo: updatedAssignedTo });
  }
}

export { deleteContact };
```

---

### User Story 5: Edit Own Account

**As a user, I want to edit my own account in the contact list.**

#### Acceptance Criteria

- [x] Own account visible in contact list
- [x] Can click and edit own contact like any other contact

#### Implementation

The current user is automatically added to the contacts collection during registration, so this works automatically.

---

## Contact Form Validation

```javascript
// js/contacts/contact__validation.js

/**
 * Validates contact form data
 * @param {Object} formData - Form data
 * @returns {Object} Validation result
 */
function validateContactForm(formData) {
  const errors = {};

  // Name validation
  if (!formData.name || formData.name.trim().length === 0) {
    errors.name = "Name is required";
  }

  if (formData.name.length < 2) {
    errors.name = "Name must be at least 2 characters";
  }

  // Email validation
  if (!formData.email || formData.email.trim().length === 0) {
    errors.email = "Email is required";
  }

  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailRegex.test(formData.email)) {
    errors.email = "Invalid email format";
  }

  // Phone validation
  if (!formData.phone || formData.phone.trim().length === 0) {
    errors.phone = "Phone number is required";
  }

  const phoneRegex = /^[\d\s\+\-\(\)]+$/;
  if (!phoneRegex.test(formData.phone)) {
    errors.phone = "Invalid phone number format";
  }

  return {
    isValid: Object.keys(errors).length === 0,
    errors,
  };
}

export { validateContactForm };
```

---

## Contacts Page HTML

```html
<!-- pages/contacts.html -->
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Contacts - Join</title>
    <link rel="stylesheet" href="../css/pages/contacts.css" />
  </head>
  <body>
    <div w3-include-html="../assets/templates/header.html"></div>
    <div w3-include-html="../assets/templates/menu.html"></div>

    <main class="contacts">
      <aside class="contacts__sidebar">
        <div class="contacts__header">
          <h1 class="contacts__title">Contacts</h1>
          <button
            class="button button--primary"
            onclick="openAddContactModal()"
          >
            <svg class="button__icon"><!-- + icon --></svg>
            Add Contact
          </button>
        </div>

        <div id="contact-list" class="contacts__list">
          <!-- Contacts rendered here -->
        </div>
      </aside>

      <section
        id="contact-details"
        class="contacts__details"
        style="display: none;"
      >
        <!-- Contact details rendered here -->
      </section>
    </main>

    <!-- Contact Modal -->
    <div id="contact-modal" class="modal" style="display: none;">
      <div class="modal__content" id="contact-modal-content">
        <!-- Form rendered here -->
      </div>
    </div>

    <script type="module" src="../js/contacts/contact__init.js"></script>
  </body>
</html>
```

---

## Checklist for Contact Management

- [ ] Contacts sorted alphabetically by name
- [ ] Contacts grouped by first letter (A, B, C, etc.)
- [ ] Email shown below name in list
- [ ] Clicking contact opens detail view
- [ ] Detail view shows name, email, phone
- [ ] "Add Contact" button opens form
- [ ] Contact form validates name, email, phone
- [ ] New contacts get initials and random color automatically
- [ ] Edit contact opens pre-filled form
- [ ] Delete contact requires confirmation
- [ ] Deleted contact removed from all tasks
- [ ] Own user account visible and editable in contact list

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
