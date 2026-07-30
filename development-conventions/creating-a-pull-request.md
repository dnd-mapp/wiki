---
title: Creating a Pull Request
description: Guide for agents and humans on writing and submitting pull requests across dnd-mapp repositories
published: true
date: 2026-07-30T18:00:00.000Z
tags: conventions, github, pull-requests
editor: markdown
dateCreated: 2026-07-30T18:00:00.000Z
---

This guide covers how to open a good pull request in any `dnd-mapp` repository, for humans and agents alike. It complements the [GitHub Repository Conventions](/development-conventions/github) page, which covers commit message and CI conventions.

## Use the org-wide template

Every `dnd-mapp` repository without its own `PULL_REQUEST_TEMPLATE.md` inherits the default template from [dnd-mapp/.github](https://github.com/dnd-mapp/.github/blob/main/.github/PULL_REQUEST_TEMPLATE.md). GitHub loads it into the PR body automatically when a new pull request is opened. Fill in every section rather than deleting it.

## Keep it small and scoped

Prefer one self-contained change per PR. Google's engineering practices guide treats about 100 lines as a reasonable size and about 1000 lines as usually too large, though it's a judgment call. Breadth across files matters as much as line count. Small PRs review faster, get reviewed more thoroughly, and are easier to revert or rebuild if the approach turns out wrong.

## Write a clear title and description

- **Title**: a complete sentence in the imperative mood, for example "Fix the off-by-one in tile rendering" rather than "Fixed a bug." Follow [Conventional Commits](https://www.conventionalcommits.org/) formatting (`type(scope): description`), since a PR title typically becomes the squash-merge commit message. See [commit conventions](/development-conventions/github#commit-conventions).
- **Description**: explain what changed and why, not just what. Cover the problem being solved, why this approach was chosen, and any known limitations. Future readers search history by description, so give them enough context to find and understand the change later.

## Link related issues

Use a closing keyword in the PR description, such as `Closes #123`, `Fixes #123`, or `Resolves #123`. Only description-level keywords register as a linked issue in the GitHub UI; the same keyword in a commit message does not. The PR must target the repository's default branch for the keyword to take effect. List multiple issues individually, for example `Resolves #10, resolves #123`.

## Use draft PRs for work in progress

Open a draft pull request to share in-progress work without requesting review. Draft PRs can't be merged and don't automatically request code owners until marked ready for review. You can convert between draft and ready for review at any time.

## What reviewers look for

Reviewers approve once a change clearly improves the codebase's overall health, not once it's "perfect." Expect feedback on design, functionality, complexity, tests, naming, comments, and consistency with the style guide, and expect non-blocking nits to be marked as such.

## Before merging

Required CI checks must pass on the latest commit. A check that passed on an earlier commit does not carry over once new commits are pushed.

## See also

- [Development Conventions](/development-conventions): index of all development conventions.

## Sources

- [Creating a pull request template for your repository](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)
- [Small CLs](https://google.github.io/eng-practices/review/developer/small-cls.html)
- [CL Descriptions](https://google.github.io/eng-practices/review/developer/cl-descriptions.html)
- [Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
- [About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [What to Look For In a Code Review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)
- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
