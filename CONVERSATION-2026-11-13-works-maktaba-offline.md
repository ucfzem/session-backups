# Conversation Backup — Works/Maktaba Offline Loading Fix (2026-11-13)

## Status

Completed, pushed, deployed to GitHub Pages, verified live, and backed up.

## User request

- Some webapps in `https://ucfzem.github.io/works/` (Maktaba) did not load offline.
- Diagnose and fix them so they load offline too.
- Do not break existing apps, styling, data, or offline behavior.
- Then push/deploy, save this conversation as a GitHub Markdown backup, and share all links.

## Root causes

- The root service worker omitted 8 Maktaba apps from both `SHELLS` and `APPS`:
  `masail-nafi-ibn-azraq`, `risalat-ibn-fadlan`, `barnabas`, `musawa-qasas`,
  `al-ajwiba-al-musaddada`, `basair02`, `al-a3chab-attebiah`, `earth-proofs-pwa`.
- `prefetchBookData()` used marker `/__bookdata-v34`, so existing installs skipped
  caching newly added apps and their book data.
- `ASSET_EXT` did not include `.md`, so `al-ajwiba-al-musaddada/data/book.md`
  was not cached and offline showed `لا يوجد كتاب لم يُعثر على الكتاب`.
- `livre-al-istibsar` was linked to a cross-origin Cloudflare worker URL; the root
  service worker can only intercept same-origin app paths.
- `tafsir-ibn-taymiyyah/data/index.json` is an object with a `chapters` list, not
  the flat array expected by the generic prefetch logic, so chapter text was not cached.

## Changes

### `sw.js` (root service worker)

- Added the 8 missing apps to `SHELLS` and `APPS`.
- Bumped `DATA_MARKER` from `/__bookdata-v34` to `/__bookdata-v37` so existing
  installs recache the complete library once.
- Added `md|txt|csv|xml` to `ASSET_EXT`.
- Added support for object-style data indexes with `chapters[].file` and
  `chapters[].search`, caching `.json` chapter text and `.txt` search data.
- Kept all existing app lists, cache-first/network-first behavior, and CDN handling intact.

### `works/index.html`

- Changed the Maktaba Al-Istibsar URL from the cross-origin worker to
  `https://ucfzem.github.io/livre-al-istibsar/`.
- Updated its JSON-LD structured-data URL to the same canonical GitHub Pages URL.

### `works/sw.js`

- Bumped page cache from `works-v1.0.31` to `works-v1.0.32`.

## Commits

Repository: https://github.com/ucfzem/ucfzem.github.io

- `492c4ac` — `fix(offline): precache all Maktaba apps and book text data`
- `f94c179` — `fix(works): point Al-Istibsar structured data to GitHub Pages`
- `90e8c2b` — `fix(offline): cache chapter data for object-style book indexes`

Final `main` after this work: `90e8c2b`

Commit links:

- https://github.com/ucfzem/ucfzem.github.io/commit/492c4ac
- https://github.com/ucfzem/ucfzem.github.io/commit/f94c179
- https://github.com/ucfzem/ucfzem.github.io/commit/90e8c2b

Note: concurrent QR-code/landing-page commit `d563c36` was already on `main`
and remains included. This offline work did not modify its files.

## Verification

### Pre-fix reproduction

- Live Works page loaded with marker `/__bookdata-v34`.
- Going offline, `masail-nafi-ibn-azraq` failed with `net::ERR_INTERNET_DISCONNECTED`.
- `sifat-al-safwa` still loaded, confirming partial library coverage.
- `al-ajwiba-al-musaddada` failed its book request because `book.md` was not cached.

### Local verification

- `node --check` passed for root `sw.js` and `works/sw.js`.
- Static check: no Maktaba slug missing from `SHELLS`/`APPS`; no list duplicates;
  both lists contain 41 entries and all 30 unique Maktaba slugs are covered.
- Local offline tests passed for the affected apps, including `masail`, `barnabas`,
  `musawa`, `al-ajwiba`, `basair02`, `al-a3chab`, `risalat`, `earth-proofs-pwa`,
  and `tafsir`.
- `al-ajwiba` data loaded after the `.md` asset fix.
- Tafsir chapter text loaded offline after the object-index fix; all 7 `.json`
  chapters and all 7 `.txt` search files were cached.
- Earth Proofs loaded 200 cards offline at app version `v1.2.1`.
- `git diff --check` clean.

### Live verification after deployment

GitHub Pages built `90e8c2b`, then a fresh browser context loaded the Works page
online, prefetched data, switched offline, and opened every Maktaba app.

Results:

- Prefetch marker `/__bookdata-v37` present.
- Shell cache contained 863 entries.
- All 30 unique Maktaba app shells were cached; no missing entries.
- `al-ajwiba-al-musaddada/data/book.md` cached.
- Earth Proofs `data.json` cached.
- Tafsir: 7 chapter JSON files and 7 search text files cached.
- All 30 apps passed offline navigation with no local asset request failures.
- Key checks:
  - `masail-nafi-ibn-azraq`: 165,165 characters rendered offline.
  - `barnabas`: 7,008 characters rendered offline.
  - `musawa-qasas`: 23,237 characters rendered offline.
  - `al-ajwiba-al-musaddada`: 1,508 characters rendered offline.
  - `tafsir-ibn-taymiyyah`: 4,984 characters rendered offline (previously a 239-character shell).
  - `earth-proofs-pwa`: 200 cards, `v1.2.1`, navigation icon rendered offline.
- Works page Al-Istibsar links now point to `https://ucfzem.github.io/livre-al-istibsar/`.
- `works/sw.js` is live as `works-v1.0.32`.

## Live links

- Works index: https://ucfzem.github.io/works/
- Maktaba section: https://ucfzem.github.io/works/#f-maktaba
- Maktaba landing/QR page: https://ucfzem.github.io/works/Maktaba/
- Al-Istibsar: https://ucfzem.github.io/livre-al-istibsar/
- Earth Proofs PWA: https://ucfzem.github.io/earth-proofs-pwa/
- Tanger gallery: https://ucfzem.github.io/tanger/
- Tanger Histoire: https://ucfzem.github.io/tanger/histoire/
- GitHub repository: https://github.com/ucfzem/ucfzem.github.io
- Root service worker: https://ucfzem.github.io/sw.js
- Works service worker: https://ucfzem.github.io/works/sw.js

## Notes

- Apps that register their own service worker can still log
  `An unknown error occurred when fetching the script.` while offline; the root
  service worker still serves their shell and cached data. This is non-fatal.
- `al-a3chab-attebiah` text and shell work offline. Its Wikimedia images are
  external and remain network-dependent; they were not bulk-cached to avoid a
  large unintended download.
- `basair02` appears twice in the Maktaba markup. The duplicate is pre-existing
  and does not affect offline loading; all unique slugs are covered.
- No secrets are stored in this document.
