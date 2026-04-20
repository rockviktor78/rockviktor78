# Copilot Instructions für Join Project

## Projektübersicht

**Join** ist ein Kanban Project Management Tool als **Multi-Page Application (MPA)**.

### Architektur

- MPA (Multi-Page Application) mit mehreren separaten HTML-Seiten
- Gemeinsames Board für alle User (inkl. Gast-Login)
- Gemeinsame Kontakte, Tasks, etc. für alle Benutzer

### Wichtige Seiten

- `index.html` - Login/Landing Page (muss so heißen für Standard-Load)
- `html/summary.html` - Dashboard mit Task-Übersicht
- `html/board.html` - Kanban Board
- `html/add_task.html` - Task erstellen
- `html/contacts.html` - Kontaktverwaltung
- `html/legal_notice.html` - Impressum
- `html/privacy_policy.html` - Datenschutzerklärung
- `html/help.html` - Hilfe

---

## CSS/Styling Guidelines

**Ausführliche Styling-Richtlinien siehe:** [.github/skills/styling.md](.github/skills/styling.md)

### Wichtigste Regeln (Kurzübersicht)

1. **BEM-Namenskonvention** für alle CSS-Klassen
   - Block: `.component-name`
   - Element: `.component-name__element`
   - Modifier: `.component-name--modifier`

2. **Kebab-case** für alle Klassennamen: `.task-card`, `.main-header`

3. **IDs/data-attributes** für JavaScript statt CSS-Klassen

4. **Konsistente Präfixe**: `main-`, `task-`, `contact-`, `board-`

5. **Design Tokens** via CSS Custom Properties verwenden

6. **Responsive**: Mobile First, Breakpoints bei 768px, 1024px, 1440px

Bei Unsicherheiten, siehe detaillierte Dokumentation in [.github/skills/styling.md](.github/skills/styling.md).

---

## Clean Code Richtlinien

### JavaScript Standards

#### Funktionen

```javascript
/**
 * Adds a new task to the board
 * @param {string} title - The task title
 * @param {string} category - Task category (technical/user-story)
 * @returns {Object} The created task object
 */
function addTaskToBoard(title, category) {
  // Maximal 14 Zeilen pro Funktion (ohne HTML)
  // Eine Funktion = eine Aufgabe
}
```

#### Naming Conventions

- **camelCase** für Variablen, Funktionen und Dateinamen: `shoppingCart`, `addTask.js`
- **Erster Buchstabe klein**: `getUserData()`, `taskList`
- **Beschreibende Namen**: `getUserById()` statt `get()`
- **Keine Unterstriche**: ❌ `Shopping_Cart`, ✅ `shoppingCart`

#### Code-Organisation

- **Maximal 400 Zeilen** pro Datei
- **2 Leerzeilen** zwischen Funktionen
- **JSDoc-Kommentare** für alle Funktionen ([JSDoc Standard](https://jsdoc.app/))
- Statischer HTML-Code wird **nicht über JavaScript generiert**
- Ggf. HTML-Code in separate Template-Funktion auslagern

- **Maximum 14 lines** per function
- **One task** per function
- **camelCase naming:** `getUserById`
- **Async/await** statt .then() chains
- **Error handling:** Try-catch für alle async functions
- **JSDoc comments:** Required für alle functions
- **Export am Ende:** `export { func1, func2 };`

#### Beispiel

```javascript
/**
 * Retrieves user data from Firebase
 * @param {string} userId - The user ID
 * @returns {Promise<Object>} User data object
 */
async function getUserData(userId) {
  const response = await fetch(`/api/users/${userId}`);
  return response.json();
}

/**
 * Updates task status on the board
 * @param {string} taskId - The task ID
 * @param {string} newStatus - New status (todo/in-progress/done)
 */
function updateTaskStatus(taskId, newStatus) {
  const task = getTaskById(taskId);
  task.status = newStatus;
  saveToFirebase(task);
  renderBoard();
}
```

### Dateistruktur

#### JavaScript

```
script.js                    # Allgemeine seitenübergreifende Funktionen
scripts/
  ├── add_task.js           # Spezifisch für add_task.html
  ├── board.js              # Spezifisch für board.html
  ├── contacts.js           # Spezifisch für contacts.html
  ├── summary.js            # Spezifisch für summary.html
  ├── login.js              # Spezifisch für Login
  ├── sign_up.js            # Spezifisch für Registrierung
  └── firebase.js           # Firebase-spezifische Funktionen
```

#### CSS

```
style.css                    # Allgemeine globale Styles
styles/
  ├── header.css            # Header-Komponente
  ├── menu.css              # Sidebar/Menü
  ├── components.css        # Wiederverwendbare UI-Komponenten
  ├── add_task.css          # Seiten-spezifische Styles
  ├── board.css
  ├── contacts.css
  ├── summary.css
  ├── login.css
  └── sign_up.css
```

#### Templates & Assets

```
assets/
  ├── img/                  # Alle Bilder
  └── templates/            # HTML-Templates
      ├── header.html
      ├── menu.html
      ├── task_form.html
      └── ...
```

---

## UI/UX Anforderungen

### Allgemein

- **Intuitives Feedback** bei Interaktionen (hover, toast-messages)
- **Transitions**: 75ms - 125ms für anklickbare Elemente
- **Buttons**: Immer `cursor: pointer;`
- **Inputs/Buttons**: `border: unset;` statt Standard-Border
- **Keine Konsolenfehler** oder logs in Production
- **Content sofort sichtbar**: Erstellter Content unmittelbar nach dem Speichern anzeigen

### Design-Tokens (aus Figma)

```css
/* Farben */
--color-primary: #4589ff;
--color-dark: #2a3647;
--color-light: #cdcdcd;
--color-urgent: #ff3d00;
--color-medium: #ffa800;
--color-low: #7ae229;

/* Shadows */
--shadow-card: 0 2px 4px rgba(0, 0, 0, 0.1);
--shadow-hover: 0 4px 8px rgba(0, 0, 0, 0.15);

/* Spacing */
--spacing-sm: 8px;
--spacing-md: 16px;
--spacing-lg: 24px;
--spacing-xl: 40px;
```

### Responsive Design

- **Min-width**: 320px
- **Max-width**: 1440px (Content-Begrenzung, linksbündig)
- **Mobile**: Bottom Navigation
- **Desktop**: Side Navigation (200px breit)
- **Breakpoints**:
  - Desktop: > 1024px
  - Tablet: 768px - 1024px
  - Mobile: < 768px
- **Keine horizontalen Scrollbalken**
- **Landscape-Modus deaktivieren** (außer speziell optimiert)

```css
/* Responsive Header Beispiel */
.main-header {
  height: 80px;
}

@media (max-width: 1024px) {
  .main-header {
    left: 0;
  }
}

@media (max-width: 768px) {
  .main-header {
    height: 60px;
  }
}
```

---

## Formular-Validierung

### Wichtig

- **Keine HTML5-Standard-Validierung** verwenden
- **Custom Validation** gemäß Figma-Design implementieren
- **Button deaktivieren** während der Ladezeit
- **Fehlermeldungen** bei falscher Eingabe anzeigen
- **Toast-Messages** bei erfolgreicher Aktion

### Assigned-to Feld

```javascript
// Dropdown muss sich schließen bei Klick außerhalb
document.addEventListener("click", (e) => {
  if (!dropdown.contains(e.target)) {
    closeDropdown();
  }
});
```

### Subtask-Feld

```javascript
// Enter-Taste fügt Subtask hinzu, erstellt NICHT den Haupt-Task
subtaskInput.addEventListener("keydown", (e) => {
  if (e.key === "Enter") {
    e.preventDefault(); // Verhindert Form-Submit
    addSubtask(e.target.value);
  }
});
```

### Validierungs-Beispiel

```javascript
/**
 * Validates the add task form
 * @returns {boolean} True if form is valid
 */
function validateTaskForm() {
  const title = document.getElementById("taskTitle").value;
  const dueDate = document.getElementById("dueDate").value;
  const category = document.getElementById("category").value;

  if (!title || !dueDate || !category) {
    showError("Please fill in all required fields");
    return false;
  }

  return true;
}
```

---

## Projekt-spezifische Features

### Task Management

#### Task-Struktur

```javascript
const task = {
  id: "task_123",
  title: "Implement login",
  description: "Create login page with validation",
  dueDate: "2026-01-30",
  priority: "urgent", // urgent | medium | low
  category: "technical", // technical | user-story
  status: "todo", // todo | in-progress | awaiting-feedback | done
  assignedTo: ["contact_1", "contact_2"],
  subtasks: [
    { id: "sub_1", title: "Create HTML", completed: false },
    { id: "sub_2", title: "Add validation", completed: true },
  ],
};
```

#### Drag & Drop

- **Desktop**: Drag & Drop zwischen Spalten
- **Mobile**: Popup-Menü mit Pfeil-Icon (wegen Safari-Probleme)
- **Visuelle Rückmeldung**: Leichte Rotation beim Dragging
- **Highlight**: Gestrichelte Box beim Hover über Spalte

```javascript
/**
 * Handles drag start event
 * @param {DragEvent} e - The drag event
 */
function handleDragStart(e) {
  e.target.classList.add("task-card--dragging");
  e.dataTransfer.effectAllowed = "move";
  e.dataTransfer.setData("text/html", e.target.id);
}
```

#### Subtask-Fortschritt

```html
<div class="task-card__progress">
  <div class="progress-bar">
    <div class="progress-bar__fill" style="width: 71%;"></div>
  </div>
  <span class="progress-bar__text">5 von 7 Subtasks</span>
</div>
```

### Kontakte

#### Kontakt-Struktur

```javascript
const contact = {
  id: "contact_123",
  name: "Sofia Müller",
  email: "sofia.mueller@example.com",
  phone: "+49 123 456789",
  initials: "SM",
  color: "#4589ff",
};
```

#### Alphabetische Sortierung

```javascript
/**
 * Groups contacts by first letter
 * @param {Array} contacts - Array of contact objects
 * @returns {Object} Contacts grouped by letter
 */
function groupContactsByLetter(contacts) {
  return contacts.reduce((groups, contact) => {
    const letter = contact.name[0].toUpperCase();
    if (!groups[letter]) groups[letter] = [];
    groups[letter].push(contact);
    return groups;
  }, {});
}
```

### Dashboard (Summary)

#### Metriken anzeigen

- Anzahl Tasks pro Status (To-do, In Progress, Awaiting Feedback, Done)
- Nächste Deadline
- Tageszeit-abhängige Begrüßung

```javascript
/**
 * Gets greeting based on time of day
 * @returns {string} Greeting message
 */
function getGreeting() {
  const hour = new Date().getHours();
  if (hour < 12) return "Good morning";
  if (hour < 18) return "Good afternoon";
  return "Good evening";
}
```

---

## GitHub Best Practices

### Commits

- **Regelmäßig committen**: Mindestens ein Commit pro Arbeitssitzung
- **Aussagekräftige Messages**: `Add task drag-and-drop feature` statt `update`
- **Conventional Commits** empfohlen:
  ```
  feat: Add contact search functionality
  fix: Resolve drag-and-drop Safari bug
  style: Update button hover effects
  refactor: Simplify task validation logic
  docs: Update README with setup instructions
  ```

### .gitignore

```gitignore
# Sensible Daten
.env
firebase-config.json
n8n-credentials.json

# Build
dist/
build/

# Dependencies
node_modules/

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db
```

### Repository

- **Public Repository** für Arbeitgeber-Sichtbarkeit
- **README.md** mit Setup-Anleitung und Features
- **Nach Abschluss**: Jedes Gruppenmitglied forkt das Projekt

---

## Häufige Fehler vermeiden

### ❌ Typische Fehler

1. **Menüpunkte verschieben sich beim Hover**

   ```css
   /* ✅ Lösung: Feste Breite/Padding */
   .nav-item {
     padding: 12px 16px;
   }
   ```

2. **Tasks verschwinden beim Drag**

   ```javascript
   // ✅ Lösung: Korrekte Event-Handler
   element.addEventListener("dragend", () => {
     element.classList.remove("task-card--dragging");
   });
   ```

3. **Kein User-Feedback**

   ```javascript
   // ✅ Lösung: Toast-Message
   function showToast(message) {
     // Toast anzeigen
   }
   ```

4. **Content läuft raus (overflow)**

   ```css
   /* ✅ Lösung: Overflow handhaben */
   .task-card__description {
     overflow: hidden;
     text-overflow: ellipsis;
     display: -webkit-box;
     -webkit-line-clamp: 3;
     -webkit-box-orient: vertical;
   }
   ```

5. **Fehlende Form-Validierung**
   ```javascript
   // ✅ Lösung: Custom Validation
   if (!validateContactForm()) {
     return;
   }
   ```

---

## Testing vor Abgabe

### Checkliste

- [ ] Alle User Stories implementiert
- [ ] Mindestens 5 realistische Tasks hinzugefügt
- [ ] Mindestens 10 Kontakte hinzugefügt
- [ ] Getestet in Chrome, Firefox, Safari, Edge (aktuelle Versionen)
- [ ] Responsive auf 320px bis 1440px getestet
- [ ] Keine Konsolenfehler
- [ ] Alle Funktionen manuell getestet
- [ ] Guest-Login funktioniert
- [ ] Drag & Drop funktioniert (Desktop)
- [ ] Formular-Validierungen funktionieren
- [ ] Toast-Messages werden angezeigt

---

## Quick Reference

### BEM für häufige Komponenten

```css
/* Login */
.login-form {
}
.login-form__input {
}
.login-form__button {
}
.login-form__error {
}

/* Task Card */
.task-card {
}
.task-card--urgent {
}
.task-card--medium {
}
.task-card--low {
}
.task-card__title {
}
.task-card__category {
}
.task-card__assignees {
}
.task-card__progress {
}

/* Board */
.board__column {
}
.board__column--todo {
}
.board__column--in-progress {
}
.board__column--awaiting-feedback {
}
.board__column--done {
}
.board__column-empty {
}

/* Contact */
.contact-card {
}
.contact-card__avatar {
}
.contact-card__info {
}
.contact-card__actions {
}
.contact-list__letter-group {
}
```

Bei Unsicherheiten, orientiere dich an existierenden Komponenten im Projekt.
