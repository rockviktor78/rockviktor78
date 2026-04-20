# CSS light-dark() Function

> **✅ Baseline 2024 - Newly Available**
> Modern browsers now support this feature!

## Overview

The `light-dark()` CSS function enables setting **two colors** for a property - automatically switching between light and dark values based on the user's color scheme preference, **without needing `@media (prefers-color-scheme)` queries**.

---

## How It Works

```css
:root {
  /* REQUIRED: Enable light-dark() support */
  color-scheme: light dark;
}

body {
  /* First value = light mode, Second value = dark mode */
  color: light-dark(#333b3c, #efefec);
  background-color: light-dark(#efedea, #223a2c);
}
```

The function:
1. Returns **first value** if user prefers light mode (or no preference)
2. Returns **second value** if user prefers dark mode
3. Respects OS settings (Windows Dark Mode, macOS Dark Mode, etc.)

---

## Syntax

```css
/* Named color values */
color: light-dark(black, white);

/* RGB color values */
color: light-dark(rgb(0 0 0), rgb(255 255 255));

/* Hex colors */
color: light-dark(#000000, #ffffff);

/* HSL colors */
color: light-dark(hsl(0 0% 0%), hsl(0 0% 100%));

/* Custom properties */
color: light-dark(var(--light), var(--dark));
```

### Functional Notation
```
light-dark(light-color, dark-color)
```

**Parameters:**
- `light-color` - `<color>` value for light mode
- `dark-color` - `<color>` value for dark mode

---

## Requirements

⚠️ **CRITICAL:** Must set `color-scheme` property to enable `light-dark()`

```css
:root {
  color-scheme: light dark;
}
```

Without this, `light-dark()` won't work!

---

## Basic Example

```css
:root {
  /* Enable light-dark() */
  color-scheme: light dark;
}

body {
  /* Light: white bg, dark text */
  /* Dark: dark bg, light text */
  background-color: light-dark(white, #1a1a1a);
  color: light-dark(#333, #eee);
}

a {
  color: light-dark(blue, lightblue);
}

button {
  background: light-dark(#f0f0f0, #2a2a2a);
  color: light-dark(#000, #fff);
  border: 1px solid light-dark(#ccc, #555);
}
```

---

## With Custom Properties

**Best practice:** Define theme colors as custom properties

```css
:root {
  color-scheme: light dark;

  /* Light theme colors */
  --light-bg: ghostwhite;
  --light-color: darkslategray;
  --light-code: tomato;

  /* Dark theme colors */
  --dark-bg: darkslategray;
  --dark-color: ghostwhite;
  --dark-code: gold;
}

* {
  background-color: light-dark(var(--light-bg), var(--dark-bg));
  color: light-dark(var(--light-color), var(--dark-color));
}

code {
  color: light-dark(var(--light-code), var(--dark-code));
}
```

---

## Forcing Light or Dark Mode

Use `color-scheme` property to override user preference for specific sections:

```css
/* Force light mode */
.light-section {
  color-scheme: light;
  background: light-dark(white, black); /* Always returns white */
}

/* Force dark mode */
.dark-section {
  color-scheme: dark;
  background: light-dark(white, black); /* Always returns black */
}

/* Auto mode (respects user preference) */
.auto-section {
  color-scheme: light dark;
  background: light-dark(white, black); /* Switches based on user */
}
```

⚠️ **Warning:** Generally don't override user preferences. Only use for specific UI components or demos.

---

## Complete Theming Example

```css
:root {
  color-scheme: light dark;

  /* Define all theme colors */
  --bg-primary: light-dark(#ffffff, #0d1117);
  --bg-secondary: light-dark(#f6f8fa, #161b22);
  --text-primary: light-dark(#24292f, #c9d1d9);
  --text-secondary: light-dark(#57606a, #8b949e);
  --border: light-dark(#d0d7de, #30363d);
  --link: light-dark(#0969da, #58a6ff);
  --link-hover: light-dark(#0550ae, #79c0ff);
  --accent: light-dark(#0969da, #1f6feb);
  --success: light-dark(#1a7f37, #3fb950);
  --danger: light-dark(#cf222e, #f85149);
  --warning: light-dark(#9a6700, #d29922);
}

body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
}

.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
}

a {
  color: var(--link);
}

a:hover {
  color: var(--link-hover);
}

.btn-primary {
  background: var(--accent);
  color: light-dark(white, white);
}

.alert-success {
  background: light-dark(#dafbe1, #1b4721);
  color: var(--success);
}
```

---

## vs Traditional Approach

### Before (with @media):
```css
/* Light mode (default) */
body {
  background: white;
  color: #333;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  body {
    background: #1a1a1a;
    color: #eee;
  }
}
```

### After (with light-dark()):
```css
:root {
  color-scheme: light dark;
}

body {
  background: light-dark(white, #1a1a1a);
  color: light-dark(#333, #eee);
}
```

**Benefits:**
- ✅ Less code duplication
- ✅ Single source of truth
- ✅ Easier to maintain
- ✅ Better readability
- ✅ No media query nesting

---

## Combining with CSS Variables

**Pattern 1: Direct usage**
```css
:root {
  color-scheme: light dark;
}

.element {
  color: light-dark(#000, #fff);
  background: light-dark(#fff, #000);
}
```

**Pattern 2: Via custom properties (recommended)**
```css
:root {
  color-scheme: light dark;
  --text: light-dark(#000, #fff);
  --bg: light-dark(#fff, #000);
}

.element {
  color: var(--text);
  background: var(--bg);
}
```

**Pattern 3: Semantic naming**
```css
:root {
  color-scheme: light dark;

  --color-text-primary: light-dark(#1a1a1a, #f0f0f0);
  --color-text-secondary: light-dark(#666, #aaa);
  --color-bg-primary: light-dark(#fff, #0d0d0d);
  --color-bg-secondary: light-dark(#f5f5f5, #1a1a1a);
  --color-accent: light-dark(#0066cc, #3399ff);
}
```

---

## Practical Examples

### Navigation
```css
:root {
  color-scheme: light dark;
}

nav {
  background: light-dark(#ffffff, #1a1a1a);
  border-bottom: 1px solid light-dark(#e5e5e5, #333);
}

nav a {
  color: light-dark(#333, #eee);
}

nav a:hover {
  background: light-dark(#f5f5f5, #2a2a2a);
}
```

### Cards
```css
.card {
  background: light-dark(white, #1e1e1e);
  border: 1px solid light-dark(#ddd, #444);
  box-shadow: 0 2px 4px light-dark(rgba(0,0,0,0.1), rgba(0,0,0,0.5));
}

.card__title {
  color: light-dark(#000, #fff);
}

.card__text {
  color: light-dark(#555, #aaa);
}
```

### Forms
```css
input, textarea {
  background: light-dark(white, #2a2a2a);
  color: light-dark(#000, #fff);
  border: 2px solid light-dark(#ccc, #444);
}

input:focus {
  border-color: light-dark(#0066cc, #3399ff);
  outline: 2px solid light-dark(rgba(0,102,204,0.1), rgba(51,153,255,0.1));
}

::placeholder {
  color: light-dark(#999, #666);
}
```

### Buttons
```css
.btn {
  background: light-dark(#f0f0f0, #2a2a2a);
  color: light-dark(#000, #fff);
  border: 1px solid light-dark(#ccc, #555);
}

.btn:hover {
  background: light-dark(#e0e0e0, #3a3a3a);
}

.btn--primary {
  background: light-dark(#0066cc, #3399ff);
  color: white;
}
```

---

## User Preference Detection

The `light-dark()` function automatically detects:

**Operating System:**
- Windows: Settings > Personalization > Colors > "Choose your mode"
- macOS: System Preferences > General > Appearance
- Linux: GNOME Settings > Appearance
- iOS: Settings > Display & Brightness
- Android: Settings > Display > Dark theme

**Browser:**
- Chrome: DevTools > Rendering > "Emulate CSS media feature prefers-color-scheme"
- Firefox: DevTools > Inspector > "Simulate" button
- Safari: Develop > Experimental Features

---

## Testing in DevTools

### Chrome/Edge:
1. Open DevTools (F12)
2. Press `Ctrl+Shift+P` (Cmd+Shift+P on Mac)
3. Type "Rendering"
4. Select "Emulate CSS media feature prefers-color-scheme"
5. Choose: light, dark, or no-preference

### Firefox:
1. Open DevTools (F12)
2. Inspector tab
3. Click "Simulate" button in toolbar
4. Toggle between light/dark

### Safari:
1. Open Web Inspector
2. Elements tab
3. Use OS settings or enable experimental features

---

## Browser Compatibility

✅ **Baseline 2024:** Widely supported in modern browsers

**Supported:**
- Chrome 123+ (March 2024)
- Edge 123+
- Firefox 120+ (November 2023)
- Safari 17.5+ (May 2024)

**Not supported:**
- IE 11 (use fallbacks)
- Older mobile browsers

Check current support: [Can I Use - light-dark()](https://caniuse.com/mdn-css_types_color_light-dark)

---

## Fallbacks for Older Browsers

**Option 1: Provide fallback color**
```css
:root {
  color-scheme: light dark;
}

body {
  /* Fallback for browsers without support */
  background: white;
  color: #333;

  /* Override with light-dark() if supported */
  background: light-dark(white, #1a1a1a);
  color: light-dark(#333, #eee);
}
```

**Option 2: Use @supports**
```css
/* Fallback */
body {
  background: white;
  color: #333;
}

@media (prefers-color-scheme: dark) {
  body {
    background: #1a1a1a;
    color: #eee;
  }
}

/* Override if light-dark() is supported */
@supports (color: light-dark(white, black)) {
  body {
    background: light-dark(white, #1a1a1a);
    color: light-dark(#333, #eee);
  }
}
```

---

## Common Pitfalls

❌ **Forgetting color-scheme**
```css
/* WON'T WORK - missing color-scheme */
body {
  color: light-dark(black, white);
}
```

✅ **Correct**
```css
:root {
  color-scheme: light dark; /* REQUIRED */
}

body {
  color: light-dark(black, white);
}
```

---

❌ **Using only "light" or "dark"**
```css
:root {
  color-scheme: light; /* Only light mode */
}

body {
  /* Will ALWAYS return black (first value) */
  color: light-dark(black, white);
}
```

✅ **Correct**
```css
:root {
  color-scheme: light dark; /* Both modes */
}
```

---

❌ **Overriding user preferences unnecessarily**
```css
/* Don't force dark mode on everything */
* {
  color-scheme: dark;
}
```

✅ **Correct**
```css
/* Respect user preferences */
:root {
  color-scheme: light dark;
}

/* Only override for specific components if needed */
.demo-dark {
  color-scheme: dark;
}
```

---

## Best Practices

✅ **DO:**
- Set `color-scheme: light dark` on `:root`
- Use custom properties for theme colors
- Provide fallbacks for older browsers
- Test in both light and dark modes
- Use semantic variable names
- Consider accessibility (contrast ratios)

❌ **DON'T:**
- Forget the `color-scheme` property
- Override user preferences without good reason
- Use only for some properties (be consistent)
- Forget to test with actual OS dark mode
- Use too many inline `light-dark()` calls (use variables)

---

## Migration Guide

### Step 1: Add color-scheme
```css
:root {
  color-scheme: light dark;
}
```

### Step 2: Convert simple colors
```css
/* Before */
body {
  background: white;
  color: black;
}

@media (prefers-color-scheme: dark) {
  body {
    background: #1a1a1a;
    color: white;
  }
}

/* After */
body {
  background: light-dark(white, #1a1a1a);
  color: light-dark(black, white);
}
```

### Step 3: Create custom properties
```css
:root {
  color-scheme: light dark;

  --bg: light-dark(white, #1a1a1a);
  --text: light-dark(black, white);
  --accent: light-dark(blue, lightblue);
}
```

### Step 4: Use throughout CSS
```css
body {
  background: var(--bg);
  color: var(--text);
}

a {
  color: var(--accent);
}
```

---

## Key Takeaways

✅ **Advantages:**
- Cleaner, more maintainable code
- No media query duplication
- Single source of truth for colors
- Automatic OS theme detection
- Better developer experience

⚠️ **Remember:**
- Must set `color-scheme: light dark`
- Provide fallbacks for older browsers
- Respect user preferences
- Test in both modes

🎯 **Perfect for:**
- Modern web applications
- Design systems
- Component libraries
- Progressive enhancement
- Accessible theming

---

## See Also

- [CSS color-scheme property](https://developer.mozilla.org/en-US/docs/Web/CSS/color-scheme)
- [prefers-color-scheme media query](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)
- [CSS Color Module Level 5](https://drafts.csswg.org/css-color-5/)
- [Web.dev: prefers-color-scheme](https://web.dev/prefers-color-scheme/)

---

**Last Updated:** January 2026
**Status:** ✅ Baseline 2024 - Production Ready
