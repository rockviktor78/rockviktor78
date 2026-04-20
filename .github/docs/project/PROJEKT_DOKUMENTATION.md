# Join - Task Management System

## 📋 Projektübersicht

**Join** ist eine webbasierte Task-Management-Anwendung, die Teams dabei hilft, Aufgaben zu organisieren, Kontakte zu verwalten und den Projektfortschritt zu überwachen. Die Anwendung bietet eine intuitive Benutzeroberfläche mit verschiedenen Ansichten für effektives Projektmanagement.

## 🛠️ Technologie-Stack

### Frontend
- **HTML5** - Strukturierung der Webseiten
- **CSS3** - Styling mit CSS-Variablen und Mobile-First-Ansatz
- **Vanilla JavaScript (ES6+)** - Client-seitige Logik
- **Inter Font Family** - Typografie (Gewichte 100-900)

### Backend & Datenbank
- **Firebase Realtime Database** - Cloud-basierte NoSQL-Datenbank
  - Endpoint: `https://join-7c944-default-rtdb.europe-west1.firebasedatabase.app/`

### Architektur
- Multi-Page Application (MPA) mit Template-System
- Modulare JavaScript-Architektur
- Component-basiertes CSS-Design

## 📁 Projektstruktur

```
join/
├── index.html                    # Login/Signup-Seite (Einstiegspunkt)
├── sconfig.json                  # JavaScript-Compiler-Konfiguration
│
├── pages/                        # Alle Unterseiten
│   ├── board.html               # Kanban-Board für Tasks
│   ├── summary.html             # Dashboard/Übersicht
│   ├── add-task.html            # Neue Aufgabe erstellen
│   ├── contacts.html            # Kontaktverwaltung
│   ├── help.html                # Hilfe-Seite
│   ├── legal-notice.html        # Impressum
│   └── privacy-policy.html      # Datenschutzerklärung
│
├── scripts/                      # JavaScript-Dateien
│   ├── firebase.js              # Firebase-API-Wrapper (GET, POST)
│   ├── board.js                 # Board-Logik
│   ├── contacts.js              # Kontakt-Management
│   ├── contacts-templates.js    # Kontakt-Templates
│   ├── summary.js               # Summary-Dashboard
│   ├── add_task.js              # Task-Erstellung
│   │
│   ├── auth/                    # Authentifizierungs-Module
│   │   ├── auth_login.js       # Login-Logik
│   │   ├── auth_signup.js      # Registrierungs-Logik
│   │   └── auth_utilities.js   # Auth-Hilfsfunktionen
│   │
│   └── shared/                  # Gemeinsam genutzte Module
│       ├── include-html.js     # HTML-Template-Loader
│       ├── init-template.js    # Template-Initialisierung
│       ├── menu.js             # Navigationsmenü
│       └── view-transitions.js # Seitenübergänge
│
├── styles/                       # CSS-Dateien
│   ├── auth.css                 # Login/Signup-Styling
│   ├── board.css                # Board-spezifisches Styling
│   ├── contacts.css             # Kontakte-Ansicht
│   ├── contacts-form.css        # Kontakt-Formular
│   ├── summary.css              # Dashboard-Styling
│   ├── help.css                 # Hilfe-Seite
│   ├── content-pages.css        # Statische Seiten (Legal, Privacy)
│   │
│   ├── base/                    # Basis-Styles
│   │   ├── reset.css           # CSS-Reset
│   │   ├── fonts.css           # Font-Definitionen
│   │   ├── variables.css       # CSS-Variablen (Design Tokens)
│   │   └── transitions.css     # Animationen
│   │
│   └── components/              # Wiederverwendbare Komponenten
│       ├── header.css          # Header-Komponente
│       ├── header-mobile-first.css
│       ├── menu.css            # Navigationsmenü
│       ├── menu-mobile-first.css
│       └── template.css        # Template-Layout
│
└── assets/                       # Statische Assets
    ├── fonts/inter/             # Inter-Schriftarten (WOFF2)
    ├── img/                     # Bilder & Icons
    │   ├── auth/               # Login/Signup-Icons
    │   ├── board/              # Board-Icons
    │   ├── contacts/           # Kontakt-Icons
    │   ├── menu/               # Menü-Icons
    │   ├── header/             # Header-Icons
    │   ├── shared/             # Gemeinsame Icons
    │   ├── tasks/              # Task-Icons
    │   └── favicon/            # Favicon-Varianten
    │
    └── templates/               # HTML-Templates
        ├── layout.html         # Haupt-Layout (Header + Menu)
        └── task_form.html      # Task-Formular
```

## 🎯 Hauptfunktionen

### 1. **Authentifizierung**
- Benutzerregistrierung (Sign Up)
- Benutzer-Login
- Passwort-Sichtbarkeit umschalten
- "Remember Me" Funktion
- Firebase-basierte Benutzerverwaltung

### 2. **Dashboard (Summary)**
- Übersicht über alle Tasks
- Statistiken und KPIs
- Schnellzugriff auf wichtige Bereiche

### 3. **Board (Kanban)**
- Drag & Drop Task-Management
- Verschiedene Spalten (To-Do, In Progress, Done, etc.)
- Task-Filterung und Suche

### 4. **Task-Verwaltung**
- Neue Tasks erstellen
- Tasks bearbeiten
- Tasks löschen
- Task-Details anzeigen
- Prioritäten setzen
- Fälligkeitsdaten

### 5. **Kontaktverwaltung**
- Kontakte hinzufügen, bearbeiten, löschen
- Alphabetische Sortierung
- Kontaktdetails anzeigen
- Farbcodierte Avatare
- Telefon- und E-Mail-Integration

### 6. **Navigation**
- Responsives Seitenmenü
- Desktop- und Mobile-Ansichten
- Header mit Benutzerproil
- Breadcrumb-Navigation

## 🚀 Setup & Installation

### Voraussetzungen
- Moderner Webbrowser (Chrome, Firefox, Safari, Edge)
- Lokaler Webserver (z.B. VS Code Live Server, Python SimpleHTTPServer)

### Installation

1. **Repository klonen oder Projekt herunterladen**
   ```bash
   git clone <repository-url>
   cd join
   ```

2. **Lokalen Webserver starten**
   
   **Option A: VS Code Live Server**
   - Installiere die "Live Server" Extension
   - Rechtsklick auf `index.html` → "Open with Live Server"

   **Option B: Python**
   ```bash
   python -m http.server 8000
   # Öffne http://localhost:8000
   ```

   **Option C: Node.js**
   ```bash
   npx http-server
   ```

3. **Anwendung öffnen**
   - Navigiere zu `http://localhost:<port>/index.html`
   - Erstelle einen Account oder logge dich ein

## 💡 Entwicklungshinweise

### CSS-Architektur

**Design Tokens** (variables.css):
- Farben: Primary (`--color-primary`), Accent (`--color-accent`), Feedback-Farben
- Spacing: `--spacing-xs` bis `--spacing-xxl`
- Typography: `--font-family`, Font-Größen, Line-Heights
- Shadows, Borders, Breakpoints

**Mobile-First-Ansatz**:
- Basis-Styles für mobile Geräte
- Media Queries für größere Bildschirme
- Responsive Komponenten in separaten Dateien (`*-mobile-first.css`)

### JavaScript-Architektur

**Modularer Aufbau**:
- Jede Seite hat eigene JS-Datei
- Gemeinsame Funktionen in `shared/`
- Auth-Logik isoliert in `auth/`

**Firebase-Integration**:
```javascript
// GET-Request
const data = await getData("contacts");

// POST-Request
await postData("contacts", { name: "Max", email: "max@example.com" });
```

### Template-System

**HTML-Includes**:
```html
<div w3-include-html="../assets/templates/layout.html"></div>
```

Die `include-html.js` lädt Templates dynamisch nach.

### Wichtige Konventionen

1. **Dateibenennung**:
   - CSS: Kleinbuchstaben mit Bindestrichen (`contacts-form.css`)
   - JS: Snake_case für Funktionen, camelCase für Variablen
   
2. **CSS-Klassen**:
   - BEM-ähnliche Notation (`.component__element--modifier`)
   - Sprechende Klassennamen

3. **Bildformate**:
   - Icons: SVG (vektorskalierbar)
   - Logos: SVG
   - Fotos: PNG/JPG (optimiert)

## 🔧 Nützliche Befehle & Workflows

### Entwicklung
```bash
# Live Server starten (VS Code)
# Rechtsklick auf index.html → Open with Live Server

# Browser Developer Tools
F12 oder Cmd/Ctrl+Shift+I
```

### Debugging
- Console Logs prüfen (Firebase-Fehler werden geloggt)
- Network-Tab für Firebase-Requests
- Elements-Tab für CSS-Debugging

### Neue Seite hinzufügen

1. HTML-Datei in `pages/` erstellen
2. Entsprechende CSS-Datei in `styles/` erstellen
3. JavaScript-Datei in `scripts/` erstellen
4. Template einbinden:
   ```html
   <div w3-include-html="../assets/templates/layout.html"></div>
   ```
5. Menü-Link in `assets/templates/layout.html` hinzufügen

## 📊 Datenbank-Struktur (Firebase)

```json
{
  "users": {
    "userId1": {
      "name": "Max Mustermann",
      "email": "max@example.com",
      "password": "hashed_password"
    }
  },
  "contacts": {
    "contactId1": {
      "name": "Anna Schmidt",
      "email": "anna@example.com",
      "phone": "+49 123 456789"
    }
  },
  "tasks": {
    "taskId1": {
      "title": "Projekt starten",
      "description": "...",
      "priority": "high",
      "dueDate": "2026-02-15",
      "status": "todo",
      "assignedTo": ["contactId1"]
    }
  }
}
```

## 🎨 Design-System

### Farbschema
- **Primary**: `#2a3647` (Dunkelblau)
- **Accent**: `#29abe2` (Hellblau)
- **Success**: `#7ae229` (Grün)
- **Error**: `#e60025` (Rot)
- **Warning**: `#ffa800` (Orange)

### Typografie
- **Font**: Inter (Google Fonts)
- **Gewichte**: 100-900 verfügbar
- **Basis-Größe**: 16px

### Breakpoints
- Mobile: < 768px
- Tablet: 768px - 1024px
- Desktop: > 1024px

## 🤝 Zusammenarbeit

### Code-Qualität
- Konsistente Formatierung
- Aussagekräftige Kommentare
- Keine Konsolenausgaben in Production

### Git-Workflow (falls verwendet)
```bash
# Feature-Branch erstellen
git checkout -b feature/neue-funktion

# Änderungen committen
git add .
git commit -m "feat: Neue Funktion hinzufügen"

# Branch pushen
git push origin feature/neue-funktion
```

### Commit-Konventionen
- `feat:` Neue Funktion
- `fix:` Bugfix
- `style:` CSS/Design-Änderungen
- `refactor:` Code-Umstrukturierung
- `docs:` Dokumentation

## 📝 Offene Aufgaben / To-Do

- [ ] Board.js-Implementierung vervollständigen
- [ ] Drag & Drop für Tasks implementieren
- [ ] Task-Formular-Validierung verbessern
- [ ] Responsive Design testen
- [ ] Performance-Optimierung (Lazy Loading)
- [ ] Offline-Funktionalität (Service Worker)
- [ ] Unit-Tests schreiben
- [ ] E2E-Tests implementieren

## 🐛 Bekannte Issues

- README.md ist derzeit leer
- `board.js` ist leer (noch zu implementieren)
- Einige Platzhalter-Texte auf Board/Summary-Seiten

## 📞 Support & Kontakt

Bei Fragen zum Projekt:
1. Code-Kommentare prüfen
2. Bestehende Implementierungen als Referenz nutzen
3. Firebase-Dokumentation konsultieren

## 📄 Lizenz & Rechtliches

- Legal Notice: [pages/legal-notice.html](pages/legal-notice.html)
- Privacy Policy: [pages/privacy-policy.html](pages/privacy-policy.html)

---

**Letzte Aktualisierung**: Februar 2026
**Version**: 1.0.0
**Entwicklungsstatus**: In aktiver Entwicklung
