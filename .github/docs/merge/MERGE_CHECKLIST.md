# 📋 MERGE CHECKLISTE - Montag 9. Feb 2026

## VOR DEM MEETING (5 Min)

### Jeder im Team:

- [ ] `git status` - Uncommitted work sichern
- [ ] `git stash` falls nötig
- [ ] `git checkout develop && git pull`
- [ ] Branch-Status prüfen: `git log --oneline --graph -5`

---

## IM MEETING: ENTSCHEIDUNG (2 Min)

### Option wählen:

**[ ] Option A - Shared utilities.js nutzen (EMPFOHLEN)**

- ✅ Bessere Code-Qualität
- ⚠️ Anpassung in contacts.js nötig (~5 Min)

**[ ] Option B - Getrennte Implementierungen**

- ✅ Schnell (kein Konflikt)
- ⚠️ Code-Duplikate bleiben

---

## MERGE PROZESS (10 Min)

### Falls Option A:

```bash
# 1. Merge starten
git checkout develop
git merge feature/cleanup-refactoring

# 2. Konflikte sehen
git status

# 3. Lösen in VS Code oder:
# contacts.js:
#   - getInitial() → getInitials()
#   - function getInitial() löschen
# contacts.html:
#   - utilities.js Script hinzufügen

# 4. Abschließen
git add .
git commit
git push
```

### Falls Option B:

```bash
# Einfacher Merge - keine Konflikte
git checkout develop
git merge feature/cleanup-refactoring
git push
```

---

## TESTING (5 Min)

### Alle testen:

**Contacts-Seite:**

- [ ] Seite lädt ohne Fehler
- [ ] Kontakte angezeigt mit korrekten Initialen
- [ ] Add Contact funktioniert
- [ ] Edit Contact funktioniert
- [ ] Delete Contact funktioniert

**Summary/Header:**

- [ ] Avatar zeigt User-Initialen
- [ ] Dropdown funktioniert

**Browser Console:**

- [ ] Keine Errors
- [ ] getInitials ist definiert

---

## ROLLBACK (falls nötig)

```bash
git reset --hard HEAD~1
git push origin develop --force
```

---

## ✅ FERTIG!

**Nächster Schritt:** Weiterarbeiten an Features

**Docs:** MERGE_GUIDE.md, MERGE_CONFLICTS.md
