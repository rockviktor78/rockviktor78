# Theme Implementation Approaches

> Dokumentation der verschiedenen Optionen zur Umsetzung von Light/Dark Mode in einer App

---

## Option 1: CSS `light-dark()` Funktion (Modern, Empfohlen)

**Ansatz:** Browser macht alles automatisch, **kein JavaScript nötig!**

### CSS Setup
```css
:root {
  color-scheme: light dark;
}

body {
  background-color: light-dark(#efedea, #223a2c);
  color: light-dark(#333b3c, #efefec);
}

a {
  color: light-dark(#0969da, #58a6ff);
}

button {
  background: light-dark(#f0f0f0, #2a2a2a);
  color: light-dark(#000, #fff);
  border: 1px solid light-dark(#ccc, #555);
}
```

### JavaScript
```javascript
// Keine Funktionen nötig!
// Der Browser erkennt automatisch die OS-Einstellung
```

### ✅ Vorteile
- Minimal JavaScript
- Automatische OS-Erkennung
- Zukunftssicher
- Weniger Code
- Performance-optimal

### ❌ Nachteile
- Browser-Support: Nicht in älteren Browsern (IE 11)
- Keine manuelle Kontrolle über Theme-Wechsel
- Kann nicht lokal speichern (folgt immer OS)

### Browser-Support
- Chrome 123+
- Edge 123+
- Firefox 120+
- Safari 17.5+
- ❌ IE 11

---

## Option 2: CSS-Variablen + Theme-Klasse

**Ansatz:** JavaScript setzt `data-theme` Attribut, CSS reagiert darauf

### JavaScript (theme-service.js)
```javascript
/**
 * Sets the theme with CSS variable support.
 *
 * @param {string} theme - Theme to apply ("device", "light", or "dark")
 */
function applyTheme(theme) {
  const realTheme = theme === "device" ? getSystemTheme() : theme;
  document.documentElement.setAttribute("data-theme", realTheme);
  localStorage.setItem("joinTheme", theme);
}

/**
 * Gets system theme preference.
 *
 * @returns {string} System theme ("dark" or "light")
 */
function getSystemTheme() {
  return window.matchMedia("(prefers-color-scheme: dark)").matches
    ? "dark"
    : "light";
}

/**
 * Initializes theme from localStorage.
 */
function initTheme() {
  const theme = localStorage.getItem("joinTheme") || "device";
  applyTheme(theme);
}
```

### CSS mit Variablen
```css
:root {
  /* Default / Light theme */
  --bg-color: #efedea;
  --text-color: #333b3c;
  --link-color: #0969da;
  --border-color: #d0d7de;
}

/* Dark theme */
:root[data-theme="dark"] {
  --bg-color: #223a2c;
  --text-color: #efefec;
  --link-color: #58a6ff;
  --border-color: #30363d;
}

/* Light theme (explicit) */
:root[data-theme="light"] {
  --bg-color: #efedea;
  --text-color: #333b3c;
  --link-color: #0969da;
  --border-color: #d0d7de;
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
}

a {
  color: var(--link-color);
}

button {
  background: var(--bg-color);
  color: var(--text-color);
  border: 1px solid var(--border-color);
}
```

### ✅ Vorteile
- Volle Kontrolle über Theme-Wechsel
- Theme speichern in localStorage
- Browser-kompatibel (auch IE 11 mit Fallback)
- Einfach zu debuggen
- "Device" Mode möglich

### ❌ Nachteile
- Mehr JavaScript
- Flash of unstyled content (FOUC) möglich
- Variablen müssen dupliziert werden (light/dark)

---

## Option 3: Separate CSS-Dateien

**Ansatz:** Dynamischer CSS-Link wird gewechselt

### JavaScript
```javascript
/**
 * Applies theme by switching CSS file.
 *
 * @param {string} theme - Theme to apply ("device", "light", or "dark")
 */
function applyTheme(theme) {
  const link = document.getElementById("theme-stylesheet");
  link.href = `/css/theme-${theme}.css`;
  localStorage.setItem("joinTheme", theme);
}

/**
 * Initializes theme from localStorage.
 */
function initTheme() {
  const theme = localStorage.getItem("joinTheme") || "light";
  applyTheme(theme);
}
```

### HTML
```html
<head>
  <link id="theme-stylesheet" rel="stylesheet" href="/css/theme-light.css">
</head>
```

### CSS Struktur
```
css/
├── theme-light.css
├── theme-dark.css
└── theme-device.css
```

### theme-light.css
```css
:root {
  --bg-color: #efedea;
  --text-color: #333b3c;
  --link-color: #0969da;
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
}
```

### theme-dark.css
```css
:root {
  --bg-color: #223a2c;
  --text-color: #efefec;
  --link-color: #58a6ff;
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
}
```

### ✅ Vorteile
- Komplett getrennte Stylesheets
- Performance: Nur relevante CSS wird geladen
- Sehr wartbar für große Apps
- Keine Duplizierung

### ❌ Nachteile
- FOUC (Flash of Unstyled Content) wahrscheinlich
- Mehr Dateien zu maintainen
- HTTP Requests für CSS-Dateien
- Komplexer zu implementieren

---

## **🎯 Hybrid-Ansatz: Option 1 + 2 kombiniert (EMPFOHLEN)**

**Ansatz:** Best of both worlds!
- `light-dark()` für automatische Browser-Erkennung
- CSS-Variablen für manuelle Kontrolle
- Data-Attribute für explizite Theme-Override

### JavaScript (theme-service.js)
```javascript
/**
 * Sets theme with hybrid support (light-dark + variables).
 *
 * @param {string} theme - Theme to apply ("device", "light", or "dark")
 */
function applyTheme(theme) {
  const realTheme = theme === "device" ? getSystemTheme() : theme;
  document.documentElement.setAttribute("data-theme", realTheme);
  localStorage.setItem("joinTheme", theme);
}

/**
 * Gets system theme preference.
 *
 * @returns {string} System theme ("dark" or "light")
 */
function getSystemTheme() {
  return window.matchMedia("(prefers-color-scheme: dark)").matches
    ? "dark"
    : "light";
}

/**
 * Initializes theme from localStorage on page load.
 */
function initTheme() {
  const theme = localStorage.getItem("joinTheme") || "device";
  applyTheme(theme);
}

/**
 * Sets up theme toggle button and initializes theme.
 */
function setupThemeToggle() {
  const themeBtn = document.getElementById("headerThemeBtn");
  if (themeBtn) {
    themeBtn.addEventListener("click", handleThemeToggle);
  }
  document.addEventListener("DOMContentLoaded", initTheme);
}

/**
 * Handles theme toggle click event.
 */
function handleThemeToggle() {
  const current = localStorage.getItem("joinTheme") || "device";
  const next = getNextTheme(current);
  applyTheme(next);
}

/**
 * Gets the next theme in rotation cycle.
 *
 * @param {string} current - Current theme
 * @returns {string} Next theme
 */
function getNextTheme(current) {
  const THEMES = ["device", "light", "dark"];
  const idx = THEMES.indexOf(current);
  return THEMES[(idx + 1) % THEMES.length];
}

export {
  applyTheme,
  initTheme,
  setupThemeToggle,
  handleThemeToggle,
  getSystemTheme,
  getNextTheme,
};
```

### CSS (Hybrid Setup)
```css
:root {
  /* CRITICAL: Enable light-dark() support */
  color-scheme: light dark;

  /* Define all colors with light-dark() */
  --bg-primary: light-dark(#ffffff, #0d1117);
  --bg-secondary: light-dark(#f6f8fa, #161b22);
  --text-primary: light-dark(#24292f, #c9d1d9);
  --text-secondary: light-dark(#57606a, #8b949e);
  --border-color: light-dark(#d0d7de, #30363d);
  --link-color: light-dark(#0969da, #58a6ff);
  --link-hover: light-dark(#0550ae, #79c0ff);
  --accent: light-dark(#0969da, #1f6feb);
  --success: light-dark(#1a7f37, #3fb950);
  --danger: light-dark(#cf222e, #f85149);
  --warning: light-dark(#9a6700, #d29922);
}

/* Override für explizites "device" Mode */
:root[data-theme="light"] {
  color-scheme: light;
}

:root[data-theme="dark"] {
  color-scheme: dark;
}

/* Nutze die Variablen überall */
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
}

.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
}

a {
  color: var(--link-color);
}

a:hover {
  color: var(--link-hover);
}

button {
  background: var(--accent);
  color: white;
}

.alert-success {
  background: light-dark(#dafbe1, #1b4721);
  color: var(--success);
}

.alert-danger {
  background: light-dark(#ffebe9, #3d1f1a);
  color: var(--danger);
}
```

### HTML
```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="color-scheme" content="light dark">
  <link rel="stylesheet" href="/css/base/variables.css">
  <link rel="stylesheet" href="/css/base/reset.css">
  <link rel="stylesheet" href="/css/base/fonts.css">
  <!-- Weitere CSS files -->
</head>
<body>
  <header>
    <button id="headerThemeBtn" class="header__theme-btn">
      <img id="headerThemeIcon" src="/assets/img/theme/device.svg" alt="Theme toggle">
    </button>
  </header>

  <script type="module">
    import { setupThemeToggle } from './js/shared/theme-service.js';
    setupThemeToggle();
  </script>
</body>
</html>
```

### ✅ Vorteile
- **Best of both worlds:**
  - `light-dark()` für automatische OS-Erkennung (Modern)
  - Daten-Attribute für manuelle Kontrolle (Flexibel)
  - CSS-Variablen für wartbaren Code
- **Zukunftssicher** - nutzt moderne CSS Features
- **Fallback-ready** - funktioniert auch ohne JS
- **Keine Duplizierung** - single source of truth
- **localStorage Support** - speichert Nutzer-Wahl
- **"Device" Mode** möglich - folgt OS automatisch
- **Einfach zu erweitern** - neue Variablen hinzufügen

### ❌ Nachteile
- Etwas mehr Setup
- Browser-Support: Ältere Browser benötigen Fallback (IE 11)

### Browser-Support
- ✅ Chrome/Edge 123+
- ✅ Firefox 120+
- ✅ Safari 17.5+
- ⚠️ IE 11: Funktioniert mit CSS-Variablen Fallback

---

## Vergleich: Alle Optionen

| Feature                       | Option 1 | Option 2 | Option 3 | Hybrid    |
|-------------------------------|----------|----------|----------|-----------|
| **Automatische OS-Erkennung** | ✅       | ✅       | ⚠️       | ✅        |
| **Manuelle Theme-Kontrolle**  | ❌       | ✅       | ✅       | ✅        |
| **localStorage Support**      | ❌       | ✅       | ✅       | ✅        |
| **CSS-Code Duplizierung**     | ❌       | ✅       | ❌       | ❌        |
| **Wartbarkeit**               | ✅       | ✅       | ⚠️       | ✅        |
| **Performance**               | ✅       | ✅       | ⚠️       | ✅        |
| **JavaScript nötig**          | ❌       | ✅       | ✅       | ✅        |
| **Browser-Support**           | ⚠️       | ✅       | ✅       | ⚠️        |
| **Empfohlen für**             | Einfach  | Komplett | Groß     | **Alle!** |

---

## Implementierungs-Checkliste für Hybrid-Ansatz

- [ ] `color-scheme: light dark` in `:root` setzen
- [ ] Alle Farben mit `light-dark()` definieren
- [ ] CSS-Variablen für Komponenten erstellen
- [ ] `applyTheme()` in theme-service.js implementieren
- [ ] `setupThemeToggle()` in header__init.js verwenden
- [ ] localStorage Check bei initTheme()
- [ ] Theme-Button Icon aktualisieren
- [ ] In beiden Modi testen (light + dark)
- [ ] Browser DevTools emulation testen
- [ ] OS-Einstellung testen

---

## Migration Pfad: Zu Hybrid-Ansatz

### Schritt 1: Basis Setup
```css
:root {
  color-scheme: light dark;
}
```

### Schritt 2: Farben mit light-dark() definieren
```css
:root {
  --bg: light-dark(white, #1a1a1a);
  --text: light-dark(black, white);
}
```

### Schritt 3: CSS-Variablen nutzen
```css
body {
  background: var(--bg);
  color: var(--text);
}
```

### Schritt 4: JavaScript Theme-Toggle
```javascript
function applyTheme(theme) {
  document.documentElement.setAttribute("data-theme", theme);
  localStorage.setItem("joinTheme", theme);
}
```

### Schritt 5: Für alle Komponenten anwenden
Wiederhole für alle CSS-Dateien (header, menu, cards, etc.)

---

## Nächste Schritte für dein Projekt

1. **theme-service.js** - Hybrid-Funktionen implementieren ✅
2. **variables.css** - light-dark() Variablen definieren
3. **Alle CSS-Dateien** - Variablen statt Hard-coded Colors
4. **Testen** - Light + Dark Mode
5. **Deployment** - Feature flaggen falls nötig

---

**Status:** Hybrid-Ansatz empfohlen für Join-MPA Projekt
**Datum:** Januar 2026
