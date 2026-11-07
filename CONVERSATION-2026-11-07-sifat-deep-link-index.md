# Sifat al-Safwa — chapter deep links — 2026-11-07

## Problem

The hamburger index listed 68 chapter categories, but links pointed only to `#list`, so clicking a chapter did not open that chapter.

## Fix

- Added `slugify()` for stable category anchors.
- `render()` now inserts one `.chapter-anchor` with `id="cat-<slug>"` and `data-category="<category>"` before the first rendered card of each category.
- Numeric rendered cards now have `id="entry-<id>"` and `scroll-margin-top` for the sticky header.
- `buildIndex()` now generates links of the form `#cat-<slug>` with `data-cat`, `data-entry`, and `aria-label="Aller au chapitre <category>"`.
- `jumpToChapter()` no longer relies on chips to simulate a filter click. If the anchor is not yet rendered, it renders that exact category once, then scrolls to the category anchor, highlights the first card, closes the panel, and updates the hash with `history.replaceState`.

## Strategy

Strategy A: generated category anchors, with a category-only render fallback for categories beyond the initially rendered page.

Note: categories recur in `data.json` (81 transitions for 68 distinct categories), so anchors are deduplicated within each rendered `#list` pass.

## Test

Local Playwright check on a local HTTP server:

```text
{ hash: '#cat-%D8%A7%D9%87%D9%84-%D8%A7%D9%84%D9%85%D8%BA%D8%B1%D8%A8', top: 72.96875, cardTop: 70.96875, cardCat: 'أهل المغرب', open: false, cat: 'أهل المغرب', cards: 60 }
OK true
```

Raw GitHub code contains the new chapter anchors:

```text
.chapter-anchor
anchor = '<div class="chapter-anchor" id="cat-' + slugify(it.category) + '" data-category="' + esc(it.category) + '"></div>'
```

## Finitions post-c4041f9

Inspection:

- `slugify()` existed in two IIFEs and was semantically identical (whitespace only).
- Rendered cards already have `id="entry-<id>"`.
- The `entry-<id>` fallback is reachable when a category has no rendered `#cat-<slug>` anchor yet, so Fix 2 choice is **A** (keep it).
- The rendered list is semi-grouped, but categories recur later, so anchors are deduplicated per rendered `#list` pass.

Changes:

- Kept one canonical `slugify()` in the app IIFE and exposed it as `window.slugify`.
- Removed the duplicate `slugify()` from the index-panel IIFE.
- Added a comment explaining the `entry-<id>` fallback.
- Bumped service worker to `sifat-v13`.
- Tagged the validated deep-link commit as `v-deeplink-ok`.

Slug non-regression check over the 68 distinct categories:

```text
count 68
before b976d0bcecc5f4dedf70907bf7238ff4ee4c4b1bdcf4203df182d495782ba859
after  b976d0bcecc5f4dedf70907bf7238ff4ee4c4b1bdcf4203df182d495782ba859
equal true
```

Manual checks:

1. Clicking `أهل المغرب` still scrolls to `#cat-…`, URL unchanged, panel closed.
2. Special-character category names still produce stable slugs.
3. Reloading a `#cat-…` URL still lands on the rendered chapter anchor.
4. Search/filters unchanged.
5. Panel open/close unchanged.

## Commits

- `c4041f9` — `fix(sifat): deep-link chapter index entries`
- `da04cf0` — `chore: factorisation slugify + nettoyage fallback jumpToChapter`

## Links

- Site: <https://ucfzem.github.io/sifat-al-safwa/>
- Direct chapter example: <https://ucfzem.github.io/sifat-al-safwa/#cat-%D8%A7%D9%87%D9%84-%D8%A7%D9%84%D9%85%D8%BA%D8%B1%D8%A8>
- GitHub HTML: <https://github.com/ucfzem/ucfzem.github.io/blob/main/sifat-al-safwa/index.html>
- Raw HTML: <https://raw.githubusercontent.com/ucfzem/ucfzem.github.io/main/sifat-al-safwa/index.html>
- Repo folder: <https://github.com/ucfzem/ucfzem.github.io/tree/main/sifat-al-safwa>
- Backup md: <https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-sifat-deep-link-index.md>
