# ReDocs design book

The system design book of [ReDocs](https://github.com/hdduytran/redocs), written with ReDocs
itself: vision, architecture, decisions, key flows, integrations and the phase 2 proposal.

**Read it: [hdduytran.github.io/redocs-book](https://hdduytran.github.io/redocs-book/)**

This repository is a ReDocs book: `book.json`, `pages/`, `entities/`, `artifacts/` and
`assets/` are the sources; `dist/` is built and not committed. Agents working on it follow
[AGENTS.md](AGENTS.md).

## Build locally

```bash
git clone https://github.com/hdduytran/redocs ../redocs
(cd ../redocs/skill/redocs && npm ci)
node ../redocs/skill/redocs/bin/redocs.mjs finalize .
open dist/index.html
```

Pushing to the default branch runs `.github/workflows/publish-book.yml`, which runs the same
`finalize` gate and publishes `dist/` to GitHub Pages.

## Notices

Diagrams under `artifacts/` were rendered by [archify](https://github.com/tt-a1i/archify) (MIT)
and embed its viewer and the JetBrains Mono font (SIL Open Font License 1.1, license text
included in each file).
