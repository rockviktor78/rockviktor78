# Branch Management - Git Workflow

## Branch wechseln, löschen und neu erstellen

### Szenario
Du möchtest einen Branch lokal und remote löschen und einen neuen Branch erstellen.

---

## Schritt-für-Schritt Anleitung

### 1. Zu develop wechseln
```bash
git checkout develop
```
**Warum?** Du musst auf einem anderen Branch sein, bevor du den zu löschenden Branch entfernen kannst.

### 2. Lokalen develop aktualisieren
```bash
git pull --rebase origin develop
```
**Warum?** Stelle sicher, dass dein lokaler develop auf dem neuesten Stand ist.

**Alternative ohne Rebase:**
```bash
git pull origin develop
```

### 3. Lokalen Branch löschen
```bash
git branch -d feature/layout-navigation
```
**Warum?** Entfernt den lokalen Branch aus deinem Repository.

**Hinweis:**
- `-d` löscht nur, wenn der Branch bereits gemerged wurde
- `-D` (force delete) löscht auch ungemerged Branches

### 4. Remote Branch löschen
```bash
git push origin --delete feature/layout-navigation
```
**Warum?** Entfernt den Branch aus dem GitHub Repository.

**Alternative Syntax:**
```bash
git push origin :feature/layout-navigation
```

### 5. Neuen Branch erstellen und wechseln
```bash
git checkout -b feature/header-menu
```
**Warum?** Erstellt einen neuen Branch basierend auf dem aktuellen Stand von develop.

**Entspricht:**
```bash
git branch feature/header-menu
git checkout feature/header-menu
```

### 6. Neuen Branch zu GitHub pushen
```bash
git push -u origin feature/header-menu
```
**Warum?**
- Pusht den Branch zu GitHub
- `-u` (upstream) setzt Tracking, sodass du später einfach `git push` nutzen kannst

---

## Schnellreferenz

### Branch löschen (lokal + remote)
```bash
git checkout develop
git pull --rebase origin develop
git branch -d feature/old-branch
git push origin --delete feature/old-branch
```

### Neuen Branch erstellen
```bash
git checkout -b feature/new-branch
git push -u origin feature/new-branch
```

### Branch-Liste anzeigen
```bash
# Lokale Branches
git branch

# Remote Branches
git branch -r

# Alle Branches
git branch -a
```

### Branch umbenennen
```bash
# Lokal umbenennen
git branch -m old-name new-name

# Remote löschen und neu pushen
git push origin --delete old-name
git push -u origin new-name
```

---

## Häufige Probleme

### Problem: "Cannot delete branch - not fully merged"
```bash
# Lösung: Force delete
git branch -D feature/branch-name
```

### Problem: "Divergent branches"
```bash
# Lösung 1: Rebase
git pull --rebase origin develop

# Lösung 2: Merge
git pull --no-rebase origin develop

# Lösung 3: Fast-forward only
git pull --ff-only origin develop
```

### Problem: Remote Branch existiert nicht mehr
```bash
# Lokale Remote-Referenzen aufräumen
git fetch --prune
```

---

## Best Practices

1. **Immer auf develop/main basieren** - Erstelle neue Feature-Branches von develop
2. **Regelmäßig pullen** - Halte deinen Branch aktuell
3. **Aussagekräftige Namen** - `feature/header-menu` statt `test-branch`
4. **Branch nach Merge löschen** - Vermeide Branch-Chaos
5. **Upstream tracking setzen** - Nutze `-u` beim ersten Push

---

## Commit Convention

Siehe [GIT-WORKFLOW.md](GIT-WORKFLOW.md) für vollständige Commit-Konventionen:
- `feat:` - Neues Feature
- `fix:` - Bugfix
- `refactor:` - Code-Umstrukturierung
- `docs:` - Dokumentation
- `style:` - CSS/Formatierung

---

**Weitere Details:** Siehe [GIT-WORKFLOW.md](GIT-WORKFLOW.md)
