---
title: CSS & SCSS Conventions
description: Coding conventions for CSS and SCSS in applications built from angular-app-template
published: true
date: 2026-08-03T15:00:00.000Z
tags: conventions, css, scss, angular, styling
editor: markdown
dateCreated: 2026-08-03T15:00:00.000Z
---

These conventions apply to CSS and SCSS in repositories built from [angular-app-template](https://github.com/dnd-mapp/angular-app-template). They complement the [Angular & TypeScript Conventions](/development-conventions/angular-typescript) and the org-wide [GitHub Repository Conventions](/development-conventions/github).

## Sass module system

- Use `@use`, not `@import`. `@import` has been deprecated since Dart Sass 1.80 (October 2024) and is on a multi-year path to removal, targeting Dart Sass 3.0.0. It still compiles today, but don't introduce new usage.
- Prefix files that are only meant to be loaded as modules, not compiled standalone, with an underscore (`_reset.scss`), and load them without the underscore or extension (`@use "reset"`).
- Once a stylesheet forwards more than one partial to consumers, use `@forward` to build a single entrypoint instead of stacking many `@use` lines directly in the file that needs them.
- Keep `@use` namespaced (the default) rather than `as *`. The unnamespaced form is only appropriate for a single stylesheet you fully control, since it risks name collisions.

## Sass variables vs. CSS custom properties

- Use Sass `$variables` for values fixed at build/compile time that never need to change afterward, e.g. a breakpoint referenced only inside a `@media` condition.
- Use CSS custom properties (`--*`, read with `var()`) for anything that needs to vary at runtime: theme colors, per-component overrides, values a media query or JavaScript changes live. They've been Baseline-available since 2017, so there's no compatibility reason to avoid them.

## Modern native CSS

These are all safe to use directly in new styles, without a Sass equivalent:

- `@layer` for cascade priority, instead of fighting selector specificity.
- `@container` queries to make a component responsive to its host element's size rather than only the viewport: the native replacement for `ResizeObserver`-driven responsive components.
- `:has()` to let a parent selector react to its descendants. Avoid anchoring it to broad selectors (`body`, `:root`, `*`); it's a real performance cost.
- Logical properties (`margin-inline`, `padding-block`, etc.) instead of physical ones (`margin-left`, `padding-top`) by default. They cost nothing today and avoid a retrofit if RTL or vertical writing modes are ever needed.
- Native CSS nesting reached Baseline availability in 2024 and is supported broadly, but there's still no need to migrate off Sass's own `&` nesting in SCSS files: native nesting has no equivalent to Sass's `&__element`/`&--modifier` string-concatenation.

## Angular styling

- Rely on the default Emulated view encapsulation; don't set `encapsulation` unless a specific component genuinely needs `ShadowDom`, `ExperimentalIsolatedShadowDom`, or `None`.
- Don't use `::ng-deep` in new code. Angular's own docs call new usage "strongly discouraged": it exists only for backwards compatibility with a deprecated browser mechanism.
- Reference an external stylesheet with the singular `styleUrl`, not the older `styleUrls` array.
- Name a component's stylesheet identically to the component (`<name>.scss`); only split into multiple files with a descriptive suffix (`<name>-<concern>.scss`) when a component's styles genuinely outgrow one file.

## Linting and formatting

`.stylelintrc.json` extends `stylelint-config-standard-scss` and `stylelint-config-recess-order`.

- Property order within a rule follows Recess/Bootstrap grouping, not alphabetical or freeform order.
- `$variables`, `@mixin`s, `@function`s, and `%placeholders` use kebab-case.
- SCSS strings use double quotes: Stylelint and Prettier agree on this, so they won't fight each other.
- Neither config forbids `@import` or `!important`. There's no lint rule to catch either, so avoid them by convention and in review rather than expecting tooling to flag them.

## Architecture

Default to one scoped stylesheet per Angular component, matching Angular's own encapsulation default. This is architecturally closer to BEM-per-component than to a utility-first (Tailwind-style) system, though Angular's mechanical scoping makes BEM's own collision-avoidance naming largely unnecessary.

## See also

- [Development Conventions](/development-conventions): index of all development conventions.
- [Angular & TypeScript Conventions](/development-conventions/angular-typescript): coding conventions for TypeScript and Angular applications built from angular-app-template.

## Sources

- [`@use`](https://sass-lang.com/documentation/at-rules/use/)
- [`@import` is deprecated](https://sass-lang.com/blog/import-is-deprecated/)
- [`@forward`](https://sass-lang.com/documentation/at-rules/forward/)
- [Variables](https://sass-lang.com/documentation/variables/)
- [Using CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)
- [CSS nesting](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting)
- [CSS Nesting Module Level 1](https://www.w3.org/TR/css-nesting-1/)
- [`@layer`](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer)
- [`@container`](https://developer.mozilla.org/en-US/docs/Web/CSS/@container)
- [`:has()`](https://developer.mozilla.org/en-US/docs/Web/CSS/:has)
- [CSS logical properties and values](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_logical_properties_and_values)
- [Styling components](https://angular.dev/guide/components/styling)
- [Angular style guide](https://angular.dev/style-guide)
- [stylelint-config-standard-scss](https://github.com/stylelint-scss/stylelint-config-standard-scss)
- [stylelint-config-recess-order](https://github.com/stormwarning/stylelint-config-recess-order)
