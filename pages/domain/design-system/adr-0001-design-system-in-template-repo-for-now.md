---
title: Design system lives in this template repo for now, not its own package
description: Why the design system is built inside angular-app-template rather than a separate package, and when that will change
published: true
date: 2026-08-03T12:00:00.000Z
tags: domain, adr, design-system
editor: markdown
dateCreated: 2026-08-03T12:00:00.000Z
---

This repo is a template for bootstrapping new Angular-based `dnd-mapp` apps, and a design system (tokens, and eventually a small component library) is being built to keep those apps visually consistent. A design system's value depends on multiple apps staying in sync with it, which argues for it living in its own shared package (e.g. `@dnd-mapp/design-system`, published to the registry the Dockerfile already authenticates against) rather than being copy-pasted into each app when it's bootstrapped from the template: copies would diverge immediately with no way to push a fix or new token to all of them.

We're deliberately building it inside this template repo (`src/design-system/`) anyway, for now, so it can be iterated on quickly without the overhead of a second repo/package before it's proven out. It will be extracted into its own package **before any real app is bootstrapped from this template**: extracting any later would mean the first apps already have their own frozen, diverging copy.

## Status

Proposed: extraction to its own package is a prerequisite for this template producing its first real app, not yet scheduled.

## See also

- [Design System](/domain/design-system): the glossary this ADR belongs to.
