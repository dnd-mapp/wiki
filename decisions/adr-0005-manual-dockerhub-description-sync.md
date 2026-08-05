---
title: Sync the Docker Hub description manually instead of via CI
description: Why template-app-angular dropped CI automation for the Docker Hub repository description
published: true
date: 2026-08-05T14:20:00.000Z
tags: adr, ci, domain-docker, domain-docker-publishing
editor: markdown
dateCreated: 2026-08-05T13:30:00.000Z
---

[`template-app-angular`](https://github.com/dnd-mapp/template-app-angular)'s `sync-description` CI job pushed `.docker/DOCKERHUB.md` to the Docker Hub repository description on every change, authenticated with the same `DH_READ_WRITE` access token used to push images (see the [token-scope table](/domain/docker/publishing) in the Docker Publishing glossary). It failed with `403 Forbidden` on every run.

The cause is a Docker Hub platform limitation, not a misconfiguration: the description-update API endpoint rejects Personal Access Tokens outright, regardless of scope or account role ([docker/hub-feedback#1927](https://github.com/docker/hub-feedback/issues/1927), [#1914](https://github.com/docker/hub-feedback/issues/1914), [#2438](https://github.com/docker/hub-feedback/issues/2438)). The only credential that endpoint accepts is the real Docker Hub account password.

We chose not to store that password as a CI secret. Every other Docker Hub credential in this pipeline is a purpose-scoped access token (see [Docker Publishing](/domain/docker/publishing)'s token-scope table). A full account password would be a large increase in blast radius for one low-frequency, non-critical sync step. Instead, `.docker/DOCKERHUB.md`'s header now documents that changes must be pasted into the Docker Hub UI by hand.

## Status

Accepted.

## See also

- [Docker Publishing](/domain/docker/publishing): the glossary this ADR belongs to.
- [ADR 0004](/decisions/adr-0004-docker-hub-over-ghcr): the decision that introduced the token-scoping model this ADR keeps intact.
