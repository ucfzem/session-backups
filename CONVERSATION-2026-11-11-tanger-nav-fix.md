# Tanger d'Antan — Nav Regression Fix (2026-11-11)

Supersedes the deploy info in `CONVERSATION-2026-11-11-tanger-upgrade.md` (that upgrade shipped a nav-width regression and was rolled back first).

## What happened
The first additive upgrade replaced the four text language buttons with flag + full-name spans. That widened each button ~2×, straining the nav at 560–768px. User review flagged the nav.

Two parts of the review were incorrect and are recorded here for accuracy:
- The `AR` button was **not** added by the upgrade; the original live nav already had `FR EN ES AR` (pre-upgrade `index.html` lines 951–954).
- Whole-page RTL was **not** introduced by the upgrade; `document.documentElement.dir = lang === 'ar' ? 'rtl' : 'ltr';` and `tanger_lang` persistence already existed (lines 1180, 1222, 1228).

The only real regression was button width (flags + full names).

## Timeline
1. Rollback deployed: restored blob `c884210…` → commit `0df94114af022250b7367dd32b79f0b6becf67ad`. Verified live: 94,750 bytes, sha256 `819ab056…`.
2. Minimal fix prepared: text-only `FR EN ES AR` buttons; removed `.flag-*`, `.lang-name`, inline-flex/gap, 560px name rule, and TV `.lang-btn` enlargement. Nav/lang CSS is **byte-identical to the original** (lines 219–307). Kept invisible a11y attributes (`aria-pressed`, `role="group"`).
3. Fix deployed: commit `2b5eb2f927d2070da06feb74011b3b77d5588a3f`, blob `10956f77b6f1789aec3cd461a5c5fa0958334376`, 103,070 bytes, sha256 `8e8dd82dbbd2a06e258e3279bf8ad8d2a827e74b8431c0d380f1d8ac8b8fc425`.

## Kept from the upgrade (no layout impact)
- Localized `document.title` (fr/en/es/ar), `data-i18n-aria`.
- Language persistence reads `tangier-lang` then legacy `tanger_lang`; writes both, try/catch.
- Skip link, focus-visible rings.
- `content-visibility`, `loading="lazy" decoding="async"`, IO fallback.
- Low-end device class, `prefers-reduced-motion`.
- TV/remote Home/End/Backspace keys.
- RTL before/after slider mirroring; translated "Avant / Après" badge.

## Verification
- Inline scripts syntax-checked; JSON-LD parses.
- 30 i18n keys × 4 languages, full parity.
- 10 original theme tokens unchanged; 14 music refs unchanged; 60 gallery items; no duplicate ids.
- jsdom runtime suite: language switch/title/aria/badge, legacy key restore, RTL slider 2%→98%, play aria, Home/End, IO fallback, low-end — all pass.
- Nav/lang CSS `cmp` against original: identical (lines 219–307).
- Live check: both URLs returned 103,070 bytes / sha256 `8e8dd82d…`; live markup has 4 text-only buttons and **0** `.flag` / `.lang-name` nodes.

## Links
- Live (worker): https://ucfzem.azer-tyu199p.workers.dev/tanger/
- Live (Pages): https://ucfzem.github.io/tanger/
- Language links: `?lang=fr|en|es|ar`
- Fix commit: https://github.com/ucfzem/tanger/commit/2b5eb2f927d2070da06feb74011b3b77d5588a3f
- Rollback commit: https://github.com/ucfzem/tanger/commit/0df94114af022250b7367dd32b79f0b6becf67ad
- Rollback doc: https://github.com/ucfzem/session-backups/blob/main/ROLLBACK.md
- Repo: https://github.com/ucfzem/tanger
