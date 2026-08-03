---
title: Design-system code is a plain `src/` folder, not an Angular library project
description: Why the design system uses a plain folder instead of an Angular library project, and when to revisit that
published: true
date: 2026-08-03T12:15:00.000Z
tags: domain, adr, design-system
editor: markdown
dateCreated: 2026-08-03T12:15:00.000Z
---

Angular supports multi-project workspaces via `ng generate library`, which would give the design system a compiler-enforced boundary — a `public-api.ts` surface, its own `ng-packagr` build — and would make the eventual extraction to its own package (see [ADR 0001](/domain/design-system/adr-0001-design-system-in-template-repo-for-now)) closer to a straight file move.

We chose a plain `src/design-system/` folder instead, at least while the design system is tokens-only (no shared components yet, see the [Design System](/domain/design-system) glossary's "Phased scope" note). Scaffolding a full library project buys boundary enforcement for component code that doesn't exist yet; a plain folder is faster to start with, at the cost of nothing currently stopping app code and design-system code from silently coupling to each other before extraction.

## Consequences

Revisit this — either add an import-boundary lint rule (e.g. `no-restricted-imports` scoped to `src/design-system/`) or convert to a real library project — once component work under `src/design-system/` actually starts, since that's when the coupling risk this ADR accepts becomes real.

## Status

Accepted for the tokens-only phase; scheduled for re-evaluation when the component-library half of the design system begins.

## See also

- [Design System](/domain/design-system): the glossary this ADR belongs to.
