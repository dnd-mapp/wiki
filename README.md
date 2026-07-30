# dnd-mapp/wiki

Git storage backend for the D&D Mapp Wiki.js instance.

## Structure

- `pages/` holds the wiki content itself, edited and committed by Wiki.js.
- Everything else in this repo is tooling, not wiki content.

## Publishing

Wiki.js builds its pages from the `live` branch, which is kept in sync with the `pages/` directory on `main`.
