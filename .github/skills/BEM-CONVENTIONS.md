# BEM Conventions für Join Project

## Übersicht
Dieses Dokument definiert die BEM (Block Element Modifier) Konventionen für das Join Project.

## ⚠️ WICHTIG: CSS Development Checklist

### VOR jeder CSS-Datei-Erstellung prüfen:

#### BEM-Struktur
- [ ] Kommentar-Header: `/* ========== Block Name ========== */`
- [ ] Sektion 1: "Block"
- [ ] Sektion 2: "Elements"
- [ ] Sektion 3: "Modifiers" (falls vorhanden)
- [ ] **KEINE** Sektion "Responsive Design" am Ende!

#### Media Queries Platzierung
- [ ] **NIEMALS** gesammelt am Ende der Datei
- [ ] **IMMER** inline direkt beim jeweiligen Block/Element mit `@media`
- [ ] Mobile-First: Base ohne Media Query, dann `@media (min-width: ...)`

#### Standard Breakpoints
- **640px** - Small tablets
- **768px** - Tablets (Mobile → Desktop Switch)
- **1025px** - Desktop

### Reminder für AI
**STOPP bevor du CSS erstellst:**
1. Prüfe: Sind Media Queries inline bei Blocks/Elements?
2. Prüfe: Mobile-First mit min-width?
3. Prüfe: BEM-Namenskonvention eingehalten?
4. **Bei Unsicherheit:** Schaue in header.css, menu.css, summary.css - diese zeigen die korrekte Struktur!

---

## BEM Namenskonvention

### Block
- Eigenständige Komponente, die wiederverwendbar ist
- Beispiel: `.header`, `.menu`, `.card`, `.button`

### Element
- Teil eines Blocks, macht außerhalb des Blocks keinen Sinn
- Verbunden mit doppeltem Unterstrich `__`
- Beispiel: `.header__logo`, `.menu__link`, `.card__title`

### Modifier
- Variante oder Zustand eines Blocks oder Elements
- Verbunden mit doppeltem Bindestrich `--`
- Beispiel: `.button--primary`, `.menu--open`, `.card--highlighted`

## Namensstruktur
```
.block { }
.block__element { }
.block--modifier { }
.block__element--modifier { }
```

## CSS Organisation

### 1. Inline Media Queries bei jedem Block/Element
Media Queries werden **inline direkt beim jeweiligen Block/Element** platziert, nicht am Ende der Datei.

**✅ Richtig (Inline @media):**
```css
.header {
  /* Mobile base styles */
  height: 60px;
  padding: var(--spacing-sm);

  @media (min-width: 640px) {
    height: 70px;
  }

  @media (min-width: 768px) {
    height: 80px;
    padding: var(--spacing-md);
  }
}

.header__logo {
  /* Mobile base styles */
  height: 40px;

  @media (min-width: 768px) {
    height: 50px;
  }
}

.header__user-menu {
  position: absolute;
  top: calc(100% + var(--spacing-sm));
  right: var(--spacing-md);
  background-color: var(--color-white);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  min-width: 180px;
  padding: var(--spacing-sm) 0;
  z-index: var(--z-dropdown);

  @media (min-width: 768px) {
    right: var(--spacing-xl);
    min-width: 200px;
  }
}
```

**❌ Falsch (Separate Media Queries am Ende):**
```css
.header {
  height: 60px;
}

.header__logo {
  height: 40px;
}

/* Media Queries am Ende - NICHT BEM-konform */
@media (min-width: 768px) {
  .header {
    height: 80px;
  }
  .header__logo {
    height: 50px;
  }
}
```

### 2. Breakpoints (Mobile-First)
```css
/* Mobile: Base Styles (0-639px) */
.block {
  property: value;
}

/* Small Tablets and above */
@media (min-width: 640px) {
  .block {
    property: value;
  }
}

/* Tablets and above */
@media (min-width: 768px) {
  .block {
    property: value;
  }
}

/* Desktop and above */
@media (min-width: 1025px) {
  .block {
    property: value;
  }
}
```
### 3. Dateistruktur
```
css/
├── base/
│   ├── variables.css    # Design tokens
│   ├── reset.css        # CSS Reset
│   └── fonts.css        # Font-faces
├── layout/
│   └── app-layout.css   # .app-layout + grid system
├── components/
│   ├── header.css       # .header + .header__*
│   ├── menu.css         # .menu + .menu__*
│   └── button.css       # .button + .button__*
└── pages/
    ├── login.css        # .login + .login__*
    └── summary.css      # .summary + .summary__*
```

### 4. Sektionen in CSS-Dateien
```css
/* ==========================================================================
   Block Name
   ========================================================================== */

.block {
  /* Mobile-first base styles */
  property: value;

  @media (min-width: 640px) {
    property: value;
  }

  @media (min-width: 768px) {
    property: value;
  }
}

/* ==========================================================================
   Block Elements
   ========================================================================== */

.block__element {
  /* Mobile-first base styles */
  property: value;

  @media (min-width: 768px) {
    property: value;
  }
}

/* ==========================================================================
   Block Modifiers
   ========================================================================== */

.block--modifier {
  /* Modifier styles */
  property: value;

  @media (min-width: 768px) {
    property: value;
  }
}

/* Block States (interaktive Zustände) */
.block__element:hover {
  /* Hover state */
}

.block__element:focus {
  /* Focus state */
}

.block__element--active {
  /* Active modifier */
}
```

## Vermeiden

### 1. ❌ Keine Verschachtelung über 3 Ebenen
```css
/* FALSCH */
.block__element__subelement { }

/* RICHTIG */
.block__subelement { }
```

### 2. ❌ Keine Block-Namen in Element-Namen
```css
/* FALSCH */
.menu__menu-link { }

/* RICHTIG */
.menu__link { }
```

### 3. ❌ Keine kombinierten Selektoren (außer States)
```css
/* FALSCH */
.header .header__logo { }

/* RICHTIG */
.header__logo { }

/* ERLAUBT für States */
.header__link:hover { }
.header__link:focus { }
```

### 4. ❌ Keine ID-Selektoren in CSS
```css
/* FALSCH */
#header { }

/* RICHTIG */
.header { }
```

### 5. ❌ Keine Tag-Selektoren (außer Reset)
```css
/* FALSCH */
header { }
button { }

/* RICHTIG */
.header { }
.button { }
```

## Modifier Anwendungsfälle

### Boolean Modifier
Zustand ist an/aus
```css
.menu--open { }
.button--disabled { }
.card--highlighted { }
```

### Key-Value Modifier
Verschiedene Varianten
```css
.button--primary { }
.button--secondary { }
.button--danger { }

.card--small { }
.card--medium { }
.card--large { }
```

## Beispiel: Complete Component

```css
/* ==========================================================================
   Card Component
   ========================================================================== */

.card {
  background: var(--color-white);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  box-shadow: var(--shadow-sm);

  @media (min-width: 640px) {
    padding: var(--spacing-lg);
  }

  @media (min-width: 768px) {
    padding: var(--spacing-xl);
  }
}

/* ==========================================================================
   Card Elements
   ========================================================================== */

.card__header {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  margin-bottom: var(--spacing-md);
}

.card__title {
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-bold);
  color: var(--color-primary);

  @media (min-width: 768px) {
    font-size: var(--font-size-xl);
  }
}

.card__content {
  color: var(--color-text);
  line-height: 1.6;
}

.card__footer {
  margin-top: var(--spacing-lg);
  display: flex;
  gap: var(--spacing-sm);
}

/* ==========================================================================
   Card Modifiers
   ========================================================================== */

.card--highlighted {
  border: 2px solid var(--color-accent);
  box-shadow: var(--shadow-lg);
}

.card--compact {
  padding: var(--spacing-sm);

  @media (min-width: 640px) {
    padding: var(--spacing-md);
  }
}

/* Card States */
.card:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-2px);
  transition: all var(--transition-base);
}
```

## HTML Beispiel
```html
<!-- Normaler Card -->
<div class="card">
  <div class="card__header">
    <h2 class="card__title">Title</h2>
  </div>
  <div class="card__content">
    Content here
  </div>
  <div class="card__footer">
    <button class="button button--primary">Action</button>
  </div>
</div>

<!-- Highlighted Card -->
<div class="card card--highlighted">
  <div class="card__header">
    <h2 class="card__title">Important</h2>
  </div>
</div>

<!-- Compact Card -->
<div class="card card--compact">
  <div class="card__content">Small content</div>
</div>
```

## Vorteile dieser Konvention

1. **Wartbarkeit**: Alle Styles einer Komponente an einem Ort
2. **Lesbarkeit**: Sofort erkennbar, wie eine Komponente auf verschiedenen Breakpoints aussieht
3. **Modularität**: Komponenten können einfach in andere Projekte übernommen werden
4. **Spezifität**: Niedrige CSS-Spezifität durch flache Selektoren
5. **Keine Konflikte**: Block-Namen sind einzigartig, keine Naming-Kollisionen
6. **Skalierbarkeit**: Einfach neue Elemente und Modifier hinzufügen

## Best Practices

1. **Ein Block pro Datei** (bei größeren Komponenten)
2. **Alphabetische Reihenfolge** der Properties (optional, aber konsistent)
3. **Mobile-First**: Base Styles für Mobile, Enhancement für größere Screens
4. **Kommentare**: Sektionen klar trennen mit Kommentaren
5. **Konsistente Naming**: Immer gleiche Pattern für ähnliche Elemente
6. **Design Tokens**: Variablen aus variables.css verwenden

## Referenzen

- [BEM Official Methodology](https://en.bem.info/methodology/)
- [CSS Guidelines](https://cssguidelin.es/)
- [SMACSS](http://smacss.com/) (ähnliche Methodik)
