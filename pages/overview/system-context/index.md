# System context

The [[app:owner]] gives instructions; the [[app:agent]] reads [[mod:skill-md]] and calls the [[mod:cli]]. The CLI runs the engine ([[mod:loader]], [[mod:renderer]], [[mod:check]], [[mod:build]]) over the [[db:book-repo]] and writes [[db:dist]]. The [[app:reader]] opens `dist/index.html` straight from disk. Diagrams come from [[plugin:archify]] calling the [[ext:archify]] skill; [[ext:chrome]] is the final gate.

```redocs-embed
{"artifact": "system-context-v1", "caption": "ReDocs v1: from the owner's instructions to static HTML a reader opens over file://", "height": 760}
```

What to notice in the diagram:

1. **The main path has no server**: everything is a Node process run on demand plus files on disk.
2. **The book repository is separate from the skill**: the skill can be upgraded without touching books; each system has its own book.
3. **Chrome sits at the end**, opening exactly the files a reader will open, under the same `file://` rules.

## Technology

| Area | Choice |
|---|---|
| Runtime | Node ≥ 18, ES modules |
| Dependencies (pinned exactly) | `markdown-it`, `ajv`, `yazl` |
| Tests | `node:test`, including a real archify render and the browser gate |
| Browser gate | Headless Chrome/Chromium over `--remote-debugging-pipe` |

## Quality attributes

- **Deterministic**: the same sources give the same `distHash`; the zip has fixed order and mtimes.
- **Offline**: no `fetch()`, no remote resources (RD009); runtime data loads through `<script src>`.
- **No lost artifacts**: a failed render keeps the previous output and the receipt records the error.
- **Agent-readable errors**: every problem is a diagnostic with a code, file, JSON pointer or line, and a fix hint.
