---
title: Changelog Conventions
description: Keep a Changelog format and consumer-facing scope for changelogs across dnd-mapp repositories
published: true
date: 2026-08-07T00:00:00.000Z
tags: conventions, changelog
editor: markdown
dateCreated: 2026-08-07T00:00:00.000Z
---

These conventions apply to every repository in the `dnd-mapp` GitHub organization that produces a consumer-facing artifact: an application, library, package, custom GitHub Action, CLI, or anything else another party depends on.

## Format

Changelogs follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and live at `CHANGELOG.md` in the repository root.

Use only its six standard change types, and omit any that have no entries for a given version:

- `Added` for new features.
- `Changed` for changes in existing functionality.
- `Deprecated` for soon-to-be-removed features.
- `Removed` for now-removed features.
- `Fixed` for bug fixes.
- `Security` for vulnerability fixes.

Don't invent repo-type-specific categories (e.g. a "Docker" category); file an entry under whichever of the six fits, so every repo's changelog reads the same way regardless of what it produces.

## What counts as a consumer-facing change

Only record changes that affect the consumer of the artifact the repo produces. What that means depends on the repo:

- **Application**: changes to shipped, user-visible behavior.
- **Library or package**: changes to its public API or behavior.
- **CLI**: changes to its commands, flags, or output.
- **Custom GitHub Action**: changes to its inputs, outputs, or behavior.
- **Template repository**: changes to what gets scaffolded into a newly instantiated repo (added/removed/renamed scripts, changed folder structure, changed CI or build setup that's copied into the new repo).

Internal-only changes that never reach that consumer (refactors, a repo's own CI tweaks, dependency bumps with no observable effect) are left out.

## Versioning

Until a repository has an actual release process, all entries accumulate under a single `## [Unreleased]` heading; there's nothing to version yet. Once a repo starts tagging releases, give each release its own `## [x.y.z] - YYYY-MM-DD` heading above `Unreleased`.

Every changelog still states adherence to [Semantic Versioning](https://semver.org/spec/v2.0.0.html) in its header, as a forward commitment even before releases start.

## Links

Reference-style links at the bottom of the file point `[Unreleased]` at the repository's full commit history:

    [Unreleased]: https://github.com/dnd-mapp/<repo>/commits/main

Once a version is released, point its link at that release's GitHub Releases page instead of a compare diff:

    [1.0.0]: https://github.com/dnd-mapp/<repo>/releases/tag/1.0.0

The exact tag-naming format (e.g. whether tags carry a `v` prefix) is decided when a repo's release process is defined.

## Process

Add the entry under `## [Unreleased]` in the same pull request as the change it describes. This is manual, curated by the PR author. There is no CI check enforcing it yet.

## See also

- [Development Conventions](/development-conventions): index of all development conventions.
