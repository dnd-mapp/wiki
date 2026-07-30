# Agent instructions

## Repository structure

- `pages/` holds the actual Wiki.js content. Only files in this directory should be treated as wiki pages.
- Every other file in this repo (README, CI workflows, editor config, this file, etc.) is repo tooling, not wiki content.

## Wiki page frontmatter

Pages under `pages/` use Wiki.js's YAML frontmatter format:

```yaml
---
title: Page Title
description: Short description
published: true
date: 2026-07-30T11:51:09.000Z
tags: tag-one, tag-two
editor: markdown
dateCreated: 2026-07-30T11:51:09.000Z
---
```

`tags` must be a comma-separated string, not a YAML list. Wiki.js's git storage parser calls `.split(', ')` on it directly; a list value causes an import failure (`_.get(...).split is not a function`).

## The `live` branch

Wiki.js builds pages from the `live` branch, not `main`. `live` is generated automatically by the `publish-live` GitHub Actions workflow, which runs `git subtree split --prefix=pages` on `main` and force-pushes the result. Never edit `live` directly; changes will be overwritten on the next publish run.

## CI conventions

GitHub Actions pinning follows the org-wide convention documented in [GitHub Repository Conventions](https://github.com/dnd-mapp/wiki/blob/main/pages/github-conventions.md).

- The publish workflow authenticates as the `dnd-mapp` GitHub App (via `secrets.GH_APP_CLIENT_ID` and `secrets.GH_APP_PRIVATE_KEY`, both org-level secrets) instead of the default `GITHUB_TOKEN`.

## Commit conventions

Commit message format and commit grouping follow the org-wide convention documented in [GitHub Repository Conventions](https://github.com/dnd-mapp/wiki/blob/main/pages/github-conventions.md).

- Draft commit messages and get confirmation before committing.
