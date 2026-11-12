# Conversation Backup — Tanger Histoire TOC Offset Fix

Date: 2026-11-12
Repo: https://github.com/ucfzem/tanger
Branch: main
Commit: 53beefb
Previous commit: 9d942a4

## Objective

Fix the table-of-contents click offset in the Tangier history page so section titles are not hidden under the sticky top navigation. The affected links were `#intro` (`مقدمة عامة`) and `#ch1` (`01 عصور ما قبل التاريخ والجذور القديمة`).

Important: these labels belong to `tanger/histoire/index.html`, not to `earth-proofs-pwa`.

## Diagnosis

- The sticky element is the top `<nav>` (`body > nav`), not `header.hero`.
- `header` resolves to the non-sticky hero and would produce a wrong offset.
- The nav height is dynamic: approximately 147 px on mobile Arabic, 140 px on mobile French, 83 px on desktop, and 103 px on 1920 px TV layout.
- Lazy images without intrinsic dimensions caused large layout shifts during long TOC jumps.
- An initial active-link style using border/padding changed the TOC height by 30 px and shifted every section; it was replaced with a layout-neutral pseudo-element.

## Changes in `histoire/index.html`

- Added `--sticky-nav-height` set dynamically from the real sticky nav height.
- Added `scroll-margin-top: calc(var(--sticky-nav-height,0px) + 18px)` for `main[id]`, `section[id]`, `h2[id]`, and `h3[id]`.
- Added smooth TOC click handling with a precise 18 px visual gap below the sticky nav.
- Preserved modifier-click behavior and updated the URL hash with `history.pushState`.
- Added instant hash-position correction on initial load, hashchange, font load, and a short 350 ms fallback.
- Added active TOC highlighting while scrolling, with `aria-current="location"`.
- Added a layout-neutral gold active marker using `.toc-grid a.is-active::before`.
- Added intrinsic `width` and `height` attributes to all 20 lazy-loaded images to reserve layout space and prevent anchor drift.
- Kept the existing language switcher, translations, image set, and page design unchanged.

## Tests

Playwright local and live tests passed:

- Viewports: 320x700, 360x800, 390x844, 1280x900, 1920x1080.
- Languages: Arabic, French, English.
- TOC targets tested: intro, ch1, ch2, ch3, ch4, ch5, ch6, ch7, ch9, ch10, timeline, book.
- Landing offset: target top equals sticky nav height + 18 px, within 1 px.
- Active TOC link matches the clicked/current section.
- Direct hash load (`?lang=ar#ch3`) lands correctly.
- No horizontal overflow at tested widths.
- All 20 lazy images have width/height attributes and correct rendered aspect ratio.
- No page errors or console errors.
- Live GitHub Pages file contains the fix.

## Deployment

- Pushed commit `53beefb` to `ucfzem/tanger` main.
- GitHub Pages deployed successfully.
- Live: https://ucfzem.github.io/tanger/histoire/
- Repo: https://github.com/ucfzem/tanger

Note: `https://tangerdantan.is-a.dev/histoire/` currently redirects to the is-a.dev "available domain" page. The custom domain is not active; use the GitHub Pages URL.

## Rollback

- Previous working commit: `9d942a4`
- Changed file only: `histoire/index.html`
