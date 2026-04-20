```text
wie ich das sehe, haben wir in den pages im body: <body class="desktop-layout"> nur das desktop-layout. Sollten wir da nicht auch mobile-layout haben?
denn wir haben ja auch mobile styles definiert.
```

```text
das fn so nicht. wir können nicht die   .desktop-layout,
  .app-layout im selben styling haben:
  /* APP LAYOUT - Mobile First */
.desktop-layout,
.app-layout {
  /* Mobile: Flexbox Layout */
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  background-color: var(--bg-page);
  font-family: var(--font-primary);
}

/* Desktop: Grid Layout ab 769px */
@media (min-width: 769px) {
  .desktop-layout,
  .app-layout {
    display: grid;
    grid-template-columns: 232px 1fr;
    grid-template-rows: var(--spacing-5xl) 1fr; /* 96px */
  }
}
wie können wir das lÖsen?

```

