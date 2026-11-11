# Tanger d'Antan — Additive Upgrade (2026-11-11)

## Directive
User asked to upgrade the live Tanger d'Antan site while **keeping the existing dark-cinematic theme and colors** ("Keep Tanger d'Antan theme and colors"), then test, deploy, back up the conversation, and share links.

## Target / deployment model
- Live: https://ucfzem.github.io/tanger/ (also served via https://ucfzem.azer-tyu199p.workers.dev/tanger/)
- The Cloudflare worker `ucfzem.azer-tyu199p.workers.dev` **proxies** `PROXY_TARGET = https://ucfzem.github.io`; `/tanger/` is in `PUBLIC_PATHS` (password `UcfZem@2026`). No Cloudflare token needed for this deploy.
- Source repo: `ucfzem/tanger` (branch `main`). Repo is heavy (images/audio), so a full clone is unreliable; deployed with the **GitHub Contents API**.
- Previous live commit: `1b48b12689f5e07a7bc9093d5d7343abeb0b285c`, `index.html` blob `c88421028e59ff04260bb049dbd8a42e938232e7`, 94,750 bytes, sha256 `819ab056813ec6a8b128c918d64bed5c28b93121669eaad9d19d7a2883473014`.
- New live commit: `43696e74f5926d54ee956d242ad2a7d67db95ac1`, blob `6bab46d201ca1912760568354b94157f59e6d102`, 104,716 bytes, sha256 `26b9349ee999a3fef8196fc5ebcfba6692d154a039dd48bc265b5f99b2c73438`.

## Audit findings (before)
The site was already multilingual (FR/EN/ES/AR, `data-i18n`, RTL, `?lang`, `tanger_lang`, 60 photos ×4 languages, 14 MP3, lightbox before/after, `?edit`/`?export`). Gaps addressed:
- No localized `document.title`; no `data-i18n-aria`.
- Emoji flags; no full language names; no `aria-pressed`.
- Hardcoded "Avant / Après" badge (not translated).
- No `content-visibility`, no low-end detection, no `prefers-reduced-motion`, no IntersectionObserver fallback, no TV/remote keyboard support.
- Slider used physical `left`/`clip-path` (wrong in RTL).
- No skip link. Images lacked `decoding="async"`.

## Changes (additive only — palette/fonts untouched)
- `lang` persistence: reads `tangier-lang` then legacy `tanger_lang`, writes both, wrapped in try/catch.
- Localized `document.title` via `TITLES` map (fr/en/es/ar) + `data-lang` on `<html>`.
- CSS-drawn flags + code + full language name; `aria-pressed` kept in sync.
- `data-i18n-aria` support for play/pause/prev/next and the comparison slider; play/pause aria follows state.
- "Avant / Après" badge translated (re-rendered on language change).
- Accessibility: skip link to `#galerie`, focus-visible rings (larger on TV).
- Performance: `.gallery-item { content-visibility:auto; contain-intrinsic-size:auto 340px }`, `loading="lazy" decoding="async"`, IO fallback shows items immediately when unsupported.
- Low-end: `.low-end` class when `deviceMemory < 2` or `hardwareConcurrency < 3` (flat effects, no filters/backdrop).
- Reduced motion: `prefers-reduced-motion` disables animations/transitions.
- TV: `min-width:1920px` larger lang flags/buttons/focus; Home/End/Backspace scroll shortcuts (ignored while typing).
- RTL: slider clip-path/labels mirrored when `dir="rtl"`.
- Rollback breadcrumb added at top of `index.html`.

## Verification performed
- `node --check` equivalent (vm.Script) on all inline scripts: pass.
- JSON-LD parses.
- i18n: 30 `data-i18n`/`data-i18n-aria` keys present in all 4 languages; full key parity.
- 10 original theme tokens byte-identical; 14 music refs unchanged; 60 gallery items render.
- No duplicate DOM ids.
- jsdom runtime smoke tests: language switch/title/aria/badge, legacy key restore, RTL slider math (2%→98%), play aria, Home/End, IO fallback, low-end detection — all pass.
- HTTP serve: 200, served bytes sha256 identical to file.
- Live verification: both `ucfzem.github.io/tanger/` and the worker URL returned 104,716 bytes / sha256 `26b9349e…` (matches deployed file).

## Rollback
See `ROLLBACK.md` in this repo. One-liner: restore blob `c88421028e59ff04260bb049dbd8a42e938232e7` (commit `1b48b12`) of `index.html` via `git checkout 1b48b12 -- index.html` or Contents API PUT.

## Links
- Live (Pages): https://ucfzem.github.io/tanger/
- Live (worker/proxy): https://ucfzem.azer-tyu199p.workers.dev/tanger/
- Language deep links: `?lang=fr|en|es|ar`
- Repo: https://github.com/ucfzem/tanger
- Deploy commit: https://github.com/ucfzem/tanger/commit/43696e74f5926d54ee956d242ad2a7d67db95ac1
- Backups: https://github.com/ucfzem/session-backups
