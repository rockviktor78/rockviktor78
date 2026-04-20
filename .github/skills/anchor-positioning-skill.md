# CSS Anchor Positioning Skill

## Overview
This skill provides comprehensive guidance for implementing CSS Anchor Positioning, a modern CSS feature that enables precise positioning of elements relative to other "anchor" elements without JavaScript. This is particularly useful for tooltips, dropdowns, popovers, context menus, and floating UI components.

## Browser Support Status
- **Chromium**: Chrome 125+, Edge 125+ (Full support)
- **Firefox**: In development (use polyfill)
- **Safari**: In development (use polyfill)

Always check current browser support at [caniuse.com/css-anchor-positioning](https://caniuse.com/css-anchor-positioning) and consider using the official polyfill for production.

## Core Concepts

### 1. Defining an Anchor
Use `anchor-name` to designate an element as an anchor:

```css
.button {
  anchor-name: --my-anchor;
}
```

### 2. Linking to an Anchor
Use `position-anchor` to connect a positioned element to its anchor:

```css
.tooltip {
  position: absolute;
  position-anchor: --my-anchor;
}
```

### 3. Position Functions and Properties

#### `anchor()` Function
The `anchor()` function allows precise positioning relative to anchor edges:

```css
.tooltip {
  position: absolute;
  position-anchor: --my-anchor;
  
  /* Position tooltip's bottom at anchor's top */
  bottom: anchor(top);
  
  /* Align left edge with anchor's left edge */
  left: anchor(left);
  
  /* Center horizontally */
  left: anchor(center);
  transform: translateX(-50%);
}
```

**Available anchor sides:**
- `top`, `bottom`, `left`, `right`
- `center` (horizontal/vertical center)
- `start`, `end` (logical properties)
- Percentage values: `anchor(50%)`, `anchor(25%)`

#### `position-area` Property
Shorthand for common positioning patterns:

```css
.tooltip {
  position: absolute;
  position-anchor: --my-anchor;
  position-area: top;
}
```

**Position area values:**
- Single values: `top`, `bottom`, `left`, `right`
- Combinations: `top left`, `top center`, `top right`, `bottom left`, etc.
- Span values: `top span-left`, `bottom span-right`
- All: `block-start`, `block-end`, `inline-start`, `inline-end`

**Visual grid reference:**
```
top-left        top         top-right
left          center          right
bottom-left   bottom      bottom-right
```

### 4. Anchor Sizing
Use `anchor-size()` to size elements based on anchor dimensions:

```css
.dropdown {
  position: absolute;
  position-anchor: --trigger;
  
  /* Match anchor width */
  width: anchor-size(width);
  
  /* Match anchor height */
  min-height: anchor-size(height);
  
  /* Use anchor dimensions in calc */
  width: calc(anchor-size(width) * 1.5);
}
```

### 5. Fallback Positioning with `position-try`
Automatically reposition when there's insufficient space:

```css
.tooltip {
  position: absolute;
  position-anchor: --my-anchor;
  position-area: top;
  
  /* Try bottom if top doesn't fit */
  position-try-options: flip-block;
}
```

**Built-in position-try options:**
- `flip-block` - Flip vertically (top ↔ bottom)
- `flip-inline` - Flip horizontally (left ↔ right)
- `flip-start` - Flip along start axis

**Custom fallback positions:**
```css
@position-try --bottom-fallback {
  position-area: bottom;
  margin-top: 0;
  margin-bottom: 8px;
}

@position-try --left-fallback {
  position-area: left;
  margin-right: 8px;
}

.tooltip {
  position: absolute;
  position-anchor: --my-anchor;
  position-area: top;
  margin-top: 8px;
  
  position-try-options: --bottom-fallback, --left-fallback;
}
```

### 6. Anchor Scope
Control anchor name visibility to avoid conflicts:

```css
.container {
  anchor-scope: --my-anchor;
}

/* Anchor only visible within .container */
.button {
  anchor-name: --my-anchor;
}
```

## Common Patterns

### Pattern 1: Tooltip
```css
/* Anchor element */
.tooltip-trigger {
  anchor-name: --tooltip;
}

/* Tooltip */
.tooltip {
  position: absolute;
  position-anchor: --tooltip;
  position-area: top;
  margin-bottom: 8px;
  
  /* Fallback to bottom if no space above */
  position-try-options: flip-block;
  
  /* Optional: center alignment */
  left: anchor(center);
  transform: translateX(-50%);
}
```

### Pattern 2: Dropdown Menu
```css
.dropdown-trigger {
  anchor-name: --dropdown;
}

.dropdown-menu {
  position: absolute;
  position-anchor: --dropdown;
  position-area: bottom;
  
  /* Match trigger width */
  width: anchor-size(width);
  
  /* Margin for spacing */
  margin-top: 4px;
  
  /* Flip if no space below */
  position-try-options: flip-block;
}
```

### Pattern 3: Context Menu
```css
.context-trigger {
  anchor-name: --context;
}

.context-menu {
  position: fixed; /* Use fixed for viewport positioning */
  position-anchor: --context;
  
  /* Default: bottom-right */
  position-area: bottom right;
  
  /* Try multiple fallbacks */
  position-try-options: 
    flip-block,
    flip-inline,
    --top-left-fallback;
}

@position-try --top-left-fallback {
  position-area: top left;
}
```

### Pattern 4: Popover with Arrow
```css
.button {
  anchor-name: --popover-anchor;
}

.popover {
  position: absolute;
  position-anchor: --popover-anchor;
  position-area: top;
  margin-bottom: 12px;
  position-try-options: flip-block;
}

/* Arrow pointing to anchor */
.popover::after {
  content: '';
  position: absolute;
  
  /* Center arrow horizontally */
  left: anchor(--popover-anchor center);
  transform: translateX(-50%);
  
  /* Position at bottom of popover */
  bottom: -6px;
  
  /* Arrow styling */
  width: 0;
  height: 0;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-top: 6px solid white;
}
```

### Pattern 5: Multi-Anchor (Inline Annotations)
```css
/* Multiple anchors in text */
.word-1 { anchor-name: --word-1; }
.word-2 { anchor-name: --word-2; }
.word-3 { anchor-name: --word-3; }

/* Annotation for specific word */
.annotation {
  position: absolute;
  
  /* Dynamically set via inline style or JS */
  position-anchor: var(--anchor-name);
  
  position-area: top;
  margin-bottom: 4px;
}
```

### Pattern 6: Sticky Header with Anchor
```css
.section {
  anchor-name: --section;
}

.floating-action {
  position: sticky;
  top: 20px;
  
  /* Align with section edge */
  left: anchor(--section right);
  margin-left: 16px;
}
```

## Best Practices

### 1. Always Provide Fallbacks
```css
.tooltip {
  /* Old browser fallback */
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  
  /* Modern anchor positioning (will override above) */
  position-anchor: --my-anchor;
  position-area: bottom;
  left: anchor(center);
  transform: translateX(-50%);
  
  @supports (position-anchor: --my-anchor) {
    top: auto;
  }
}
```

### 2. Use Logical Properties
Prefer `block-start`, `block-end`, `inline-start`, `inline-end` for better internationalization:

```css
.tooltip {
  position-area: block-start; /* Instead of 'top' */
  margin-block-end: 8px; /* Instead of 'margin-bottom' */
}
```

### 3. Scope Anchors Appropriately
```css
/* Prevent anchor name conflicts */
.card {
  anchor-scope: --card-anchor;
}

.card-button {
  anchor-name: --card-anchor;
}

.card-tooltip {
  position-anchor: --card-anchor;
}
```

### 4. Consider z-index Stacking
```css
.tooltip {
  position: absolute;
  position-anchor: --my-anchor;
  z-index: 1000; /* Ensure tooltip appears above other content */
}
```

### 5. Add Smooth Transitions
```css
.dropdown-menu {
  position: absolute;
  position-anchor: --dropdown;
  
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s, transform 0.2s;
  
  &[data-open] {
    opacity: 1;
    pointer-events: auto;
  }
}
```

## Polyfill Usage

For cross-browser support, use the official CSS Anchor Positioning Polyfill:

```html
<!-- Via CDN -->
<script type="module">
  import 'https://cdn.jsdelivr.net/npm/@oddbird/css-anchor-positioning@0.1.0/dist/css-anchor-positioning.min.js';
</script>
```

Or via npm:
```bash
npm install @oddbird/css-anchor-positioning
```

```javascript
import '@oddbird/css-anchor-positioning';
```

### Polyfill Limitations
- May have performance implications with many anchors
- Some edge cases might not work identically to native implementation
- Test thoroughly in target browsers

## Feature Detection

```css
@supports (position-anchor: --test) {
  /* Use anchor positioning */
  .tooltip {
    position-anchor: --my-anchor;
    position-area: top;
  }
}

@supports not (position-anchor: --test) {
  /* Fallback positioning */
  .tooltip {
    position: absolute;
    bottom: 100%;
    left: 50%;
    transform: translateX(-50%);
  }
}
```

JavaScript detection:
```javascript
if (CSS.supports('position-anchor', '--test')) {
  // Native support
} else {
  // Load polyfill or use fallback
}
```

## Common Pitfalls

### 1. Forgetting `position` Property
```css
/* ❌ Wrong - anchor positioning requires position: absolute/fixed */
.tooltip {
  position-anchor: --my-anchor;
}

/* ✅ Correct */
.tooltip {
  position: absolute;
  position-anchor: --my-anchor;
}
```

### 2. Anchor Element Must Be in DOM
```css
/* ❌ Anchor element display:none won't work */
.hidden-anchor {
  display: none;
  anchor-name: --my-anchor;
}

/* ✅ Use visibility:hidden or opacity:0 instead */
.hidden-anchor {
  visibility: hidden;
  anchor-name: --my-anchor;
}
```

### 3. Circular Dependencies
```css
/* ❌ Don't create circular anchor relationships */
.element-a {
  anchor-name: --anchor-a;
  position-anchor: --anchor-b;
}

.element-b {
  anchor-name: --anchor-b;
  position-anchor: --anchor-a;
}
```

### 4. Overflow Issues
```css
/* Ensure positioned elements aren't clipped */
.container {
  overflow: visible; /* Not 'hidden' or 'auto' if tooltip extends outside */
}

/* Or use position: fixed for viewport-relative positioning */
.tooltip {
  position: fixed;
  position-anchor: --my-anchor;
}
```

## Debugging Tips

### 1. Visualize Anchor Bounds
```css
/* Temporarily add outline to see anchor element */
[anchor-name] {
  outline: 2px solid red;
}
```

### 2. Check Positioning Context
```css
/* Verify the positioned element is rendered */
.tooltip {
  background: yellow;
  min-width: 50px;
  min-height: 50px;
}
```

### 3. Console Logging (with Polyfill)
```javascript
// Check if polyfill is active
console.log('Polyfill active:', !!window.CSSAnchorPositioning);
```

## Advanced Patterns

### Dynamic Anchor Assignment
```html
<button class="trigger" data-anchor="button-1">Button 1</button>
<button class="trigger" data-anchor="button-2">Button 2</button>

<div class="shared-tooltip"></div>
```

```css
.trigger[data-anchor="button-1"] { anchor-name: --button-1; }
.trigger[data-anchor="button-2"] { anchor-name: --button-2; }

.shared-tooltip {
  position: absolute;
  /* Set dynamically via inline style */
  position-anchor: var(--current-anchor);
}
```

```javascript
document.querySelectorAll('.trigger').forEach(btn => {
  btn.addEventListener('mouseenter', () => {
    const tooltip = document.querySelector('.shared-tooltip');
    tooltip.style.setProperty('--current-anchor', `--${btn.dataset.anchor}`);
  });
});
```

### Anchor with Scroll Containers
```css
.scroll-container {
  overflow: auto;
  anchor-scope: --scroll-anchor; /* Limit scope to container */
}

.item {
  anchor-name: --scroll-anchor;
}

.floating-label {
  position: absolute;
  position-anchor: --scroll-anchor;
  position-area: right;
}
```

## Performance Considerations

1. **Limit number of anchors**: Each anchor requires calculation
2. **Use `anchor-scope`**: Reduces complexity by limiting anchor visibility
3. **Avoid frequent updates**: Anchor calculations happen on layout/scroll
4. **Consider `content-visibility`**: Can improve performance for off-screen anchors

## Resources

- [MDN: CSS Anchor Positioning](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_anchor_positioning)
- [W3C Spec](https://drafts.csswg.org/css-anchor-position-1/)
- [Official Polyfill](https://github.com/oddbird/css-anchor-positioning)
- [Chrome Developers Guide](https://developer.chrome.com/blog/anchor-positioning-api/)

## Summary Cheatsheet

```css
/* Define anchor */
.anchor { anchor-name: --my-anchor; }

/* Link to anchor */
.positioned {
  position: absolute;
  position-anchor: --my-anchor;
  
  /* Method 1: position-area (simple) */
  position-area: top;
  
  /* Method 2: anchor() function (precise) */
  bottom: anchor(top);
  left: anchor(left);
  
  /* Method 3: anchor-size() for dimensions */
  width: anchor-size(width);
  
  /* Fallbacks */
  position-try-options: flip-block, flip-inline;
  
  /* Scope */
  anchor-scope: --my-anchor;
}
```
