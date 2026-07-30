---
title: GitHub Repository Conventions
description: Commit message and CI conventions shared across all dnd-mapp repositories
published: true
date: 2026-07-30T12:00:00.000Z
tags: conventions, github, ci, commits
editor: markdown
dateCreated: 2026-07-30T12:00:00.000Z
---

These conventions apply to every repository in the `dnd-mapp` GitHub organization.

## Commit conventions

- Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/).
- Group changes into separate commits by intent rather than one large commit.

## CI conventions

- Pin GitHub Actions to a full commit SHA, with the version as a trailing comment, instead of a floating tag:

  ```yaml
  uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
  ```

  Floating tags can be moved to point at a different commit after review. Pinning to an SHA guarantees the workflow always runs the exact code that was audited.
