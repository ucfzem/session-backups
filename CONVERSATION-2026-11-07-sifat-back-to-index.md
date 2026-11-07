# Sifat al-Safwa — back to index button — 2026-11-07

## Request

- Returning to the index on `ucfzem.github.io/sifat-al-safwa/` was too difficult.
- Add a floating return-to-index button.

## Fix

- Added the provided self-contained block just before `</body>` in `sifat-al-safwa/index.html`:
  - CSS for `#back-to-index`.
  - `<a href="#top" id="back-to-index">`.
  - Script that detects `#controls` (and other summary anchors), shows the button after 300 px of scrolling, and respects reduced-motion preferences.
- Bumped service worker cache from `sifat-v6` to `sifat-v7` so installed PWAs receive the new shell.

## Test

Local Playwright check on a local HTTP server:

```text
visible true href #controls
```

Live page check after push:

```text
LIVE_OK
back-to-index present in live index.html
```

## Refix: target actually lands on `#controls`

The first version used a plain anchor jump to `#controls`. Because `#controls` is sticky under the site header, the button could appear to land on the previous position instead of the search/index controls.

Revised behavior:

- `#controls` now has `scroll-margin-top: var(--head-h, 73px)`.
- The button click is handled explicitly: it computes `targetTop + scrollY - headerHeight` and smooth-scrolls there, then updates the hash to `#controls`.
- Service worker bumped from `sifat-v7` to `sifat-v8`.

Local Playwright check after scrolling down and clicking the button:

```text
{ top: 73, scrollY: 800, hash: '#controls' }
OK true
```

## Commits

- `1fb5006` — `fix(sifat): add floating back-to-index button`
- `166fb3a` — `fix(sifat): scroll back-to-index to controls below sticky header`

## Links

- Site: <https://ucfzem.github.io/sifat-al-safwa/>
- Repo folder: <https://github.com/ucfzem/ucfzem.github.io/tree/main/sifat-al-safwa>
- GitHub HTML: <https://github.com/ucfzem/ucfzem.github.io/blob/main/sifat-al-safwa/index.html>
- Raw HTML: <https://raw.githubusercontent.com/ucfzem/ucfzem.github.io/main/sifat-al-safwa/index.html>
- Backup md: <https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-sifat-back-to-index.md>
