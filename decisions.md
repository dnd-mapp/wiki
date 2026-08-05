---
title: Decisions
description: Index of architecture decision records (ADRs) shared across dnd-mapp repositories
published: true
date: 2026-08-05T14:00:00.000Z
tags: adr, index
editor: markdown
dateCreated: 2026-08-05T14:00:00.000Z
---

Architecture decision records (ADRs) for repositories in the `dnd-mapp` GitHub organization, numbered as one global sequence regardless of which domain(s) they belong to. Each ADR is tagged `domain-<slug>` for every domain it touches. A `domain-docker` ADR that's specifically about publishing carries both `domain-docker` and `domain-docker-publishing`, so browsing the parent domain surfaces its subdomains' decisions too. Browse by tag, or use the list below.

- [ADR 0001: Design system lives in this template repo for now, not its own package](/decisions/adr-0001-design-system-in-template-repo-for-now) - `domain-design-system`
- [ADR 0002: Design-system code is a plain `src/` folder, not an Angular library project](/decisions/adr-0002-design-system-plain-folder-not-library) - `domain-design-system`
- [ADR 0003: Design system owns Base Styles and Reset, not each consuming app](/decisions/adr-0003-design-system-owns-base-and-reset) - `domain-design-system`
- [ADR 0004: Publish Docker images to Docker Hub instead of GHCR](/decisions/adr-0004-docker-hub-over-ghcr) - `domain-docker`, `domain-docker-publishing`
- [ADR 0005: Sync the Docker Hub description manually instead of via CI](/decisions/adr-0005-manual-dockerhub-description-sync) - `domain-docker`, `domain-docker-publishing`

## See also

- [Domain](/domain): index of all domain glossaries.
