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

## Commit

- `c4041f9` — `fix(sifat): deep-link chapter index entries`

## Links

- Site: <https://ucfzem.github.io/sifat-al-safwa/>
- Direct chapter example: <https://ucfzem.github.io/sifat-al-safwa/#cat-%D8%A7%D9%87%D9%84-%D8%A7%D9%84%D9%85%D8%BA%D8%B1%D8%A8>
- GitHub HTML: <https://github.com/ucfzem/ucfzem.github.io/blob/main/sifat-al-safwa/index.html>
- Raw HTML: <https://raw.githubusercontent.com/ucfzem/ucfzem.github.io/main/sifat-al-safwa/index.html>
- Repo folder: <https://github.com/ucfzem/ucfzem.github.io/tree/main/sifat-al-safwa>
- Backup md: <https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-sifat-deep-link-index.md>
