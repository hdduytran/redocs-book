# Components

ReDocs is one skill package (`skill/redocs/`) holding the agent contract, the CLI, the engine and the plugins. The **Catalog** lists every component; this page shows the boundaries.

| Layer | Components | Main files |
|---|---|---|
| Agent contract | [[mod:skill-md]] | `SKILL.md`, `references/*.md`, `templates/book-starter/AGENTS.md` |
| Command interface | [[mod:cli]] | `bin/redocs.mjs` |
| Authoring | [[mod:authoring]], [[mod:promote]] | `lib/command-init-book.mjs`, `lib/command-add-page-entity-variant.mjs`, `lib/command-promote-milestone.mjs` |
| Model | [[mod:loader]] | `lib/load-book-from-directory.mjs`, `lib/validate-book-schemas.mjs`, `schemas/*.schema.json` |
| Rendering | [[mod:renderer]] | `lib/render-book-pages.mjs`, `lib/render-markdown-with-redocs-extensions.mjs`, `lib/render-html-fragment-with-redocs-extensions.mjs`, `lib/render-redocs-references.mjs` |
| Checking | [[mod:check]] | `lib/check-book-rules.mjs`, `lib/command-check-book.mjs` |
| Publishing | [[mod:build]], [[mod:pack]] | `lib/command-build-book-site.mjs`, `lib/build-generated-pages.mjs`, `lib/render-page-shell-layout.mjs`, `lib/command-pack-book-zip.mjs` |
| Final gate | [[mod:finalize]], [[mod:cdp]] | `lib/command-finalize-book.mjs`, `lib/chrome-cdp-file-protocol-check.mjs` |
| Plugins | [[mod:embed]], [[plugin:archify]], [[plugin:html]] | `lib/command-embed-artifact.mjs`, `lib/load-plugin-registry.mjs`, `plugins/*/` |
| Reader UI | (theme) | `assets/theme/redocs.css`, `assets/theme/redocs.js` |

## Key boundaries

- **The loader never throws for content**: broken JSON, schema errors and duplicate ids all become diagnostics. Only a missing `book.json` stops it.
- **Renderers record, they do not judge**: links, entities, embeds and assets are recorded in the render context; [[mod:check]] decides what is an error once every page at every milestone is rendered (anchors need that).
- **Build does not check itself**: it renders and writes; `finalize` combines check, build and the browser.

Commands and flags: [CLI and commands](page:services/cli).
