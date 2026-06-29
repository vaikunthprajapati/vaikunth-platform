---
id: ER-001
title: Engineering Rules
version: 1.0
status: Approved
owner: Vaikunth Prajapati
reviewer: ChatGPT
last_updated: 2026-06-30
---

# Engineering Rules

## General Principles

- Follow existing project architecture.
- Never introduce unnecessary complexity.
- Prefer readable code over clever code.
- Optimize for maintainability first.
- Every function should have a single responsibility.
- Every component should have a clear purpose.

---

## Project Structure

- Respect feature-first architecture.
- Shared logic belongs only inside shared directories.
- Do not duplicate utilities.
- Keep folder hierarchy consistent.

---

## TypeScript

Always

- use strict typing
- avoid `any`
- define reusable interfaces
- prefer inferred types where obvious

---

## React

- Prefer Server Components by default.
- Use Client Components only when necessary.
- Keep components small.
- Extract reusable UI.
- Avoid prop drilling where practical.

---

## Naming

Folders

- kebab-case

Components

- PascalCase

Functions

- camelCase

Constants

- UPPER_SNAKE_CASE

---

## Styling

- Tailwind CSS only.
- Avoid inline styles.
- Use design tokens where available.
- Keep spacing consistent.

---

## Performance

- Avoid unnecessary re-renders.
- Lazy load heavy components.
- Optimize images.
- Minimize bundle size.

---

## Accessibility

Every feature should support

- keyboard navigation
- semantic HTML
- accessible labels
- sufficient color contrast

---

## Documentation

Every significant architectural decision must be documented before implementation.

---

## AI Requirements

When generating code:

- Explain architectural reasoning.
- Explain trade-offs.
- Avoid unnecessary dependencies.
- Follow project conventions.
- Produce production-ready code.
- Never sacrifice maintainability for brevity.