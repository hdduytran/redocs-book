# ADR-001: Own builder instead of Starlight

**Status**: accepted (2026-10-18).

## Context

Output must open directly over `file://` and travel as a zip ([Runtime environment](page:network/topology)). Popular static site generators (Astro Starlight, Docusaurus, MkDocs Material) already solve navigation, search and theming, but their search loads its index with `fetch()`, and some assets use absolute paths. The 2026-10-18 spike confirmed that Chrome blocks `fetch()` of local files under `file://` (CORS, origin `null`).

The owner also asked for an **AI-native** tool, not "a plain Starlight site": a machine-readable model, a gate, and a plugin contract.

## Decision

Write a small Node builder: multi-page HTML, relative links, runtime data through `<script src>`, minimal dependencies (`markdown-it`, `ajv`, `yazl`).

## Consequences

- (+) Full control over the gate: renderers collect diagnostics instead of throwing, so `check` and `build` share one render.
- (+) No large toolchain; the skill is self-contained.
- (−) Navigation, search and theme are ours to maintain; no plugin ecosystem.
- (−) Search is simple substring scoring: enough for a few hundred pages, no advanced ranking.

**Revisit when** several people must write at once or the book needs authenticated hosting. Switching renderers (for example to Starlight) stays cheap because the sources are Markdown plus schema-checked JSON.
