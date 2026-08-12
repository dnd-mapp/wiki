---
title: Publish template-lib-angular to npmjs.org, not GitHub Packages
description: Why the new Angular library template publishes to the public npm registry instead of the GitHub Packages registry ADR-0001 assumed
published: true
date: 2026-08-12T12:00:00.000Z
tags: adr, ci, conventions, domain-design-system
editor: markdown
dateCreated: 2026-08-12T12:00:00.000Z
---

[`template-lib-angular`](https://github.com/dnd-mapp/template-lib-angular), the library counterpart to `template-app-angular`, bootstrapping future Angular library repos in the `dnd-mapp` org, publishes its package to **npmjs.org**, the public npm registry. Not GitHub Packages (`npm.pkg.github.com`), which is what `.npmrc` and the Dockerfile already authenticate `@dnd-mapp`-scoped installs against, and what [ADR 0001](/decisions/adr-0001-design-system-in-template-repo-for-now) assumed the eventual `@dnd-mapp/design-system` package would use too ("the same registry the Dockerfile already authenticates against").

The choice was made deliberately, scoped to `template-lib-angular` specifically:

- **No consumer-side registry auth.** A package on npmjs.org installs with zero `.npmrc` configuration on the consumer's end. A package on GitHub Packages requires every consumer to already have a `.npmrc` pointed at `npm.pkg.github.com` with a token that can read it, fine inside `dnd-mapp` repos that already carry that config, but a real barrier for anything else.
- **Trusted Publishing + staged publishing.** Both are npmjs.org-only features at the time of this decision, with no GitHub Packages equivalent: OIDC-based CI publishing with no long-lived token, plus a 2FA-gated human approval step before a CI-published version becomes installable. See the [npm Trusted Publishing research](https://github.com/dnd-mapp/template-lib-angular/blob/research/npm-trusted-publishing/docs/research/npm-trusted-publishing.md) done for this template's release pipeline.

This narrows, but doesn't reopen, ADR 0001's registry assumption: whichever registry `@dnd-mapp/design-system` eventually publishes to when it's extracted is still that effort's call to make, not decided here. It's worth that effort explicitly revisiting ADR 0001's registry line rather than inheriting it by default, now that a `dnd-mapp` package actually exists on npmjs.org.

## Status

Accepted, for `template-lib-angular`.

## See also

- [ADR 0001](/decisions/adr-0001-design-system-in-template-repo-for-now): the registry assumption this decision narrows.
- [Design System](/domain/design-system): the glossary ADR 0001 belongs to, worth revisiting once design-system extraction is scheduled.
