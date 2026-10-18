# Why ReDocs exists

ReDocs is a skill that lets **an AI agent write and maintain a system design book** on the instructions of the system's owner. The book covers the system's whole life: the original idea, the current state, proposed changes to present or hand to developers, and the long-term target.

## The problem

Documentation for a large system (many microservices, data flows, integrations, networks, infrastructure) usually falls into one of two traps:

- **Written by hand**: expensive, so it goes stale; every author writes differently.
- **Written freely by AI**: fast, but it invents things, drifts between sessions, and nobody can tell whether the book is still consistent.

ReDocs makes the AI the main author but constrains it with a **machine-readable model** and a **deterministic gate**.

## Founding decisions (owner session, 2026-10-18)

| Question | Decision |
|---|---|
| Who writes, who reads? | The AI writes on a person's instructions; the book is mainly for people. |
| Where do docs live? | One book repository per system, never inside a code repository. |
| Automatic updates? | No. The book changes when the author asks, and it may describe a system before it exists. |
| Formats? | Markdown for simple parts, HTML where layout matters, embedded artifacts from AI tools such as archify. |
| How is it read? | Opened straight from disk over `file://`; shareable as a zip. |
| Versions? | Global milestones (v1, v2, target) are system states; Git keeps edit history. |

## What "AI native" means here

1. The agent is the authoring interface; people do not hand-edit HTML.
2. A machine-readable source of truth: the [book model](page:data/book-model).
3. A deterministic gate: work is done only when [finalize](page:flows/finalize) passes.
4. One plugin contract for content tools, archify first ([Embedding artifacts](page:flows/embed-artifact)).

## Non-goals (phase 1)

Multi-author collaboration, a server, authentication, syncing from code, a chatbot inside the book, generating diagrams from entities, WYSIWYG editing.

Next: [System context](page:overview/system-context).
