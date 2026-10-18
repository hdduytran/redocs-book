# The finalize gate

[[mod:finalize]] is the only evidence an agent may use to say "the book is done". `SKILL.md` says so explicitly: done means `finalize` exited 0.

```redocs-embed
{"artifact": "finalize-sequence-v1", "caption": "redocs finalize: check, build, then the browser gate", "height": 720}
```

## Steps

1. **Check**: the [[mod:loader]] reads the book; the [[mod:renderer]] renders every page at every milestone in memory; [[mod:check]] runs RD000–RD012. Any error stops here, before build (`--strict` also fails on warnings).
2. **Build**: [[mod:build]] reuses the same render that was checked, writes `dist/` and computes `distHash`.
3. **Browser gate**: [[mod:cdp]] opens every HTML page (artifacts and assets excluded) over `file://` in headless Chrome with a throwaway profile, collecting console errors, exceptions, failed requests and pages that do not load.
4. **Receipt**: `dist/finalize-receipt.json` records checks, build and browser results.

## Why the browser gate matches a real reader

Chrome runs **without** file-relaxing flags such as `--allow-file-access-from-files`. The spike showed that a blocked `fetch()` under `file://` is exactly the kind of failure the gate must catch, and the checker caught it.

`REDOCS_CHROME_NO_SANDBOX=1` only turns off Chrome's process sandbox (needed inside an already-sandboxed agent shell); it does not change what a page may load.

## Cleanup

Chrome is closed with `Browser.close`, then `SIGTERM`/`SIGKILL` if needed; the temporary profile (`redocs-cdp-*`) is removed in `finally`.
