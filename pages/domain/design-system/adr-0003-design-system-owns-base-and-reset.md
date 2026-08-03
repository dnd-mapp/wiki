---
title: Design system owns Base Styles and Reset, not each consuming app
description: Why Base Styles and Reset live in the design system rather than each consuming app
published: true
date: 2026-08-03T15:30:00.000Z
tags: domain, adr, design-system
editor: markdown
dateCreated: 2026-08-03T15:30:00.000Z
---

Emitting Semantic Token custom properties (the tokens-layer work) turned out to have no visible effect on its own — nothing consumed them on real elements, so an app bootstrapped from the template would render completely unstyled until it hand-wrote its own `body`/heading/reset CSS. We decided the design system owns this instead: a **Base Styles** layer (`src/design-system/_base.scss`) applies resolved Semantic Tokens to real elements — `body` gets the `surface` context's color tokens plus `--font-family-base`, and each typography role maps onto its most natural element (`heading-1`→`h1`, `heading-2`→`h2`, `body`→`body` itself so it cascades to all text, `caption`→`small`, `label`→`label`) — and a **Reset** layer (`src/design-system/_reset.scss`) neutralizes browser UA defaults (`box-sizing`, element margins) so nothing fights those values.

The alternative — leaving base/reset CSS to each consuming app — would mean every app re-deriving the same boilerplate, with no guarantee they'd agree, defeating the point of a shared design system. Both layers are Brand/Mode-agnostic: Reset never references a token, and Base only references custom property *names*, which Brand × Mode resolution already fills in — so neither needs to change when a new Brand is added.

## Status

Accepted.

## See also

- [Design System](/domain/design-system): the glossary this ADR belongs to.
