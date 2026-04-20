---
agent: code-reviewer
---

# Code Review Checklist

Quick reference checklist for automated code reviews.

## TypeScript

- [ ] No `any` types
- [ ] Explicit return types
- [ ] Proper error handling
- [ ] Type exports with `export type`

## Functions

- [ ] ≤ 14 lines per function
- [ ] One task per function
- [ ] No nested functions
- [ ] Descriptive names

## Files

- [ ] Stores ≤ 100 LOC
- [ ] General ≤ 400 LOC
- [ ] One component per file
- [ ] Barrel exports

## Components

- [ ] Standalone (no NgModule)
- [ ] Signals for state
- [ ] inject() for DI
- [ ] input()/output() API
- [ ] JSDoc on public methods

## SCSS

- [ ] BEM naming
- [ ] CSS variables (no hardcoded)
- [ ] ≤ 3 levels nesting
- [ ] No !important

## Firebase

- [ ] Cleanup listeners
- [ ] Handle permission errors
- [ ] serverTimestamp()
- [ ] No sensitive data

## Documentation

- [ ] JSDoc on public methods
- [ ] @fileoverview header
- [ ] @param with types
- [ ] @returns documented

## Git

- [ ] No emojis in commits
- [ ] Clear commit message
- [ ] Imperative mood
- [ ] ≤ 72 characters first line

---

**Quick Fail Criteria:**

- Any `any` type → Request changes
- Function > 20 lines → Request changes
- File > 500 LOC → Request changes
- No Firebase cleanup → Block merge
- TypeScript errors → Block merge
