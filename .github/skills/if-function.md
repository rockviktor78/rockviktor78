# CSS if() Function

> **⚠️ Experimental Technology**
> Limited browser support - check compatibility before using in production!

## Overview

The `if()` CSS function allows different values to be set for a property depending on the result of a conditional test. The test can be based on:
- **Style queries** (custom properties)
- **Media queries** (viewport, orientation, etc.)
- **Feature queries** (browser support)

---

## Syntax

```css
/* Single <if-test> */
if(style(--scheme: dark): #eeeeee;)
if(media(print): black;)
if(media(width > 700px): 0 auto;)
if(supports(color: lch(7.1% 60.23 300.16)): lch(7.1% 60.23 300.16);)

/* <if-test> with else */
if(style(--size: "2xl"): 1em; else: 0.25em;)
if(media(print): white; else: black;)
if(media(width < 700px): 0 auto; else: 20px auto)
if(
  supports(color: lch(7.1% 60.23 300.16)): lch(7.1% 60.23 300.16);
  else: #03045e;
)

/* Multiple <if-test>s */
if(
  style(--scheme: ice): linear-gradient(#caf0f8, white, #caf0f8);
  style(--scheme: fire): linear-gradient(#ffc971, white, #ffc971);
  else: none;
)

/* <if-test> within a shorthand */
3px yellow if(
  style(--color: green): dashed;
  style(--color: yellow): inset;
  else: solid;
)
```

---

## Parameters

**Semi-colon-separated list of `<if-branch>`es:**

```
<if-branch> = <if-condition> : <value>;
```

- **`<if-condition>`**: Either an `<if-test>` or the `else` keyword
- **`<if-test>`**: A style query, media query, or feature query
- **`else`**: Keyword that always evaluates to true (fallback)
- **`<value>`**: A property value

---

## Return Value

- Returns the first matching `<value>` when `<if-condition>` evaluates to true
- Returns `guaranteed-invalid` if no condition matches (behaves as invalid/false)

---

## How It Works

```css
div {
  background-image: if(
    style(--scheme: ice): linear-gradient(#caf0f8, white, #caf0f8);
    style(--scheme: fire): linear-gradient(#ffc971, white, #ffc971);
    else: none;
  );
}
```

1. Evaluates conditions **in order**
2. Returns value of **first true condition**
3. Falls back to `else` if nothing matches
4. Returns `guaranteed-invalid` if no `else` and nothing matches

---

## Important Rules

✅ **DO:**
- Separate condition from value with colon `:`
- Separate pairs with semicolon `;`
- Last semicolon is optional
- No space between `if` and `(`

❌ **DON'T:**
```css
/* Invalid - space before ( */
margin: if (media(width > 700px): 0 auto;);

/* Invalid - missing colon */
color: if(style(--dark) black;);
```

---

## Types of if-tests

### 1. Style Queries (Custom Properties)

Check if a custom property is set to a specific value:

```css
background-image: if(
  style(--scheme: ice): linear-gradient(#caf0f8, white, #caf0f8);
  else: none;
);

/* With logic operators */
background-color: if(
  style((--scheme: dark) or (--scheme: very-dark)): black;
);

background-color: if(
  style((--scheme: dark) and (--contrast: hi)): black;
);

background-color: if(
  not style(--scheme: light): black;
);
```

**Limitation:** Only works with custom properties, not regular CSS properties.

---

### 2. Media Queries

Conditional values based on viewport, orientation, etc:

```css
/* Media types */
background-color: if(
  media(print): white;
  else: #eeeeee;
)

/* Media features */
margin: if(
  media(width < 700px): 0 auto;
  else: 20px auto;
)

/* With logic operators */
border-color: if(
  media((width > 700px) and (width < 1000px)): blue;
);

border-color: if(
  media((width < 500px) or (orientation: landscape)): blue;
);

background-color: if(
  not media(width < 500px): blue;
  else: red
);
```

---

### 3. Feature Queries

Test browser support for features:

```css
/* Color support */
color: if(
  supports(color: lch(75% 0 0)): lch(75% 0 0);
  else: rgb(185 185 185);
)

/* Selector support */
margin-top: if(
  supports(selector(:buffering)): 1em;
  else: initial;
)

/* With logic operators */
margin-top: if(
  supports((selector(:buffering)) and (color: blue)): 1em;
);

margin-top: if(
  supports((selector(:buffering)) or (color: not-a-color)): 1em;
);

margin-top: if(
  supports(not selector(:buffering)): 1em;
);
```

---

## Multiple else Conditions

You can have multiple `else` pairs, but typically one at the end:

```css
/* Typical usage - else at end */
background-image: if(
  style(--scheme: ice): linear-gradient(#caf0f8, white, #caf0f8);
  style(--scheme: fire): linear-gradient(#ffc971, white, #ffc971);
  else: none;
);

/* Debugging - else in middle stops evaluation */
background-image: if(
  style(--scheme: ice): linear-gradient(#caf0f8, white, #caf0f8);
  else: url("debug.png");
  style(--scheme: fire): linear-gradient(#ffc971, white, #ffc971);
  else: none;
);
```

⚠️ **Warning:** `else` always evaluates to true, so conditions after it are never reached!

---

## Providing Fallbacks

**Always provide fallback for non-supporting browsers:**

```css
/* Fallback first, then if() override */
padding: 1em;
padding: if(style(--size: "2xl"): 1em; else: 0.25em);
```

Browsers without `if()` support use `1em`, supporting browsers override with conditional value.

---

## Whole vs Partial Values

### Whole property value:
```css
border: if(
  supports(color: lch(75% 0 0)): 3px solid lch(75% 0 0);
  else: 3px solid silver;
);
```

### Partial component:
```css
border: 3px solid
  if(
    supports(color: lch(75% 0 0)): lch(75% 0 0);
    else: silver;
  );
```

---

## Nesting if() Functions

### Nested if() inside if():
```css
color: if(
  style(--scheme: ice):
    if(
      media(prefers-color-scheme: dark): #caf0f8;
      else: #03045e;
    );
  style(--scheme: fire):
    if(
      media(prefers-color-scheme: dark): #ffc971;
      else: #621708;
    );
  else: black
);
```

### if() inside calc():
```css
width: calc(if(
    style(--scheme: wide): 70%;
    else: 50%;
  ) - 50px);
```

---

## Practical Examples

### Responsive Layout
```css
section {
  display: flex;
  flex-direction: if(
    media(orientation: landscape): row;
    else: column;
  )
}
```

### Conditional Content
```css
h2::before {
  content: if(
    style(--show-emoji: true): "🍎 ";
  );
}
```

### Progressive Enhancement
```css
h2 {
  color: if(
    supports(color: lch(29.57% 43.25 344.44)): lch(29.57% 43.25 344.44);
    else: #792359;
  )
}
```

### Theming with Custom Properties
```css
article {
  --color1: if(
    style(--scheme: ice): #03045e;
    style(--scheme: fire): #621708;
    else: black;
  );
  --color2: if(
    style(--scheme: ice): #caf0f8;
    style(--scheme: fire): #ffc971;
    else: white;
  );

  color: var(--color1);
  background: var(--color2);
}
```

### Dynamic Font Size
```css
h1 {
  font-size: if(
    media(width > 700px): calc(3rem + 2vw);
    else: 3rem;
  );
}
```

### Responsive Spacing
```css
body {
  margin: if(
    media(width < 700px): 0;
    else: 20px;
  ) auto 0;
}
```

---

## vs Traditional Approaches

### if() vs @media
```css
/* Traditional @media */
.box {
  margin: 0 auto;
}
@media (width < 700px) {
  .box {
    margin: 20px auto;
  }
}

/* With if() - single property only */
.box {
  margin: if(
    media(width < 700px): 0 auto;
    else: 20px auto;
  );
}
```

**Use `@media` when:** Setting multiple properties
**Use `if()` when:** Varying single property value

### if() vs @supports
```css
/* Traditional @supports */
.element {
  color: rgb(185 185 185);
}
@supports (color: lch(75% 0 0)) {
  .element {
    color: lch(75% 0 0);
  }
}

/* With if() - inline */
.element {
  color: if(
    supports(color: lch(75% 0 0)): lch(75% 0 0);
    else: rgb(185 185 185);
  );
}
```

**Use `@supports` when:** Multiple properties/rules
**Use `if()` when:** Single value variation

### if() vs @container (Style Queries)
```css
/* @container - sets multiple rules */
.card {
  container: my-container / inline-size;
}
@container style(--scheme: dark) {
  .card__title {
    color: white;
    background: black;
    border: 1px solid gray;
  }
}

/* if() - single property, direct targeting */
.card {
  background: if(
    style(--scheme: dark): black;
    else: white;
  );
}
```

**Advantage of `if()`:** Can target element directly without container parent

---

## Browser Compatibility

⚠️ **As of 2026:** Experimental feature with limited support

Check current compatibility:
- [MDN Browser Compatibility Table](https://developer.mozilla.org/en-US/docs/Web/CSS/if)
- [Can I Use - CSS if()](https://caniuse.com/?search=css%20if)

**Always provide fallbacks for production use!**

---

## Key Takeaways

✅ **Use if() for:**
- Single property value variations
- Inline conditional logic
- Combining style/media/feature queries
- Theming with custom properties
- Progressive enhancement

❌ **Don't use if() for:**
- Multiple property changes (use `@media`, `@supports`, `@container`)
- Production without fallbacks
- Regular CSS properties in style queries (only custom properties work)

🔮 **Future potential:**
- Cleaner, more maintainable CSS
- Less repetition of selectors
- More powerful theming systems
- Better component-level styling

---

## See Also

- [CSS Container Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)
- [CSS @supports](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports)
- [CSS @media](https://developer.mozilla.org/en-US/docs/Web/CSS/@media)
- [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)
- [CSS calc()](https://developer.mozilla.org/en-US/docs/Web/CSS/calc)

---

**Last Updated:** January 2026
**Status:** Experimental - Use with caution and fallbacks
