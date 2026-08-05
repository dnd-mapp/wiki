---
title: Publish Docker images to Docker Hub instead of GHCR
description: Why this repo's CI publishes to Docker Hub, and the conventions that come with it
published: true
date: 2026-08-05T12:00:00.000Z
tags: conventions, adr, github, ci, docker
editor: markdown
dateCreated: 2026-08-05T12:00:00.000Z
---

[`template-app-angular`](https://github.com/dnd-mapp/template-app-angular) (and any `dnd-mapp` repo bootstrapped from it) previously published its Docker image to GHCR, authenticated via `GITHUB_TOKEN` or a GitHub App token. It now publishes to Docker Hub instead, under the org's existing `dndmapp` account, replacing GHCR entirely rather than publishing to both.

Two things fall out of that switch that aren't obvious from the workflow YAML alone:

- **The namespace can't come from `github.repository_owner`.** The Docker Hub account is `dndmapp` (no hyphen), unlike the GitHub org `dnd-mapp`, so image references are built from the `DH_USERNAME` org secret instead. Any repo bootstrapped from this template needs that secret set, not just a GitHub-side permission; the two names can't be derived from each other.
- **`pr-<N>` tag cleanup on PR close now actually works.** GHCR can only delete a whole package *version*, which can carry more than one tag (e.g. a promoted `next`), so the old cleanup workflow refused to delete anything shared with another tag; `pr-<N>` tags accumulated indefinitely once promoted. Docker Hub's tag-delete API (`DELETE /v2/repositories/<namespace>/<repo>/tags/<tag>/`) removes exactly one tag without touching others on the same digest, so that guard is gone and cleanup now runs unconditionally.

Auth is split across three Docker Hub access tokens, scoped to the least privilege each job needs, paired with `DH_USERNAME`:

| Secret                 | Scope             | Used by                                                             |
|------------------------|-------------------|---------------------------------------------------------------------|
| `DH_READONLY`          | Read              | Looking up whether a `pr-<N>` image exists                          |
| `DH_READ_WRITE`        | Read/write        | Pushing `pr-<N>`, promoting to `next`, syncing the repo description |
| `DH_READ_WRITE_DELETE` | Read/write/delete | Removing a `pr-<N>` tag once its pull request closes                |

## Status

Accepted.

## See also

- [GitHub Repository Conventions](/development-conventions/github): the CI conventions this ADR's summary bullet belongs to.
