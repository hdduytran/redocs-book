# Check codes

[[mod:check]] reports diagnostics as `SEVERITY CODE file pointer|line: message` (or JSON with `--json`), plus a `hint`. The full table with fixes lives in the skill's `references/check-codes.md`; a test keeps it in sync with `RULE_CODES` in the code.

| Code | Severity | Meaning |
|---|---|---|
| RD000 | error | JSON cannot be read or parsed |
| RD001 | error | Schema violation; page id differs from its folder; duplicate entity id |
| RD002 | error/warning | A chapter lists an unknown or duplicate page; a page is in no chapter |
| RD003 | error | Broken `page:` link or `#anchor` |
| RD004 | error | Entity not registered |
| RD005 | error | A variant mentions an entity that does not exist at its milestone |
| RD006 | error/warning | Bad variant milestone or missing source; a stray source the page never renders |
| RD007 | error/warning | Artifact missing, failed or older than its spec; artifact embedded nowhere |
| RD008 | error | Asset does not exist |
| RD009 | error | Remote resource (not offline) |
| RD010 | error | Entity graph: dependencies, replacement, since/until |
| RD011 | warning | Open `TODO(owner)` question |
| RD012 | error | Not exactly one current milestone; duplicate milestone/chapter ids |

## Design of the codes

- Codes are stable so agents and people can look them up across versions.
- A problem seen at several milestones is reported once: a broken link on a shared page does not name a milestone.
- RD011 is a warning on purpose: a book with open questions still builds, but the owner sees what is left. `--strict` turns it into a failure.
- The "stray source" warning (RD006) came from the Kiro Crew dogfood: an `index.md` silently shadowed an `index.html` and a wireframe disappeared without any error.
