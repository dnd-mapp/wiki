---
title: Docker Publishing
description: Glossary for how dnd-mapp repositories build, tag, and publish Docker images to Docker Hub in CI
published: true
date: 2026-08-05T16:10:00.000Z
tags: domain, glossary, docker, publishing, ci
editor: markdown
dateCreated: 2026-08-05T13:15:00.000Z
---

The shared vocabulary for how `dnd-mapp` repositories build, tag, and publish Docker images to Docker Hub in CI, currently implemented in [template-app-angular](https://github.com/dnd-mapp/template-app-angular) and inherited by any repo bootstrapped from it. Related decisions:

- [ADR 0004](/decisions/adr-0004-docker-hub-over-ghcr): why Docker Hub was chosen over GHCR.
- [ADR 0005](/decisions/adr-0005-manual-dockerhub-description-sync): why the repository description is synced by hand rather than by CI.
- [ADR 0006](/decisions/adr-0006-reuse-next-tag-for-e2e-only-prs): why the E2E job reuses `next` instead of rebuilding when a pull request only touches E2E-relevant paths.

## Language

### `pr-<N>` tag

A Docker Hub image tag built from pull request `<N>`, produced whenever that pull request touches [Docker-relevant paths](#docker-relevant-paths). Removed once the pull request closes, whether merged or not: on merge, after it's been promoted to `next`; on close without merging, immediately.\
_Avoid_: PR tag, preview tag

### `next` tag

A Docker Hub image tag that always reflects the current tip of `main`. Produced by retagging the most recently merged pull request's [`pr-<N>`](#pr-n-tag) image, not by rebuilding, so promotion is instant and byte-for-byte identical to what CI already verified. Also the image a pull request's E2E job tests against directly, without any promotion, when that pull request touches only [E2E-relevant paths](#e2e-relevant-paths). The app image is unchanged in that case, so there's nothing to rebuild.\
_Avoid_: latest (Docker Hub's own default tag name, not used here to keep "what does `main` currently look like" unambiguous), main

### Image Promotion

The act of retagging a merged pull request's [`pr-<N>`](#pr-n-tag) image as [the `next` tag](#next-tag), followed by deleting the `pr-<N>` tag. Both steps happen in the same CI job so a promoted image is never left with two live tags.\
_Avoid_: Release, publish (publish refers to the broader act of pushing any tag to Docker Hub, not specifically this merge-time step)

### Docker Hub access token scope

One of three access levels a Docker Hub access token can carry, each stored as its own secret and handed to only the CI jobs that need it:

| Secret                 | Scope             | Used by                                                                   |
|------------------------|-------------------|---------------------------------------------------------------------------|
| `DH_READONLY`          | Read              | Looking up whether a `pr-<N>` image exists                                |
| `DH_READ_WRITE`        | Read/write        | Pushing `pr-<N>`, promoting it to `next`                                  |
| `DH_READ_WRITE_DELETE` | Read/write/delete | Removing a `pr-<N>` tag, whether on pull request close or after promotion |

None of these scopes can update the Docker Hub repository description. That endpoint rejects access tokens outright regardless of scope, which is why the description is synced by hand instead (see [ADR 0005](/decisions/adr-0005-manual-dockerhub-description-sync)).\
_Avoid_: API key, Docker Hub PAT (Docker Hub's own UI calls these "access tokens"; use that term to stay consistent with the product)

## Change detection

### Docker-relevant paths

Paths whose changes require rebuilding and pushing a fresh [`pr-<N>` image](#pr-n-tag): application source, the Docker build context, and dependency/build manifests. Computed by the `detect-relevant-changes` composite action's `docker` output.\
_Avoid_: app-relevant paths

### E2E-relevant paths

Paths whose changes require re-running the E2E suite even when nothing [Docker-relevant](#docker-relevant-paths) changed. That covers the E2E specs, the Playwright config, the E2E composite action, and the CI workflow files that wire the E2E job together. Computed by the `detect-relevant-changes` composite action's `e2e` output, alongside and independently of its `docker` output. The two can both be true for the same pull request. See [ADR 0006](/decisions/adr-0006-reuse-next-tag-for-e2e-only-prs) for what happens when only this filter matches.\
_Avoid_: test-relevant paths

## See also

- [Docker](/domain/docker): index of Docker-related domain glossaries.
- [Decisions](/decisions): architecture decision records, filterable by the `domain-docker-publishing` tag.
