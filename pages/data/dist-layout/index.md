# dist/ layout

[[mod:build]] deletes and rewrites the whole [[db:dist]] on every build. Every `href`/`src` is relative, so the folder can be moved, zipped or opened from anywhere. Path conventions live in one place: `SITE` in `lib/site-paths-and-urls.mjs`.

```text
dist/
├── index.html                     meta refresh to <current>/index.html
├── finalize-receipt.json          written by finalize
├── _redocs/redocs.css, redocs.js  shared theme
├── assets/…                       copied from the book's assets/
├── artifacts/<id>/<output>        copied artifacts with the resize script injected
└── <milestone>/
    ├── index.html                 home page for the milestone
    ├── search-index.js            window.__REDOCS_SEARCH = […]
    ├── <page-id>.html
    ├── _standalone/<page-id>.html self-contained HTML pages (shown in an iframe)
    ├── catalog/index.html, catalog/<type>.html
    ├── changes/<a>-to-<b>.html
    └── compare/<page-id>/<a>-vs-<b>.html
```

## Generated pages

| Page | Built from | When |
|---|---|---|
| Home | `book.json` | every milestone |
| Catalog | `entities/` | every milestone; alive entities first, the rest under "Not at this milestone" |
| Changes A → B | entity lifecycles + page variants | each adjacent pair, plus current → last when not adjacent |
| Compare A ↔ B | two variants of a page | pages whose variants differ for that pair |

Compare pages iframe the real pages with `?embed=1` (hiding the chrome), so relative links inside stay correct.

## Determinism

No timestamps in the output; displayed dates come from `page.json` `updated`. `distHash` hashes the relative path and content of every file in sorted order, and a test asserts that two builds give the same hash.
