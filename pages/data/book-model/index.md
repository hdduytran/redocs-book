# Book model

The [[db:book-repo]] is the single source of truth. An agent in a later session only needs these files to understand the book and continue consistently. Schemas live in the skill's `schemas/` and are checked by the [[mod:loader]] with ajv (JSON Schema 2020-12).

```text
<book>/
├── AGENTS.md            rules for agents working on this book
├── book.json            title, language, milestones, chapters, plugin settings
├── entities/*.json      one array of entities per file
├── pages/<chapter>/<page>/page.json + index.md | index.html | <milestone>.md|html
├── artifacts/<id>/      spec.*, rendered output, receipt.json
└── assets/              screenshots/, wireframes/…
```

## Five schemas

| Schema | Checks | Key points |
|---|---|---|
| `book.schema.json` | `book.json` | `schemaVersion: 1`; milestone `status` ∈ superseded/current/planned/target |
| `page.schema.json` | `page.json` | `id` equals the folder path; `variants` maps milestone → file; `standalone` for self-contained HTML |
| `entity-file.schema.json` | `entities/*.json` | ids look like `<prefix>:<slug>`; `since`, `until`, `replaced_by`, `depends_on` |
| `artifact-receipt.schema.json` | `receipt.json` | `inputHash` is the `sha256:` of the spec at render time |
| `plugin.schema.json` | `plugin.json` | `adapter` is a module exporting `render()` |

Unknown fields are rejected, except `x-*` keys, so a typo never slips through silently.

## Entity lifecycle

An entity is alive at milestone *m* when `since ≤ m < until`, in the milestone order of `book.json` (`isAlive` in `lib/milestone-order-and-lifecycle.mjs`). No `since` means "from the first milestone"; no `until` means "never removed"; `replaced_by` requires `until` (RD010).

## Reference syntax in pages

| Syntax | Markdown | HTML fragment |
|---|---|---|
| Page link | `[x](page:chapter/page#anchor)` | `href="page:…"` |
| Entity | `[[svc:billing]]`, `[[svc:billing\|label]]` | as in Markdown, inside text |
| Asset | `![x](asset:screenshots/a.png)` | `src="asset:…"` |
| Artifact | a `redocs-embed` fence holding JSON | `<redocs-embed artifact="…">` |

The output is described in [dist/ layout](page:data/dist-layout).
