---
title: Branch Naming Conventions
description: Format for naming git branches across dnd-mapp repositories
published: true
date: 2026-08-17T14:00:00.000Z
tags: conventions, github, branches
editor: markdown
dateCreated: 2026-08-17T14:00:00.000Z
---

These conventions apply to every repository in the `dnd-mapp` GitHub organization.

## Format

Branches are named `<type>/<slug>`, or `<type>/<issue-number>-<slug>` when the branch is for a tracked GitHub issue:

```text
feat/add-login-flow
fix/123-off-by-one-in-tile-rendering
```

- **`<type>`**: one of the [Conventional Commits](https://www.conventionalcommits.org/) types (`feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`), matching the type of work the branch holds. See [commit conventions](/development-conventions/github#commit-conventions).
- **`<issue-number>`**: the GitHub issue number, with no `#` or leading zeros, when the branch addresses one. Omit it entirely when there isn't one. Don't invent a placeholder.
- **`<slug>`**: lowercase, kebab-case, specific enough to distinguish the branch from another of the same type (`add-login-flow`, not `fix-bug`).

## See also

- [GitHub Repository Conventions](/development-conventions/github): commit message and CI conventions.
- [Development Conventions](/development-conventions): index of all development conventions.
