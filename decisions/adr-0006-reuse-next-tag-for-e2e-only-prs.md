---
title: Reuse the `next` tag for E2E-only pull requests instead of rebuilding
description: Why template-app-angular's E2E job tests against next, without rebuilding, when a pull request only touches E2E-relevant paths
published: true
date: 2026-08-12T14:16:45.000Z
tags: adr, ci, domain-docker, domain-docker-publishing
editor: markdown
dateCreated: 2026-08-12T14:16:45.000Z
---

[`template-app-angular`](https://github.com/dnd-mapp/template-app-angular)'s pull-request CI ran the E2E suite only when a change touched [Docker-relevant paths](/domain/docker/publishing#docker-relevant-paths). That's because the E2E job depended on a `docker` job that only builds and pushes a [`pr-<N>` image](/domain/docker/publishing#pr-n-tag) for those changes. A pull request that only touched the E2E suite itself (specs, the Playwright config, or the E2E composite action) never triggered a rebuild. The E2E job was skipped entirely, and the change went unvalidated.

We added a second filter, [E2E-relevant paths](/domain/docker/publishing#e2e-relevant-paths), computed by the renamed `detect-relevant-changes` composite action (formerly `detect-docker-changes`), and changed the E2E job to run whenever either filter matches. When only E2E-relevant paths changed, the job runs against the existing [`next` tag](/domain/docker/publishing#next-tag) instead of triggering a Docker build. The app image is provably unchanged, so rebuilding it just to re-tag it as `pr-<N>` would be pure overhead. The `docker` job's own trigger condition (Docker-relevant paths only) is unchanged.

The alternative was to always rebuild and push a `pr-<N>` image whenever E2E-relevant paths changed too, even with no app-code difference from `next`. That would keep a single image-sourcing path for the E2E job, regardless of which filter tripped. We rejected that: it spends a full Docker build on every E2E-only change for an image that would be byte-for-byte identical to `next`.

Reusing `next` at PR time opens a narrow drift window. Another pull request's image might get promoted to `next` between this PR's own E2E run and when it merges. If that happens, the suite was validated against a `next` that no longer exists by merge time. To close that, `push-main.yml`'s `e2e` job also runs against `next` when the merged pull request was E2E-relevant-only, even though there's no `pr-<N>` image to promote in that case.

## Status

Accepted.

## See also

- [Docker Publishing](/domain/docker/publishing): the glossary this ADR extends.
- [ADR 0004](/decisions/adr-0004-docker-hub-over-ghcr): introduced the `pr-<N>`/`next` tagging model this ADR builds on.
