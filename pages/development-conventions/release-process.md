---
title: Release Process
description: The two-phase release workflow and its recovery path, for repositories that tag releases
published: true
date: 2026-08-10T00:00:00.000Z
tags: conventions, github, ci, release
editor: markdown
dateCreated: 2026-08-10T00:00:00.000Z
---

This covers how a `dnd-mapp` repository that has adopted the release workflow cuts a release, once its `Unreleased` changelog is ready. It complements the [Changelog Conventions](/development-conventions/changelogs) page, which covers changelog format rather than release mechanics.

## Trigger

A release is cut by manually running `release-prepare.yml` (`workflow_dispatch`) on `main`, with a `bump-level` input of `auto`, `major`, `minor`, or `patch` (default `auto`). `auto` computes the level from `Unreleased`'s entries: highest-severity heading wins (`Removed` to major, `Added`/`Changed`/`Deprecated` to minor, `Fixed`/`Security` to patch), and a `**BREAKING:**` entry-prefix marker forces major regardless of heading. Any explicit level always overrides the computed one.

There is no automatic or scheduled trigger. Releases happen when someone decides `Unreleased` is ready to ship.

## The two-phase workflow

Release mechanics split across two workflow files, one trigger each:

### Phase 1: `release-prepare.yml`

1. Checks `Unreleased` has entries to release (guards against cutting an empty release).
2. Resolves the bump level and computes the new version with `pnpm version <level> --no-git-tag-version`.
3. Moves `Unreleased`'s entries into a new `## [x.y.z] - YYYY-MM-DD` section in `CHANGELOG.md`.
4. Lands `package.json` and `CHANGELOG.md` in a single verified commit directly on `main`, via the org's GitHub App and GraphQL's `createCommitOnBranch` mutation (not a plain `git push`, since `main` requires verified commits).
5. Creates an annotated git tag (`v<version>`) against that commit, SSH-signed using a dedicated bot account's key, and pushes it. GitHub's Verified badge on a tag is a pure signing-key match, so this is the only way to get a Verified tag (the GitHub API can sign commits but not tag objects).

### Phase 2: `release-publish.yml`

Triggers on the `v<version>` tag push:

1. Extracts that version's changelog section as release notes.
2. Creates the GitHub Release and an Announcements-category GitHub Discussion together, via `gh release create --discussion-category`.
3. Promotes the Docker image already tagged `next` (built and pushed on every merge to `main`) to `latest`, `<version>`, `<major>.<minor>`, and `<major>`, via `docker buildx imagetools create`.

## If something breaks

### `release-prepare.yml` fails after the commit lands but before the tag is pushed

`Unreleased` is already emptied and committed to `main`, with no corresponding tag. Simply re-running `release-prepare.yml` does not self-heal this: its "check `Unreleased` has entries" guard fails immediately, since `Unreleased` is now empty on `main`.

Recovery is manual, but doesn't require handling the SSH signing key by hand: that key is a write-only secret nobody can read back out. Run `release-retry-tag.yml` (`workflow_dispatch`, input: the released version, e.g. `1.4.0`). It searches `main`'s history for the matching `chore(release): <version>` commit (not just `HEAD`, since other commits may have landed on `main` since), confirms the tag doesn't already exist, and reuses `release-prepare.yml`'s SSH-configure -> `git tag -s` -> push steps against that commit.

### `release-publish.yml` fails partway

No dedicated recovery workflow exists for this phase. GitHub Actions' built-in "re-run failed jobs" is sufficient:

- The Docker promotion job depends on the release job's outputs, so a partial re-run doesn't recreate the release or Discussion.
- `docker buildx imagetools create` is naturally idempotent, safe to reapply.
- The GitHub Release and Discussion are created together in one atomic API call, so there's no "release created, Discussion missing" partial state to recover from. If it fails outright, nothing was created, and a re-run is a clean retry.

## See also

- [Development Conventions](/development-conventions): index of all development conventions.
- [Changelog Conventions](/development-conventions/changelogs): the changelog format this workflow reads from and writes to.
- [GitHub Repository Conventions](/development-conventions/github): commit message and CI conventions this workflow follows.
