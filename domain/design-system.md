---
title: Design System
description: Glossary for the dnd-mapp design system - design tokens, Brand × Mode infrastructure, and Base/Reset styles
published: true
date: 2026-08-05T14:35:00.000Z
tags: domain, glossary, design-system, css, scss
editor: markdown
dateCreated: 2026-08-03T16:00:00.000Z
---

The shared vocabulary for the design system used across `dnd-mapp` apps: design tokens, a Brand × Mode resolution mechanism, and (eventually) a small internal component library built on `@angular/cdk` primitives. Currently implemented inside [angular-app-template](https://github.com/dnd-mapp/angular-app-template)'s `src/design-system/` folder; see [ADR 0001](/decisions/adr-0001-design-system-in-template-repo-for-now) for the plan to extract it into its own shared package.

## Language

### Design System

The shared vocabulary of design tokens plus a small internal component library, built on `@angular/cdk` primitives, that gives `dnd-mapp` apps a consistent look. Being built inside `angular-app-template` for now so it can be iterated on quickly; the plan is to extract it into its own shared package (published to the same registry the Dockerfile already authenticates against) before any real app is bootstrapped from that template, so multiple apps can consume and stay in sync with it.

### Phased scope

This first pass covers [Design Token](#design-token)s, the [Brand](#brand) × [Mode](#mode) infrastructure, and [Base Styles](#base-styles)/[Reset](#reset) (applying tokens to real elements and neutralizing browser UA defaults). The component library half of this definition is the long-term target, not part of the current effort: no shared components are being built yet, so the `dma` selector prefix and the plain-folder (not library-project) structure are forward-looking decisions with nothing built against them today.\
_Avoid_: UI library, style guide, component kit

### Brand

A named, per-app visual identity (accent colors, typography, etc.) within the design system. Exactly one brand is baked into a given app's build: an app never switches its own brand at runtime. Selected by the app's own global stylesheet doing a single `@use "@dnd-mapp/design-system/brands/<name>"` (no build configuration or file-replacement machinery; an app is permanently one brand). A separate preview/catalog build (not covered yet) is how multiple brands would ever be viewed side by side.\
_Avoid_: Theme, skin (when used to mean brand specifically; "theme" is reserved for [Mode](#mode) in this project)

### Mode

Light or dark rendering of a [Brand](#brand). Unlike Brand, Mode is a runtime concern: a running app can switch between its Mode(s) live. Selected via a `data-mode="light"` / `data-mode="dark"` attribute on `<html>`; when the attribute is absent, mode follows the `prefers-color-scheme` media query by default.\
_Avoid_: Theme (ambiguous with Brand; use Mode specifically for light/dark)

### Design Token

A named design value (color, spacing, radius, type scale, etc.), layered in three tiers:

- **Primitive Token**: a raw scale value, agnostic to [Brand](#brand) and [Mode](#mode) (e.g. a color ramp step). Implemented as a Sass variable/map, compile-time only; never emitted as a CSS custom property, since nothing at runtime needs to read one directly.
- **Semantic Token**: a role-based name (e.g. "surface color", "primary text color") whose value resolves differently per [Brand](#brand) × [Mode](#mode), uniformly across every token category (color, spacing, typography, and future categories), even for categories where the light/dark values are expected to end up identical in practice, so the resolution mechanism never needs special-casing per category. Components consume these, not primitives.
- **Component Token**: a token scoped to one component (e.g. a button's background), defaulting to a Semantic Token but independently overridable without touching the semantic layer.

_Avoid_: Variable (too generic; use Design Token, or the specific tier, when discussing this project's tokens)

Current category scope: color, spacing, and typography. Radii, elevation/shadow, and motion (duration/easing) are deliberately deferred until a real need for them shows up.

### Semantic Token naming (color)

A layered, relational grammar with two token forms:

- **Context tokens**: the surfaces/backgrounds themselves, flat with no suffix: `--color-surface`, `--color-accent`. Starting context set is just these two (`surface`, `accent`); more (e.g. `inverse`, `overlay`) are added only when a real UI need forces it.
- **Content tokens**: things placed on a context, named `--color-<subject>-on-<context>` (e.g. `--color-text-on-surface`, `--color-text-on-accent`, `--color-border-on-surface`). The `-on-` suffix only applies to content tokens, since a context can't be "on" itself.

_Avoid_: flat unrelated names for content colors (e.g. `--color-text-primary` alone, with no context); every content token must name what it's readable against.

Content token subjects: `text`, `border`, `icon`. `text` additionally carries primary/secondary emphasis (`--color-text-primary-on-surface`, `--color-text-secondary-on-surface`); `border` and `icon` stay single-purpose (`--color-border-on-surface`, `--color-icon-on-surface`) until a real need for emphasis variants on them shows up.

### Semantic Token naming (spacing)

Size-based (t-shirt scale): `--space-xs`, `--space-sm`, `--space-md`, `--space-lg`, `--space-xl`. Each is a thin, qualitative pass-through of the Primitive numeric scale, with no purpose/usage encoded in the name (contrast with color, where relational context is load-bearing).\
_Avoid_: purpose-based names (`--space-inset-sm`, `--space-stack-md`); deferred, only worth the bigger purpose × size taxonomy once real component layouts justify the distinction.\
Five steps to start: `xs`, `sm`, `md`, `lg`, `xl`. No `2xs`/`2xl` extremes until a real screen justifies them.

### Semantic Token naming (typography)

Role-based, composite type styles: each role bundles a matched size/line-height/weight, not independently mixed axes (a heading's size, weight, and line-height are a designed pairing, not free-standing scale values). Per role `R`: `--font-size-R`, `--line-height-R`, `--font-weight-R` (e.g. `--font-size-heading-1`, `--line-height-heading-1`, `--font-weight-heading-1`).\
Starting role set: `heading-1`, `heading-2`, `body`, `caption`, `label`.\
_Avoid_: independent-axis naming (`--font-size-sm`, `--line-height-tight`, `--font-weight-bold` mixed freely); deferred, typography axes aren't reused across roles the way spacing gaps are.

`--font-family-base` is a single global token, outside the per-role bundles, not part of the role grammar above, since only one typeface (Roboto) is loaded today. Forward-looking, not yet built: separate display/heading and monospace font-family tokens once those typefaces are actually added; at that point family may need to join the per-role bundle for heading/display roles specifically.

### Base Styles

A CSS layer that applies resolved [Design Token](#design-token) custom properties to real HTML elements, rather than just declaring them: `body` gets the `surface` context's color tokens plus `--font-family-base`, and each typography role maps onto its most natural element (`heading-1`→`h1`, `heading-2`→`h2`, `body`→`body` itself so it cascades to all text, `caption`→`small`, `label`→`label`). Brand/Mode-agnostic itself: it only references custom property names, which [Brand](#brand) × [Mode](#mode) resolution already fills in.\
_Avoid_: Global styles, Theme (Theme is reserved for [Mode](#mode))

### Reset

A CSS layer that neutralizes browser default UA styles (`box-sizing`, element margins) so nothing fights the values [Base Styles](#base-styles) sets. Doesn't reference any [Design Token](#design-token); identical regardless of which [Brand](#brand) or [Mode](#mode) is active.\
_Avoid_: Normalize (implies a broader cross-browser scope than this repo takes on)

Custom properties are unprefixed (e.g. `--color-surface`, not `--dm-color-surface`). Design-system component selectors use the `dma` prefix (e.g. `dma-button`), distinct from an app's own `app` prefix (`angular.json`'s `schematics` prefix), so design-system components stay visually distinguishable from app-specific ones and need no rename when [Design System](#design-system) is extracted to its own package.

## See also

- [Domain](/domain): index of all domain glossaries.
- [Decisions](/decisions): architecture decision records, filterable by the `domain-design-system` tag.
- [CSS & SCSS Conventions](/development-conventions/css-scss): general CSS/SCSS coding conventions (separate from this design system's own vocabulary).
