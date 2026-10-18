# Embedding artifacts

Diagrams, wireframes and other generated visuals are **artifacts**: a spec to edit, an output a plugin renders, and a receipt proving the output matches the spec. The flow lives in `lib/command-embed-artifact.mjs` ([[mod:embed]]).

## Steps of `redocs embed <book> <plugin> <id> --spec <file>`

1. Validate the id (a slug) and find the plugin through `lib/load-plugin-registry.mjs`: built-in plugins under `plugins/`, or plugins the book declares in `book.json` (these need `--allow-book-plugins` because they run code from the book repository).
2. Check that the spec extension matches the plugin's `specExtension`; copy it to `artifacts/<id>/spec.<ext>` (only one `spec.*` is kept).
3. Compute `inputHash = sha256(spec)`.
4. Call `adapter.render({specFile, outputFile, options, book, plugin})` in a scratch folder.
5. On success move the output into `artifacts/<id>/`. On failure **keep the previous output**.
6. Write `receipt.json`. Plugin notices (such as "a newer archify is available") go back to the caller but are not stored, so the receipt stays stable across renders.

## Freshness

[[mod:check]] reports RD007 when a receipt failed, the output is missing, or the current spec hash differs from `inputHash`. `redocs embed <book> --refresh-stale` re-renders every such artifact with the plugin and options recorded in its receipt.

## Built-in plugins

| Plugin | Spec | Notes |
|---|---|---|
| [[plugin:archify]] | `.json` | Runs `archify finalize --quality showcase --json`; a failure reports archify's first diagnostic. See [archify](page:integrations/archify). |
| [[plugin:html]] | `.html` | Must be a complete HTML document with no remote resources and no `fetch()`/XHR. |

## Display

On a page, an HTML artifact is an iframe with an "Open full screen" link; an image artifact is an `<img>`. The copy in `dist/` gets a `postMessage` height script so the frame shrinks to its content, capped by the embed's `height`.
