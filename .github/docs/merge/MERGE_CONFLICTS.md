# Merge Conflicts Preview

## 🔴 KONFLIKT #1: scripts/contacts.js

### DEINE VERSION (Cleanup):

```javascript
// Zeile 58
function renderContact(container, contact, index) {
    let initial = getInitials(contact.name, "");  // ← utilities.js
    container.innerHTML += templateContact(initial, contact.name, contact.email, index);
}

// Zeile 62-73: GELÖSCHT
// function getInitial(name) { ... }

// Zeile 74
function showContactDetails(index) {
    let contact = loadedContacts[index];
    let detailsContainer = document.getElementById("contacts-detail");
    let initial = getInitials(contact.name, "");  // ← utilities.js
    let badgeColor = document.querySelectorAll('.badge')[index].style.backgroundColor;
    ...
}
```

### KOLLEGEN VERSION (vermutlich):

```javascript
// Zeile 58
function renderContact(container, contact, index) {
    let initial = getInitial(contact.name);  // ← lokale Funktion
    container.innerHTML += templateContact(initial, contact.name, contact.email, index);
}

// Zeile 62-73: VORHANDEN
function getInitial(name) {
    if (!name) return "";
    let parts = name.trim().split(/\s+/);
    let first = parts[0].charAt(0);
    let last = parts.length > 1 ? parts[parts.length - 1].charAt(0) : "";
    return (first + last).toUpperCase();
}

// Zeile 74
function showContactDetails(index) {
    let contact = loadedContacts[index];
    let detailsContainer = document.getElementById("contacts-detail");
    let initial = getInitial(contact.name);  // ← lokale Funktion
    let badgeColor = document.querySelectorAll('.badge')[index].style.backgroundColor;
    ...
}
```

### ✅ LÖSUNG (Option A):

```javascript
// 1. Alte Funktion löschen:
// function getInitial(name) { ... }  ← LÖSCHEN

// 2. Alle Aufrufe ändern:
let initial = getInitials(contact.name, "");

// 3. In contacts.html utilities.js einbinden
<script src="../scripts/shared/utilities.js" defer></script>;
```

---

## 🔴 KONFLIKT #2: pages/contacts.html

### DEINE VERSION:

```html
<script src="../scripts/shared/utilities.js" defer></script>
<script src="../scripts/shared/include-html.js" defer></script>
<script src="../scripts/shared/init-template.js" defer></script>
```

### KOLLEGEN VERSION:

```html
<script src="../scripts/shared/include-html.js" defer></script>
<script src="../scripts/shared/init-template.js" defer></script>
```

### ✅ LÖSUNG:

```html
<!-- BEIDE Versionen kombinieren: -->
<script src="../scripts/shared/utilities.js" defer></script>
<script src="../scripts/shared/include-html.js" defer></script>
<script src="../scripts/shared/init-template.js" defer></script>
```

---

## 🟢 KEIN KONFLIKT: scripts/shared/init-template.js

Ihr habt diese Datei vermutlich nicht geändert.
→ Meine Version wird übernommen (nutzt getInitials() aus utilities.js)

---

## 🛠️ GIT MERGE COMMANDS

```bash
# Im Meeting zusammen am Bildschirm:

# 1. Status checken
git status
git log --oneline --graph --all -10

# 2. Merge starten
git checkout develop
git merge feature/cleanup-refactoring

# 3. Konflikt wird angezeigt:
# CONFLICT (content): Merge conflict in scripts/contacts.js
# CONFLICT (content): Merge conflict in pages/contacts.html

# 4. Konflikte anschauen
git diff --name-only --diff-filter=U

# 5. In VS Code öffnen - nutzt die eingebaute Merge-Tool UI
code scripts/contacts.js
code pages/contacts.html

# 6. Konflikte manuell lösen (siehe oben)

# 7. Nach Lösung markieren
git add scripts/contacts.js
git add pages/contacts.html

# 8. Merge abschließen
git commit -m "merge: Integrate cleanup-refactoring with contacts features

- Resolved conflicts in contacts.js (use shared getInitials)
- Added utilities.js to contacts.html
- All tests passing"

# 9. Testen (siehe Checklist in MERGE_GUIDE.md)

# 10. Push
git push origin develop
```

---

## 🎯 ALTERNATIVE: 3-Way Merge Tool

Falls ihr VisDiff/Meld/P4Merge nutzt:

```bash
git mergetool

# Wählt dann für contacts.js:
# - LEFT (eure Contacts-Features)
# - MIDDLE (gemeinsamer Ancestor)
# - RIGHT (meine utilities.js)
# - RESULT (kombiniert)
```

---

## ⚡ SUPER-SCHNELL-VERSION

**Wenn ihr KEINE Zeit habt:**

```bash
# Option B wählen - ich mache das:
git checkout feature/cleanup-refactoring
git revert HEAD~3  # utilities.js Commits rückgängig
git push origin feature/cleanup-refactoring

# Dann mergen - keine Konflikte!
git checkout develop
git merge feature/cleanup-refactoring
```

**Ihr müsst dann NICHTS ändern!**
