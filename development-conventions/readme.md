---
title: README Conventions
description: Best practices for writing a README.md, backed by GitHub's own documentation and the Standard Readme spec
published: true
date: 2026-07-31T12:00:00.000Z
tags: conventions, github, readme, documentation
editor: markdown
dateCreated: 2026-07-31T12:00:00.000Z
---

This guide covers how to write a good `README.md` for any `dnd-mapp` repository. It complements the [GitHub Repository Conventions](/development-conventions/github) page.

## Naming and placement

- Name the file `README.md` and write it in Markdown. GitHub's docs describe a README as covering "what your project does, why your project is useful, how users can get started, where users can get help, and who maintains the project."
- GitHub looks for a README in three locations, in this order: the `.github` directory, the repository root, or the `docs` directory. Put it at the repo root unless there's a specific reason not to.
- Whichever README GitHub finds is rendered automatically on the repository's home page, no configuration needed.
- Content beyond 500 KiB is truncated when rendered on GitHub, so keep it concise and link out to dedicated docs (or this wiki) for anything longer.

## Structure

[Make a README](https://www.makeareadme.com/) and the [Standard Readme spec](https://github.com/RichardLitt/standard-readme) largely agree on the same shape. A minimal README covers:

- **Title and short description**: what the project is, in one line.
- **Table of contents**: Standard Readme requires one unless the README is under 100 lines.
- **Installation**: how to get it running, including dependencies.
- **Usage**: example commands or code, with expected output where practical.
- **Contributing**: how others can help, or a link to a separate CONTRIBUTING doc.
- **License**: Standard Readme requires this to be the last section.

Add optional sections when they earn their place: badges, screenshots or GIFs, a support/contact section, a roadmap, and an authors/acknowledgments list.

## Optional sections by repo type

Which optional sections earn their place depends on what the repo actually ships. A few common cases:

- **Containerized app (Docker)**: how to pull or build the image, run it, and any `docker-compose` usage, plus the ports, volumes, and environment variables it exposes. Docker Hub's own guidance treats the repository "overview" as the image's real documentation: it syncs from the repo's `README.md` on automated builds, so write it accordingly and link out to Docker Hub or GHCR once the image is published.
- **npm package or library**: the install command (`npm install <package>`), a badges row (version, downloads, build status, via Shields.io), and API/usage examples. npm requires `README.md` at the package root and renders it as-is on the package's registry page, so keep it accurate rather than treating it as a separate doc. Link out to generated API references (TypeDoc, etc.) and list peer dependencies.
- **Custom GitHub Action**: an inputs/outputs table matching `action.yml`'s metadata format, a workflow snippet showing how to reference the action with `uses:`, and a badge if it's listed on the Marketplace. GitHub recommends tagging releases with semantic version tags (e.g. `v1.1.3`) and keeping a rolling major-version tag (`v1`) pointed at the latest compatible release, so document the intended pin, e.g. `uses: owner/action@v1`.
- **CLI tool**: installation across whichever package managers you publish to, sample usage or `--help` output, and shell completion setup if the tool provides it.
- **Web app or backend service**: a live demo link or deployment instructions, screenshots for UI-heavy projects, and for APIs a link to the OpenAPI/reference docs, required environment variables, and auth setup.

## Formatting

- Use standard Markdown headings (`#` through `######`). GitHub auto-generates a table of contents/outline from them, so a manual one is often unnecessary for shorter READMEs.
- Use fenced code blocks with a language identifier (` ```bash `, ` ```yaml `, etc.) for syntax highlighting.
- Link to other files in the repo, and embed images, with relative paths rather than absolute URLs. GitHub rewrites relative links and image paths to match whatever branch is being viewed, so they keep working after a rename, a fork, or a branch switch.

## See also

- [Development Conventions](/development-conventions): index of all development conventions.
- [GitHub Repository Conventions](/development-conventions/github): commit message and CI conventions.

## Sources

- [About READMEs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)
- [Basic writing and formatting syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [Standard Readme Specification](https://github.com/RichardLitt/standard-readme)
- [Make a README](https://www.makeareadme.com/)
- [Repository information](https://docs.docker.com/docker-hub/repos/manage/information/)
- [About package README files](https://docs.npmjs.com/about-package-readme-files)
- [Creating a JavaScript action](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action)
- [Metadata syntax reference](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)
- [Releasing and maintaining actions](https://docs.github.com/en/actions/sharing-automations/creating-actions/releasing-and-maintaining-actions)
- [Shields.io](https://shields.io/)
- [Making a PyPI-friendly README](https://packaging.python.org/en/latest/guides/making-a-pypi-friendly-readme/)
