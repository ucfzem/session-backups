# Conversation backup — Old-device/TV optimization + Arabic typography

Date: 2026-11-12
Status: completed, tested locally, deployed, and verified live.

## Scope
- Optimize Tanger Histoire and the Cosmic Proofs / Earth Proofs PWA for old smartphones and slow Smart TVs.
- Add remote-friendly focus and D-pad navigation.
- Unify the elegant Arabic title font with Arabic subtitles, photo captions, lightbox text, and comparator labels on the Tanger gallery page.
- Preserve French, English, and Spanish typography exactly.

## Commits
### ucfzem/tanger
- `5e36b20` — `histoire: optimize for old phones and Smart TVs`
- `7826aaa` — `gallery: unify Arabic subtitle and caption font`
- Previous baseline before this work: `53beefb`

### ucfzem/earth-proofs-pwa
- `0790121` — `perf: optimize for old devices and Smart TVs`
- Previous baseline before this work: `ad82808`

## Tanger Histoire changes
File: `histoire/index.html`
- Removed `backdrop-filter: blur(20px)` from the sticky navigation; increased its solid background opacity.
- Added early `tv-mode` and `lite-mode` detection in the head.
- Added a TV/low-power CSS mode that:
  - hides the fixed fractal-noise overlay;
  - removes backdrop filters, animations, transitions, shadows, image filters, and fixed background images;
  - keeps scroll-reveal content visible.
- Broadened low-end detection to `deviceMemory <= 2`, `hardwareConcurrency <= 2`, or Save-Data.
- Added visible 3 px gold focus outlines for TV and lite modes.
- Added spatial D-pad navigation with ArrowUp/Down/Left/Right, nearest-element scoring, automatic focus reveal, and sticky-nav-safe scrolling.
- Existing TOC offset, active state, and image dimensions remain intact.

## Tanger gallery / typography changes
File: `index.html`
- Applied the Arabic title stack to Arabic-only (`html[lang="ar"]`) subtitles and captions:
  `'Amiri', 'Geeza Pro', 'Noto Naskh Arabic', 'Traditional Arabic', serif`.
- Updated:
  - hero subtitle;
  - section titles;
  - gallery caption titles;
  - lightbox title and description;
  - before/after comparator labels;
  - lightbox navigation buttons;
  - share title and subtitle.
- Removed letter spacing and uppercase styling from Arabic comparator labels for correct Arabic shaping.
- French, English, and Spanish rules were not changed.
- Regression test compared computed font families against the pre-change baseline for FR/EN/ES: zero differences.

## Cosmic Proofs / Earth Proofs changes
Files: `index.html`, `style.css`, `app.js`, `sw.js`
- Added early `tv-mode`/`lite-mode` detection in the boot script.
- Replaced/removed `backdrop-filter` declarations with solid or high-opacity fallbacks.
- Added `color-mix()` fallbacks for critical backgrounds, overlays, badges, and modal sections.
- Added `content-visibility: auto` and intrinsic sizing to proof cards.
- Replaced per-card image error listeners with one delegated capture listener.
- Added TV/lite CSS mode: no animations/transitions/shadows, no hover transforms, opaque overlays, strong focus outlines.
- Added spatial D-pad navigation for the page, drawer, and modal.
- Drawer on TV focuses the first proof item instead of only the close button.
- Closing a modal restores focus to the originating proof card when available.
- Added explicit modal image dimensions (`1200×675`) to prevent layout shift.
- Bumped app version to `v1.2.0` and service-worker cache to `earth-proofs-shell-v1.5.0`.

## Tests performed
### Tanger Histoire
- Widths 320/360/390/1280/1920.
- Languages AR/FR/EN.
- TV UA 1280×720: `tv-mode` and `lite-mode` present, nav blur `none`, noise overlay `none`, focus outline visible, D-pad focus worked.
- Low-end simulation (1 GB memory, 2 cores): `lite-mode` and `low-end`, no hidden reveal content, no overflow.
- TOC offset under sticky nav: delta ≤ 1 px at all tested widths.
- No horizontal overflow and no application console errors.

### Tanger gallery
- Arabic at 320/390/1280.
- Gallery and lightbox opened successfully.
- Arabic subtitles, caption titles, lightbox title/description, and comparator labels computed as Amiri.
- No horizontal overflow in page or lightbox.
- FR/EN/ES computed fonts matched the pre-change baseline exactly.

### Earth Proofs
- TV UA 1280×720: `tv-mode`/`lite-mode`, no header blur, card `content-visibility: auto`, modal image dimensions.
- Drawer: first item focus, ArrowDown moved to the next item, Enter opened the modal.
- Modal: ArrowLeft/Right still changed proofs, ArrowDown moved focus, Escape restored focus to the card.
- Mobile 320/390 and desktop 1440: 200 cards, search returned 23 water results, clear restored 200, theme and language switches worked, no overflow.
- Offline: service worker controlled the page and all 200 cards loaded offline.

### Live verification
- GitHub Pages HTML/CSS/JS/SW contained the new markers.
- Playwright live smoke tests passed for:
  - Tanger Arabic font and FR typography preservation;
  - Tanger Histoire TV mode, nav blur removal, and focus outline;
  - Earth Proofs TV mode, drawer D-pad, modal focus restore, v1.2.0.

## Live links
- Tanger gallery: https://ucfzem.github.io/tanger/
- Tanger Histoire: https://ucfzem.github.io/tanger/histoire/
- Earth Proofs PWA: https://ucfzem.github.io/earth-proofs-pwa/

## Notes
- `https://tangerdantan.is-a.dev/histoire/` remains inactive; use the GitHub Pages URLs above.
- FR/EN/ES typography intentionally unchanged.
- Earth Proofs service-worker cache version is `earth-proofs-shell-v1.5.0`.
- No secrets are stored in this document.
