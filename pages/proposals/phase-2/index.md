# Proposal: phase 2

**Status**: draft. **Milestone**: v2.

## Context

Version 0.1.0 (v1) covers phase 1 and was dogfooded on a book about Kiro Crew. The implementation plan lists the phase 2 backlog: presentation/slide mode, PDF export of one chapter, optional internal hosting. The dogfood added two items: interface strings stay English inside non-English books, and entity file names mix singular and plural.

## Proposed decisions

| Item | New component | Source |
|---|---|---|
| Interface strings follow the book `language` (en, vi) | [[mod:ui-i18n]] | dogfood finding |
| Present one chapter as slides | [[mod:presentation]] | plan backlog |
| Export one chapter to PDF | [[mod:pdf-export]], using [[mod:cdp]] | plan backlog |
| Always plural entity file names | change to [[mod:authoring]] | dogfood finding |
| Optional internal hosting | not decided | plan backlog |

> TODO(owner): Priority and timing for each item?

## Impact

- New entities `since: v2`: [[mod:ui-i18n]], [[mod:presentation]], [[mod:pdf-export]].
- Pages with a v2 variant: [CLI and commands](page:services/cli), [Runtime environment](page:network/topology).
- The generated **Changes v1 → v2** page summarises the above.

## Alternatives considered

- **Slides with an external library** (reveal.js): capable, but a large dependency that must be bundled offline. Preferred: turn each `##` into a slide with the theme's own CSS/JS.
- **PDF with a dedicated library**: not needed; headless Chrome is already there for the gate (`Page.printToPDF`).

## Rollout

1. `mod:ui-i18n`: a string table in the theme and shell, picked by `book.json` `language`, defaulting to `en`.
2. `add-entity` always writes plural file names and still reads old singular files.
3. `present`: writes `dist/<m>/present/<chapter>.html`.
4. `export-pdf`: prints the chapter page through CDP `Page.printToPDF`.
5. Every step ships with tests and a green `finalize` on the dogfood books.

> TODO(owner): Review this proposal (draft → review → accepted).
