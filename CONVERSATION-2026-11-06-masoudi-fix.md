# Masoudi Encyclopedia index fix — 2026-11-06

## User request

- Fix `masoudi-encyclopedia`: index entries must scroll to the exact chapter heading, not to the start of the previous page/chapter.
- Verify that titles and content belong to the correct book (`akhbar` / `muruj`), avoid duplicate/conflicting anchors, and keep both books structurally separate.
- Test the fix, push/deploy to GitHub Pages, save a conversation backup, and share the links.

## Root cause

The Akhbar source is page-based. Several pages begin with the tail of the previous chapter, and the real chapter heading appears later in the page. The old index linked to the section start (`sec-N`), so entries such as `ذكر عمر الدنيا` landed on the previous chapter's text.

`ذكر عمر الدنيا` (p31) is correctly classified as **Akhbar**, not Muruj. The Shamela page begins with the previous chapter tail and then contains `[ذكر عمر الدنيا]`.

Muruj is already chapter-based in the archive source. A few obvious OCR/TOC fragments were present and are now hidden from the index.

## Fix

- Rebuilt Akhbar from the **41 chapter titles** of the Shamela table of contents.
- Split content on the exact bracketed chapter markers; the tail before each marker stays with the previous chapter.
- Kept Muruj source chapter boundaries, with stable numeric suffixes.
- Added stable, globally unique IDs:
  - `akhbar-chap-1` … `akhbar-chap-41`
  - `muruj-chap-1` … `muruj-chap-94`
- Added `books.akhbar.chapters` and `books.muruj.chapters`; visible Muruj chapters are 88 because 6 obvious OCR/TOC fragments are marked `hidden`.
- Frontend now renders each chapter title as `<h2 id="akhbar-chap-N">` / `<h2 id="muruj-chap-N">`.
- Index entries use `data-target` and `scrollIntoView({behavior:"smooth", block:"start"})`.
- Added `scroll-margin-top:96px` for all chapter anchors.
- Index is grouped by book and filtered by the existing work buttons.
- Previous/next, active-index tracking, copy/share, and last-position restore now use IDs instead of flat array indices.
- Bumped the service worker cache from `masoudi-v3` to `masoudi-v4` so installed PWA users receive the new shell and data.

## Follow-up fix: robust hash navigation (v5)

The first deploy fixed the data, but direct `#akhbar-chap-5` links and stale PWA caches could still open at the wrong place. The second pass adds:

- `robustJumpToSection(sectionTitle, pageNum, sectionId)` with asynchronous yielding, title lookup, and a normalized DOM fallback.
- Tashkeel/alef/ya/ta-marbuta normalization via the existing `norm()` / new `normTitle()`.
- Dynamic header offset (`headerOffsetPx()`), CSS `scroll-margin-top:var(--anchor-offset,96px)`.
- `afterPaint()` (rAF plus a 140 ms fallback) so scrolling still happens after a heavy render.
- Direct hash support on boot and on `hashchange`; index clicks update the URL hash.
- Gold flash highlight on the target heading.
- Index items now expose `data-page` and `data-title`; headings expose `data-title`.
- `APP_VERSION = "v5"`, `data.json?v=v5`, service-worker update check, and `controllerchange` reload.
- Service worker `masoudi-v5`: network-first for navigations and `data.json`, cache fallback offline; cache-first for other shell assets.

## Verification

Local data validation:

```text
PASS: 135 sections, 41 akhbar, 88 muruj, 6 hidden
PASS: unique ids, book grouping, titles, content, HTML anchors
```

DOM/click test with jsdom:

```text
index items: 129 groups: أخبار الزمان | مروج الذهب
all index targets resolve; ids unique: 278
akhbar-chap-5 click -> akhbar-chap-5
first: فأما ما ذكروه من توقيت الزمان ومدته الى انقضائه...
muruj-chap-1 heading: مروج الذهب · ذكر جوامع أغراض هذا الكتاب · ج 1
akhbar: 41 muruj: 88
search "تنيس" cards: 3 count: 11 نتيجة
PASS
```

Robust navigation tests (local jsdom):

```text
PASS click + hashchange + version
PASS direct hash load
PASS search-clear navigation
```

Live verification after deployment:

```text
live data keys: ['meta', 'books', 'sections']
sections: 135
books: {'akhbar': 41, 'muruj': 88}
akhbar-chap-5: ذكر عمر الدنيا | فأما ما ذكروه...
sw version v4: True
```

## Commits

- Fix: `eafe563` — `fix(masoudi): anchor index to exact chapter headings`
- Cleanup: `ca52554` — `chore(masoudi): ignore Python cache`
- Robust nav + v5 cache: `7022b7a` — `fix(masoudi): robust hash navigation and v5 cache`
- Initial commit URL: <https://github.com/ucfzem/ucfzem.github.io/commit/ca525541e3205a32eb3129c67cca8090c2ee8635>
- Robust nav commit URL: <https://github.com/ucfzem/ucfzem.github.io/commit/7022b7a>

## Live links

- Site: <https://ucfzem.github.io/masoudi-encyclopedia/>
- Data: <https://ucfzem.github.io/masoudi-encyclopedia/data.json>
- Direct example: <https://ucfzem.github.io/masoudi-encyclopedia/#akhbar-chap-5>
- Another direct example: <https://ucfzem.github.io/masoudi-encyclopedia/#muruj-chap-1>
- Repository: <https://github.com/ucfzem/ucfzem.github.io/tree/main/masoudi-encyclopedia>
- Tools:
  - `masoudi-encyclopedia/tools/rebuild_data.py`
  - `masoudi-encyclopedia/tools/validate_index.py`

## Changed files

- `masoudi-encyclopedia/data.json`
- `masoudi-encyclopedia/index.html`
- `masoudi-encyclopedia/sw.js`
- `masoudi-encyclopedia/tools/rebuild_data.py`
- `masoudi-encyclopedia/tools/validate_index.py`
- `masoudi-encyclopedia/tools/.gitignore`

Second pass changed files:

- `masoudi-encyclopedia/index.html`
- `masoudi-encyclopedia/sw.js`
- `masoudi-encyclopedia/tools/validate_index.py`

## Security note

A GitHub token was pasted into the chat earlier in this session and used for the first deployment. It is redacted here and **must be revoked immediately**. The temporary `/tmp/gh_token` file was deleted. A fresh token/authentication is required to push the second pass; never commit or paste a token into this file.

## Basair translation status (unchanged)

```text
TOTAL=85850 TRADUITS=1783 RESTANTS=84067 UNIQUES=79685 DOUBLONS=6165
```

`resume_safe.py` remains the crash-safe translator. Translation work was paused while the Masoudi fix was completed.
