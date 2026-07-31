# dnd-mapp/wiki

Git storage backend for the D&D Mapp Wiki.js instance, live at [wiki.dndmapp.nl.eu.org](https://wiki.dndmapp.nl.eu.org).

## Structure

- `pages/` holds the wiki content itself, edited and committed by Wiki.js.
- Everything else in this repo is tooling, not wiki content.

## Publishing

Wiki.js builds its pages from the `live` branch, which is kept in sync with the `pages/` directory on `main` by the `publish-live` GitHub Actions workflow. See [AGENTS.md](AGENTS.md) for details.

## Contributing

Commit, pull request, and page frontmatter conventions are documented in [AGENTS.md](AGENTS.md) and the wiki's own [Development Conventions](https://wiki.dndmapp.nl.eu.org/development-conventions) pages.

## License

[MIT](LICENSE)
