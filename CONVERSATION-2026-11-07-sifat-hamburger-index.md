# Sifat al-Safwa — hamburger chapter index — 2026-11-07

## Request

- Add a floating hamburger button to open an animated index panel.
- List chapter titles (not the 1025 transmitter cards) to jump to a chapter.
- Remove the book icon from the header if present.

## Inspection findings

- The old `#back-to-index` button targeted `<main id="list">`.
- The header had no emoji 📖, only an inline SVG book logo; that brand logo was left unchanged.
- `#list` contains transmitter cards; chapter titles correspond to the data `category` values.
- The existing design uses Cairo/Amiri, `--card`, `--border`, `--shadow`, `--primary`, and `--accent`.

## Change chosen (scenario A)

The hamburger replaces the old back-to-index button:

- The toggle is `<a href="#list" id="index-toggle">` so no-JS still returns to `#list`.
- With JS, clicking toggles the accordion panel.
- The panel includes an “↑ Retour à l'index” link to `#list`.
- Hamburger icon is pure CSS bars, no emoji.

## Implementation

- Added `#index-toggle` and `#index-panel` before `</body>`.
- Panel opens with transform/opacity transition (`.is-open`), closes by toggle, outside click/touch, or Escape.
- Chapter entries are generated from the loaded `data.json` categories through `window.__sifatEntries`.
- Jump uses the exposed Sifat state/render functions to select the category and scroll to the first matching card.
- Exposed `window.__sifatEntries`, `window.__sifatState`, and `window.__sifatRender` after data load.
- Bumped service worker from `sifat-v9` to `sifat-v10`.

## Tests

Local Playwright on a local HTTP server:

```text
{ open: true, count: 68, first: 'أصحاب رسول الله', href: '#list' }
closed true
{ outsideClosed: true, searchCount: 60 }
```

Live check after push:

```text
LIVE_OK
index-toggle / index-panel present in live index.html
```

## Commit

- `1a3e091` — `feat(sifat): add hamburger chapter index panel`

## Links

- Site: <https://ucfzem.github.io/sifat-al-safwa/>
- Direct list: <https://ucfzem.github.io/sifat-al-safwa/#list>
- GitHub HTML: <https://github.com/ucfzem/ucfzem.github.io/blob/main/sifat-al-safwa/index.html>
- Raw HTML: <https://raw.githubusercontent.com/ucfzem/ucfzem.github.io/main/sifat-al-safwa/index.html>
- Repo folder: <https://github.com/ucfzem/ucfzem.github.io/tree/main/sifat-al-safwa>
- Backup md: <https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-sifat-hamburger-index.md>
