---
prompt: firebase-integration
project: join
category: backend
---

# Firebase Integration - Join Project

## Firebase Services Used

Join uses the following Firebase services:

| Service              | Purpose                                      |
| -------------------- | -------------------------------------------- |
| **Firestore**        | NoSQL database for tasks, contacts, users    |
| **Firebase Auth**    | User authentication (email/password)         |
| **Firebase Hosting** | (Optional) Hosting for production deployment |

---

## Setup & Configuration

### Firebase Config

```javascript
// config/firebase-config.js

import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { getAuth } from "firebase/auth";

/**
 * Firebase configuration object
 * @type {Object}
 */
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID",
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

// Initialize services
const db = getFirestore(app);
const auth = getAuth(app);

export { db, auth };
```

---

## Firestore Database Structure

### Collections

```
join-app/
├── users/                          # User accounts
│   └── {userId}/
│       ├── email: string
│       ├── name: string
│       ├── createdAt: timestamp
│       └── updatedAt: timestamp
│
├── contacts/                       # Contact list (shared by all users)
│   └── {contactId}/
│       ├── name: string
│       ├── email: string
│       ├── phone: string
│       ├── initials: string
│       ├── color: string
│       └── createdAt: timestamp
│
└── tasks/                          # Task list (shared by all users)
    └── {taskId}/
        ├── title: string
        ├── description: string
        ├── category: string        # "Technical Task" | "User Story"
        ├── priority: string        # "low" | "medium" | "urgent"
        ├── status: string          # "todo" | "in-progress" | "awaiting-feedback" | "done"
        ├── dueDate: timestamp
        ├── assignedTo: array       # Array of contact IDs
        ├── subtasks: array         # Array of subtask objects
        ├── createdAt: timestamp
        └── updatedAt: timestamp
```

### Important Notes

- **Shared Data**: All users (including guest) share the same board, tasks, and contacts
- **No User Isolation**: There is NO user-specific data separation in this project
- **Guest Access**: Guest login has full access to all data

---

## Firestore Service Layer

### Basic CRUD Operations

```javascript
// services/firestore.service.js

import { db } from "../config/firebase-config.js";
import {
  collection,
  doc,
  getDocs,
  getDoc,
  addDoc,
  updateDoc,
  deleteDoc,
  serverTimestamp,
  query,
  where,
  orderBy,
} from "firebase/firestore";

/**
 * Creates a new document in a collection
 * @param {string} collectionName - Collection name
 * @param {Object} data - Document data
 * @returns {Promise<string>} Document ID
 */
async function createDocument(collectionName, data) {
  const docRef = await addDoc(collection(db, collectionName), {
    ...data,
    createdAt: serverTimestamp(),
    updatedAt: serverTimestamp(),
  });

  return docRef.id;
}

/**
 * Reads all documents from a collection
 * @param {string} collectionName - Collection name
 * @returns {Promise<Array>} Array of documents
 */
async function readAllDocuments(collectionName) {
  const snapshot = await getDocs(collection(db, collectionName));
  return snapshot.docs.map((doc) => ({
    id: doc.id,
    ...doc.data(),
  }));
}

/**
 * Reads a single document by ID
 * @param {string} collectionName - Collection name
 * @param {string} docId - Document ID
 * @returns {Promise<Object|null>} Document or null
 */
async function readDocument(collectionName, docId) {
  const docRef = doc(db, collectionName, docId);
  const docSnap = await getDoc(docRef);

  if (docSnap.exists()) {
    return { id: docSnap.id, ...docSnap.data() };
  }

  return null;
}

/**
 * Updates a document
 * @param {string} collectionName - Collection name
 * @param {string} docId - Document ID
 * @param {Object} updates - Update data
 * @returns {Promise<void>}
 */
async function updateDocument(collectionName, docId, updates) {
  const docRef = doc(db, collectionName, docId);
  await updateDoc(docRef, {
    ...updates,
    updatedAt: serverTimestamp(),
  });
}

/**
 * Deletes a document
 * @param {string} collectionName - Collection name
 * @param {string} docId - Document ID
 * @returns {Promise<void>}
 */
async function deleteDocument(collectionName, docId) {
  const docRef = doc(db, collectionName, docId);
  await deleteDoc(docRef);
}

export {
  createDocument,
  readAllDocuments,
  readDocument,
  updateDocument,
  deleteDocument,
};
```

---

## Task Service

```javascript
// services/task.service.js

import {
  createDocument,
  readAllDocuments,
  readDocument,
  updateDocument,
  deleteDocument,
} from "./firestore.service.js";

const TASKS_COLLECTION = "tasks";

/**
 * Creates a new task
 * @param {Object} taskData - Task data
 * @returns {Promise<string>} Task ID
 */
async function createTask(taskData) {
  return await createDocument(TASKS_COLLECTION, taskData);
}

/**
 * Loads all tasks
 * @returns {Promise<Array>} Array of tasks
 */
async function loadTasks() {
  return await readAllDocuments(TASKS_COLLECTION);
}

/**
 * Loads a single task by ID
 * @param {string} taskId - Task ID
 * @returns {Promise<Object|null>} Task or null
 */
async function loadTaskById(taskId) {
  return await readDocument(TASKS_COLLECTION, taskId);
}

/**
 * Updates task data
 * @param {string} taskId - Task ID
 * @param {Object} updates - Updated data
 * @returns {Promise<void>}
 */
async function updateTask(taskId, updates) {
  await updateDocument(TASKS_COLLECTION, taskId, updates);
}

/**
 * Deletes a task
 * @param {string} taskId - Task ID
 * @returns {Promise<void>}
 */
async function deleteTask(taskId) {
  await deleteDocument(TASKS_COLLECTION, taskId);
}

/**
 * Updates task status (for drag & drop)
 * @param {string} taskId - Task ID
 * @param {string} newStatus - New status
 * @returns {Promise<void>}
 */
async function updateTaskStatus(taskId, newStatus) {
  await updateDocument(TASKS_COLLECTION, taskId, { status: newStatus });
}

export {
  createTask,
  loadTasks,
  loadTaskById,
  updateTask,
  deleteTask,
  updateTaskStatus,
};
```

---

## Contact Service

```javascript
// services/contact.service.js

import {
  createDocument,
  readAllDocuments,
  readDocument,
  updateDocument,
  deleteDocument,
} from "./firestore.service.js";

const CONTACTS_COLLECTION = "contacts";

/**
 * Creates a new contact
 * @param {Object} contactData - Contact data
 * @returns {Promise<string>} Contact ID
 */
async function createContact(contactData) {
  const initials = generateInitials(contactData.name);
  const color = generateRandomColor();

  return await createDocument(CONTACTS_COLLECTION, {
    ...contactData,
    initials,
    color,
  });
}

/**
 * Loads all contacts
 * @returns {Promise<Array>} Array of contacts
 */
async function loadContacts() {
  return await readAllDocuments(CONTACTS_COLLECTION);
}

/**
 * Loads a single contact by ID
 * @param {string} contactId - Contact ID
 * @returns {Promise<Object|null>} Contact or null
 */
async function loadContactById(contactId) {
  return await readDocument(CONTACTS_COLLECTION, contactId);
}

/**
 * Updates contact data
 * @param {string} contactId - Contact ID
 * @param {Object} updates - Updated data
 * @returns {Promise<void>}
 */
async function updateContact(contactId, updates) {
  await updateDocument(CONTACTS_COLLECTION, contactId, updates);
}

/**
 * Deletes a contact
 * @param {string} contactId - Contact ID
 * @returns {Promise<void>}
 */
async function deleteContact(contactId) {
  await deleteDocument(CONTACTS_COLLECTION, contactId);
}

/**
 * Generates initials from name
 * @param {string} name - Full name
 * @returns {string} Initials
 */
function generateInitials(name) {
  const parts = name.trim().split(" ");
  const first = parts[0]?.[0] || "";
  const last = parts[parts.length - 1]?.[0] || "";
  return (first + last).toUpperCase();
}

/**
 * Generates random color for contact avatar
 * @returns {string} Hex color
 */
function generateRandomColor() {
  const colors = [
    "#FF7A00",
    "#FF5EB3",
    "#6E52FF",
    "#9327FF",
    "#00BEE8",
    "#1FD7C1",
    "#FF745E",
    "#FFA35E",
    "#FC71FF",
    "#FFC701",
    "#0038FF",
    "#C3FF2B",
  ];
  return colors[Math.floor(Math.random() * colors.length)];
}

export {
  createContact,
  loadContacts,
  loadContactById,
  updateContact,
  deleteContact,
};
```

---

## Authentication Service

```javascript
// services/auth.service.js

import { auth } from "../config/firebase-config.js";
import {
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
  signOut,
  onAuthStateChanged,
} from "firebase/auth";
import { createDocument } from "./firestore.service.js";

const GUEST_EMAIL = "guest@join.com";
const GUEST_PASSWORD = "guest123";

/**
 * Registers a new user
 * @param {string} email - User email
 * @param {string} password - User password
 * @param {string} name - User name
 * @returns {Promise<Object>} User object
 */
async function registerUser(email, password, name) {
  const userCredential = await createUserWithEmailAndPassword(
    auth,
    email,
    password,
  );

  const userId = userCredential.user.uid;

  await createDocument("users", {
    email,
    name,
    userId,
  });

  return userCredential.user;
}

/**
 * Logs in a user
 * @param {string} email - User email
 * @param {string} password - User password
 * @returns {Promise<Object>} User object
 */
async function loginUser(email, password) {
  const userCredential = await signInWithEmailAndPassword(
    auth,
    email,
    password,
  );

  return userCredential.user;
}

/**
 * Logs in as guest
 * @returns {Promise<Object>} User object
 */
async function loginAsGuest() {
  return await loginUser(GUEST_EMAIL, GUEST_PASSWORD);
}

/**
 * Logs out current user
 * @returns {Promise<void>}
 */
async function logoutUser() {
  await signOut(auth);
}

/**
 * Gets current authenticated user
 * @returns {Promise<Object|null>} User or null
 */
function getCurrentUser() {
  return new Promise((resolve) => {
    onAuthStateChanged(auth, (user) => {
      resolve(user);
    });
  });
}

export { registerUser, loginUser, loginAsGuest, logoutUser, getCurrentUser };
```

---

## Error Handling

### Firebase Error Codes

```javascript
// js/shared/error-handler.js

/**
 * Handles Firebase errors and shows user-friendly messages
 * @param {Error} error - Firebase error
 */
function handleFirebaseError(error) {
  let message = "An error occurred";

  switch (error.code) {
    // Auth Errors
    case "auth/email-already-in-use":
      message = "Email address is already registered";
      break;
    case "auth/invalid-email":
      message = "Invalid email address";
      break;
    case "auth/weak-password":
      message = "Password must be at least 6 characters";
      break;
    case "auth/user-not-found":
      message = "User not found";
      break;
    case "auth/wrong-password":
      message = "Incorrect password";
      break;

    // Firestore Errors
    case "permission-denied":
      message = "Permission denied";
      break;
    case "not-found":
      message = "Document not found";
      break;
    case "unavailable":
      message = "Service temporarily unavailable";
      break;

    default:
      message = error.message || "An error occurred";
  }

  console.error("Firebase Error:", error);
  showToast(message, "error");
}

export { handleFirebaseError };
```

---

## Authentication Guard

### Protecting Pages

```javascript
// js/auth/auth__guard.js

import { getCurrentUser } from "../services/auth.service.js";

/**
 * Checks if user is authenticated
 * Redirects to login if not
 */
async function checkAuthentication() {
  const user = await getCurrentUser();

  const publicPages = ["index.html", "register.html"];
  const currentPage = window.location.pathname.split("/").pop();

  if (!user && !publicPages.includes(currentPage)) {
    window.location.href = "index.html";
    return;
  }

  if (user && currentPage === "index.html") {
    window.location.href = "summary.html";
  }
}

checkAuthentication();
```

### Usage in Pages

```html
<!-- board.html -->
<script type="module" src="../js/auth/auth__guard.js"></script>
<script type="module" src="../js/board/board__init.js"></script>
```

---

## Best Practices

### Always Use serverTimestamp()

```javascript
// ✅ CORRECT: Use serverTimestamp for dates
import { serverTimestamp } from "firebase/firestore";

await addDoc(collection(db, "tasks"), {
  title: "New Task",
  createdAt: serverTimestamp(),
  updatedAt: serverTimestamp(),
});

// ❌ WRONG: Client-side dates
await addDoc(collection(db, "tasks"), {
  title: "New Task",
  createdAt: new Date(), // Client time, can be inaccurate
});
```

### Error Handling for All Operations

```javascript
// ✅ CORRECT: Error handling
async function createTask(taskData) {
  try {
    const taskId = await createDocument("tasks", taskData);
    showToast("Task created successfully");
    return taskId;
  } catch (error) {
    handleFirebaseError(error);
    throw error;
  }
}
```

### No Sensitive Data in Firestore

**Do NOT store sensitive information like passwords in Firestore.**

- Use Firebase Auth for password management
- Store only non-sensitive user data in Firestore

---

## Checklist for Firebase Integration

- [ ] Firebase initialized in `config/firebase-config.js`
- [ ] Service layer created (auth.service.js, firestore.service.js, etc.)
- [ ] All CRUD operations have error handling
- [ ] `serverTimestamp()` used for all timestamps
- [ ] Authentication guard implemented for protected pages
- [ ] Guest login configured
- [ ] Firebase errors handled with user-friendly messages
- [ ] No sensitive data stored in Firestore
- [ ] Shared data model (no user isolation) implemented

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
