# Milestone and variant resolution

All rules live in `lib/milestone-order-and-lifecycle.mjs`, shared by the [[mod:renderer]] and [[mod:build]].

## Picking a page body at milestone m (`resolveVariant`)

| Page | Shows at m |
|---|---|
| No `variants` | the shared `entry` file (default `index.md`, then `index.html`) |
| Has a variant for m | that file |
| None for m, one for an earlier milestone | the nearest earlier variant, with an "inherited from …" notice |
| None for m or any earlier milestone | a "does not exist at this milestone yet" page |

## Checking entities per milestone

- **Milestone-specific variants** are checked strictly: every `[[entity]]` must be alive at that milestone, otherwise RD005.
- **Shared pages** (vision, proposals) may mention future or removed entities; their chips show a "from v2" or "removed in …" badge instead of failing.

Example: [CLI and commands](page:services/cli) has a v2 variant that mentions [[mod:presentation]] (since v2); the v1 variant may not.

## Comparison pairs (`comparisonPairs`)

Every adjacent pair of milestones, plus current → last milestone when they are not adjacent. This book (v1, v2) has a single pair, v1 → v2.

## Promote

`redocs promote <book> v2` marks earlier milestones `superseded`, makes v2 `current`, and prints the pages and entities the owner should confirm. It never rewrites content: what really changed is the owner's call. Milestones only move forward.
