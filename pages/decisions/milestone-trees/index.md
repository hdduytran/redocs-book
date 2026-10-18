# ADR-002: One page tree per milestone

**Status**: accepted (2026-10-18).

## Context

Readers need to see the book at each milestone (v1, v2, target) and switch between them. Two options:

1. One page tree; JavaScript swaps content for the selected milestone (kept in `localStorage`).
2. Build each milestone into a complete tree `dist/<milestone>/…`.

## Decision

Option 2. Pages without variants are rendered again in every tree; pages with variants render the body of that milestone.

## Consequences

- (+) The URL states the milestone; no JavaScript is needed to read the right content; every link is a real file `check` can verify.
- (+) The milestone switcher is just a relative link to the same page in another tree.
- (−) File count grows with pages × milestones. The Kiro Crew dogfood book: 16 pages × 2 milestones → 50 HTML pages, a 59-file zip.
- (−) One search index per milestone.

Details: [Milestone and variant resolution](page:flows/milestones).
