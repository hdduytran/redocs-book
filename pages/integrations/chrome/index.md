# Headless Chrome

The [[mod:cdp]] (`lib/chrome-cdp-file-protocol-check.mjs`) drives [[ext:chrome]] through the Chrome DevTools Protocol over `--remote-debugging-pipe`: fd 3 takes commands, fd 4 returns events, each JSON message ends with a NUL byte. No port and no WebSocket are needed; the approach comes from archify.

## Finding a browser

`$REDOCS_CHROME`, then `/Applications/Google Chrome.app/…` and `/Applications/Chromium.app/…` on macOS, then `google-chrome`, `chromium`… on `PATH`. If none is found the result is `ERR_REDOCS_NO_CHROME` with a hint.

## Per page

1. `Target.createTarget` + `attachToTarget` (flattened sessions).
2. Enable `Page`, `Runtime`, `Log`, `Network`; set a 1280×900 viewport (optionally dark mode).
3. `Page.navigate` to `file://…`, wait for `loadEventFired` (15 s timeout), then let the page settle.
4. Collect `Runtime.exceptionThrown`, `console.error`, error-level `Log.entryAdded`, and `Network.loadingFailed`.
5. Optionally evaluate a JS expression or capture a screenshot (`scripts/screenshot-pages.mjs` uses this for visual review).

## Sandbox

Inside a Kiro Crew agent shell Chrome cannot initialize its own sandbox (`sandbox initialization failed: Operation not permitted`), hence `REDOCS_CHROME_NO_SANDBOX=1`. Running as root adds `--no-sandbox` automatically.
