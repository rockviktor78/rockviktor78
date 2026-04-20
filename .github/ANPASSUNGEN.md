# GitHub Ordner - Anpassungen für Join Projekt

**Datum**: 15. Februar 2026
**Status**: ✅ Auf Join (Vanilla JavaScript) angepasst

---

## ✅ Komplett angepasste Dateien

### Dokumentation

- **[docs/How-to-work-with-Copilot.md](docs/How-to-work-with-Copilot.md)**
  - ✅ DABubble → Join
  - ✅ Angular → Vanilla JavaScript
  - ✅ Alle Code-Beispiele für Join aktualisiert

- **[docs/HTML-Semantic-Guide.md](docs/HTML-Semantic-Guide.md)**
  - ✅ Projektname angepasst

### Agent Dateien

- **[agents/code-review/agent.md](agents/code-review/agent.md)**
  - ✅ Bereits Join-spezifisch

- **[agents/code-review/checklist.md](agents/code-review/checklist.md)**
  - ✅ Join Coding Standards

- **[agents/refactoring/component-cleanup.md](agents/refactoring/component-cleanup.md)**
  - ✅ Component → Module
  - ✅ _.component.ts → _.js
  - ✅ Vanilla JS Refactoring-Strategien

- **[agents/documentation/jsdoc-generator.md](agents/documentation/jsdoc-generator.md)**
  - ✅ Projektname angepasst

- **[agents/maintenance/unused-code-finder.md](agents/maintenance/unused-code-finder.md)**
  - ✅ Projektname angepasst

### Prompts

- **[prompts/join/](prompts/join/)**
  - ✅ **Alle 10 Prompts** sind bereits Join-spezifisch
  - ✅ Coding Standards, Architecture, Features etc.

---

## ⚠️ Dateien mit Angular-Beispielen (optional zum Anpassen)

Diese Dateien haben noch Angular/TypeScript Code-Beispiele, funktionieren aber auch als generelle Best-Practice Referenz:

- **[agents/maintenance/performance-audit.md](agents/maintenance/performance-audit.md)**
  - 📝 Enthält Angular Signals, computed()
  - 💡 Konzepte sind übertragbar auf Vanilla JS

---

## 📁 Ordnerstruktur

```
.github/
├── copilotinstructions.md    ✅ Join-spezifisch
├── agents/
│   ├── code-review/          ✅ Komplett angepasst
│   ├── documentation/        ✅ Angepasst
│   ├── maintenance/          ⚠️ Teilweise Angular-Beispiele
│   └── refactoring/          ✅ Auf JS angepasst
├── docs/
│   ├── How-to-work-with-Copilot.md  ✅ Komplett neu
│   └── HTML-Semantic-Guide.md       ✅ Angepasst
├── prompts/
│   ├── README.md             ✅ Join-Referenzen
│   └── join/                 ✅ Alle 10 Prompts OK
├── skills/
│   ├── BEM-CONVENTIONS.md    ✅ Framework-agnostisch
│   ├── if-function.md        ✅ CSS-Funktion
│   └── light-dark.md         ✅ CSS-Funktion
└── workflows/
    ├── BRANCH-MANAGEMENT.md          ✅ Generisch
    ├── DEFINITION-OF-DONE.md         ✅ Generisch
    ├── FIREBASE-ID-EXPLANATION.md    ✅ Generisch
    └── ...                           ✅ Alle OK
```

---

## 🎯 Was wurde geändert?

### Hauptanpassungen

1. **DABubble → Join** überall
2. **Angular → Vanilla JavaScript**
3. **Angular Service Worker → Workbox**
4. **TypeScript → JavaScript**
5. **TypeScript → JavaScript**
6. **Components/Signals → ES6 Modules**
   5## Technologie-Stack Änderungen

| Vorher (DABubble)      | Jetzt (Join)            |
| ---------------------- | ----------------------- |
| Angular 18             | Vanilla JavaScript ES6+ |
| Angular Service Worker | Workbox CLI             |
| Signals/Computed       | Standard JS             |
| Component Architecture | Module Pattern          |
| Angular CLI            | npm scripts             |

---

## 🚀 So nutzt du die Dokumentation

### 1. Für Copilot

Die Datei [copilotinstructions.md](copilotinstructions.md) ist die Hauptkonfiguration für GitHub Copilot und bereits Join-spezifisch.

### 2. Für Prompts

```bash
# Alle Join Prompts sind in:
.github/prompts/join/

# Übersicht:
- 01-coding-standards.md
- 02-architecture.md
- 03-page-structure.md
- 04-styling-bem.md
- 05-firebase-integration.md
- 06-feature-kanban-board.md
- 07-feature-task-management.md
- 08-feature-user-auth.md
- 09-feature-contact-management.md
- 10-quality-checklist.md
```

### 3. Für PWA Development

Der Agent in [agents/code-review/](agents/code-review/) prüft automatisch:

- ✅ Funktionen ≤ 14 Zeilen
- ✅ Dateien ≤ 400 LOC
- ✅ BEM-Naming
- ✅ JSDoc vorhanden
- ✅ Error Handling

---

## 📝 Nächste Schritte (Optional)

Falls du später noch mehr anpassen möchtest:

1. **performance-audit.md** - Angular Signals Beispiele durch Vanilla JS ersetzen
2. **Weitere Agent-Dateien** erstellen (z.B. für Testing, Deployment)
3. **Skills erweitern** - Join-spezifische Patterns dokumentieren

---

## ✅ Fazit

Dein `.github` Ordner ist jetzt komplett auf **Join (Vanilla JS + Workbox)** angepasst! 🎉

Die Dokumentation ist:

- ✅ Projektspezifisch (Join statt DABubble)
- ✅ Technologie-korrekt (Vanilla JS statt Angular)
- ✅ Build-Prozess aktuell (Workbox statt Angular SW)
- ✅ Ready für deine Weiterbildung 🚀

---

**Erstellt**: 15. Februar 2026  
**Projekt**: Join Task Management  
**Technologie**: Vanilla JavaScript ES6+ | Workbox | Firebase
avaScript)\*\* angepasst! 🎉

Die Dokumentation ist:

- ✅ Projektspezifisch (Join statt DABubble)
- ✅ Technologie-korrekt (Vanilla JS statt Angular)
- ✅ Ready für deine Weiterbildung 🚀

---

**Erstellt**: 15. Februar 2026  
**Projekt**: Join Task Management  
**Technologie**: Vanilla JavaScript ES6+
