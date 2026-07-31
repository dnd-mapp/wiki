---
title: Angular & TypeScript Conventions
description: Coding conventions for TypeScript and Angular applications built from angular-app-template
published: true
date: 2026-07-31T15:00:00.000Z
tags: conventions, angular, typescript, accessibility
editor: markdown
dateCreated: 2026-07-31T15:00:00.000Z
---

These conventions apply to TypeScript and Angular code in repositories built from [angular-app-template](https://github.com/dnd-mapp/angular-app-template), currently on Angular v22 and TypeScript v6. They complement the org-wide [GitHub Repository Conventions](/development-conventions/github).

## TypeScript

- Use strict type checking.
- Prefer type inference when the type is obvious; don't annotate what the compiler can already infer.
- Avoid `any`. Use `unknown` when a type is genuinely uncertain, and narrow it before use.

## Components

- Always use standalone components over NgModules. `standalone: true` has been the default for components, directives, and pipes since Angular v19, so don't set it explicitly in the decorator.
- Don't set `changeDetection: ChangeDetectionStrategy.OnPush` explicitly — it's been the default since Angular v22.
- Keep components small and focused on a single responsibility.
- Use the `input()` and `output()` functions instead of the `@Input()`/`@Output()` decorators.
- Use `computed()` for derived state.
- Prefer inline templates for small components; when using external templates or styles, reference them with paths relative to the component's `.ts` file.
- Don't use the `@HostBinding` and `@HostListener` decorators. Angular's style guide now recommends the `host` object on `@Component`/`@Directive` instead: it reads more like the template, keeps host bindings in one scannable place, and — unlike `@HostListener` — supports arbitrary expressions instead of forcing one-line handler methods.
- Use `NgOptimizedImage` for all static images. It doesn't work with inline base64 images, so fall back to a plain `<img>` there.
- Implement lazy loading for feature routes.

## Forms

- Prefer Signal Forms (`@angular/forms/signals`) for new forms. They're stable as of Angular v22 and provide signal-based state, type-safe field access, and schema-based validation.
- When not using Signal Forms, prefer reactive forms over template-driven forms.

## Templates

- Keep templates simple; avoid complex logic in the template itself.
- Use native control flow (`@if`, `@for`, `@switch`) instead of `*ngIf`, `*ngFor`, `*ngSwitch`.
- Use `class` and `style` bindings instead of `ngClass` and `ngStyle`.
- Use the async pipe to handle observables.
- Don't assume runtime globals such as `new Date()` are available directly in templates or components — inject them so behavior stays consistent and testable, including under server-side rendering.

## State management

- Use signals for local component state.
- Use `computed()` for derived state.
- Keep state transformations pure and predictable.
- Don't use `mutate()` on signals; use `update()` or `set()` instead.

## Services

- Design services around a single responsibility.
- Use `inject()` instead of constructor injection.
- Prefer the `@Service` decorator over `@Injectable({ providedIn: 'root' })` for new singleton services. `@Service()` is available since Angular v22 and defaults to root-level provision without needing the explicit option.

## Accessibility

All UI must pass axe accessibility checks and meet WCAG 2.1 AA minimums, including focus management, color contrast, and ARIA attributes.

## See also

- [Development Conventions](/development-conventions): index of all development conventions.
- [GitHub Repository Conventions](/development-conventions/github): commit message and CI conventions.

## Sources

- [Anatomy of components](https://angular.dev/guide/components)
- [The future is standalone!](https://blog.angular.dev/the-future-is-standalone-475d7edbc706)
- [Angular v22: Introducing OnPush by default](https://blog.lacolaco.net/posts/angular-v22-onpush-by-default.en)
- [Host elements](https://angular.dev/guide/components/host-elements)
- [NgOptimizedImage](https://angular.dev/api/common/NgOptimizedImage)
- [Forms · Overview](https://angular.dev/guide/forms/signals/overview)
- [Angular 22 @Service vs @Injectable](https://briantree.se/angular-service-decorator-injectable-replacement/)
- [Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG21/)
- [axe-core](https://github.com/dequelabs/axe-core)
