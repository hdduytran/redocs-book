# archify

[[ext:archify]] is a skill that renders diagrams from schema-checked JSON (architecture, workflow, sequence, dataflow, lifecycle) into self-contained HTML with inline SVG. ReDocs does not fork it; [[plugin:archify]] calls archify's own `finalize` gate (validate, deliver, strict check, browser check).

## Adapter contract (`plugins/archify/adapter-archify-finalize.mjs`)

| Step | Details |
|---|---|
| Find archify | `$REDOCS_ARCHIFY` → `book.json` `plugins.archify.path` → `~/.kiro/crew/skills/archify` → `~/.claude/skills/archify` |
| Check `--type` | `architecture`, `workflow`, `sequence`, `dataflow`, `lifecycle` |
| Run | `node bin/archify.mjs finalize <type> candidate.json diagram.html --quality showcase --json` in a scratch folder, 300 s timeout |
| Environment | Maps `REDOCS_CHROME_NO_SANDBOX` to `ARCHIFY_CHROME_NO_SANDBOX` and `REDOCS_CHROME` to `ARCHIFY_CHROME` |
| Result | Keeps `status`, `gates` and up to 10 diagnostics; update notices are only relayed to the caller |

Running in scratch keeps archify's sidecar files (`*.finalize.json`, `*.browser-check.json`…) out of the book repository.

## Lessons from real use

- archify's viewer ships `en` and `zh-CN` only; non-English books keep English diagram labels and explain them in page prose.
- Sequence diagrams: keep messages at least 30px apart, keep participant labels short, omit `meta.viewBox`. The first sequence diagram of the Kiro Crew book needed four repair rounds; both diagrams of this book passed on the first render thanks to those tips.

> TODO(owner): Upgrade archify from 3.0.1 to 3.1.0? ReDocs never updates it on its own.
