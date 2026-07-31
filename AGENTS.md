# Agent instructions

## Repository structure

- `pages/` holds the actual Wiki.js content. Only files in this directory should be treated as wiki pages.
- Every other file in this repo (README, CI workflows, editor config, this file, etc.) is repo tooling, not wiki content.

## Wiki page frontmatter

Pages under `pages/` use Wiki.js' YAML frontmatter format:

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

`tags` must be a comma-separated string, not a YAML list. Wiki.js' git storage parser calls `.split(', ')` on it directly; a list value causes an import failure (`_.get(...).split is not a function`).

Don't add an H1 heading that duplicates the frontmatter `title`. Wiki.js renders the title above the page content automatically, so a matching H1 is redundant.

## Linking conventions

- Links between pages within this wiki use relative paths, without the `pages/` prefix or `.md` extension (e.g. `/development-conventions/github`).
- Wiki.js folders are virtual and inferred from page paths. There's no automatic "folder index" by matching filename to folder name: a file at `development-conventions/development-conventions.md` resolves at `/development-conventions/development-conventions`, not `/development-conventions`. To give a virtual folder a landing page, create a sibling file outside the folder, named after the folder itself. For example, `development-conventions.md` next to the `development-conventions/` folder gives the `/development-conventions` route. See the [Folder Structure guide](https://docs.requarks.io/guide/structure).
- The live wiki (wiki.dndmapp.nl.eu.org) sits behind Cloudflare with AI bot traffic blocked, so an agent can't fetch it. Links meant to be followed by agents, such as references from another repo's `AGENTS.md`, should point at the GitHub source file instead (e.g. `https://github.com/dnd-mapp/wiki/blob/main/pages/development-conventions/github.md`).

## The `live` branch

Wiki.js builds pages from the `live` branch, not `main`. `live` is generated automatically by the `publish-live` GitHub Actions workflow, which runs `git subtree split --prefix=pages` on `main` and force-pushes the result. Never edit `live` directly; changes will be overwritten on the next publish run.

A repository ruleset ("Live branch") restricts creation, deletion, updates, and force pushes on `live` to the `dnd-mapp` GitHub App, the same app `publish-live` authenticates as. Manual pushes or deletes from anyone else are rejected.

## CI conventions

GitHub Actions pinning follows the org-wide convention documented in [GitHub Repository Conventions](https://github.com/dnd-mapp/wiki/blob/main/pages/development-conventions/github.md).

- The publish workflow authenticates as the `dnd-mapp` GitHub App (via `secrets.GH_APP_CLIENT_ID` and `secrets.GH_APP_PRIVATE_KEY`, both org-level secrets) instead of the default `GITHUB_TOKEN`.

## Commit conventions

Commit message format and commit grouping follow the org-wide convention documented in [GitHub Repository Conventions](https://github.com/dnd-mapp/wiki/blob/main/pages/development-conventions/github.md).

- Draft commit messages and get confirmation before committing.

## Pull request conventions

Pull request title, description, sizing, and review conventions follow the org-wide guide documented in [Creating a Pull Request](https://github.com/dnd-mapp/wiki/blob/main/pages/development-conventions/creating-a-pull-request.md).
