# Conversation Backup — Cosmic Proofs Encyclopedia (2026-11-12)

## User request

Build a polished multilingual edition of the “Cosmic Proofs” material, then:

- Add it to the link tree at `https://ucfzem.github.io/works/`.
- Put it in the Maktaba folder with the other unlocked web apps.
- Choose a good icon.
- Test it carefully before deploying.
- Push/deploy/save it.
- Back up the conversation as a GitHub Markdown file.
- Share all links.
- Take special care not to remove or break existing sites.

## Delivered app

- App: https://ucfzem.github.io/earth-proofs-pwa/
- Repository: https://github.com/ucfzem/earth-proofs-pwa
- Latest app commit: `ad82808` — `feat(theme): apply Nuzhat al-Mushtaq manuscript palette`
- Earlier app commits:
  - `ecff630` — `feat(nav): add searchable proof index drawer`
  - `6335a1b` — `feat(i18n): use flag pills and smooth language transition`
  - `345482e` — `fix(i18n): isolate language storage and polish responsive typography`
  - `60eb632` — `feat: add multilingual cosmic proofs encyclopedia`
- Footer version: `v1.1.0`

### Features

- 200 numbered entries, 10 strands × 20 entries.
- Languages: Arabic (RTL), English, French, Spanish.
- Dynamic flag pills: 🇲🇦 AR, 🇺🇸 EN, 🇫🇷 FR, 🇪🇸 ES, with accessible labels and pressed state.
- Fade-and-slide transition on the header title and card grid when changing language.
- Hamburger button opening a searchable index drawer containing all 200 proofs.
- Drawer search by proof number, title, or category, with direct modal navigation.
- Drawer opens from the logical start side: left in LTR, right in RTL.
- Instant search by number, title, or keyword.
- Category filtering.
- Accessible detail modal with previous/next navigation.
- Light and dark themes.
- Responsive desktop/mobile layout, no horizontal overflow at 320, 360, or 390 px.
- Arabic is the default language; browser auto-detection is intentionally disabled.
- Nuzhat al-Mushtaq palette: obsidian/charcoal + antique gold in dark mode, warm parchment + bronze-gold in light mode.
- Dark mode is now the default for new visitors; saved light/dark preference is respected.
- Amiri is used for Arabic scholarly reading and Inter for clean Latin/UI text.
- Close controls are minimalist 34 px × buttons instead of text labels.
- Language/theme storage keys are namespaced to avoid collisions on the shared `ucfzem.github.io` origin.
- Category counts are refreshed after `data.json` finishes loading, so they no longer remain at 0 on first render.
- The Service Worker caches the Google Fonts stylesheet and font files after the first controlled load for offline typography.
- Installable PWA manifest with SVG favicon and PNG icons.
- Service worker offline support for the app shell, full dataset, and diagrams.
- “Download images for offline use” button using the Cache API.
- Original SVG diagrams for all 10 strands.

### Strands

1. Water & Level
2. Horizon & Visibility
3. Motion & Gravity
4. Sun, Moon & Stars
5. Air & Pressure
6. Curvature & Measurement
7. Perspective & Optics
8. Maps & Travel
9. Space & Satellites
10. Poles & Antarctica

### Content note

The 200 entries are neutral paraphrases of commonly circulated arguments, followed by scientific and analytical notes. The notes explain the mainstream physical and geodetic context and are not presented as absolute consensus. The content is an educational critical-study index, not a verbatim reproduction of any book.

## Link tree integration

Updated:

- https://ucfzem.github.io/works/
- https://github.com/ucfzem/ucfzem.github.io
- Works commit: `432e6df` — `feat(works): add cosmic proofs app to maktaba`

Maktaba entry:

- Name: الأدلة الكونية
- Tag: Cosmography
- Icon: https://ucfzem.github.io/earth-proofs-pwa/images/icon-192.png
- URL: https://ucfzem.github.io/earth-proofs-pwa/

The existing Maktaba list went from 30 to 31 entries. All previous entries were preserved. `works/sw.js` cache was bumped from `works-v1.0.30` to `works-v1.0.31` and the new icon was added to its `CORE` list.

## Verification performed

### Local app tests

- 200/200 cards rendered.
- Old `app_lang`/`app_theme` keys were ignored; app still started in Arabic.
- Four flag pills rendered with correct flags, initials, names, titles, and `aria-pressed` state.
- Each language click updated `lang`, `dir`, heading, cards, active pill, namespaced storage key, and card-grid animation.
- Keyboard activation of a language pill worked.
- Hamburger drawer opened with 200 index items and moved focus to its close button.
- Drawer search for `97` returned proofs 97 and 197; an impossible query showed the translated empty state.
- Selecting proof 200 from the drawer opened the modal at `200 / 200`, even while the main grid was filtered to 20 cards.
- Escape and backdrop click closed the drawer and restored focus to the hamburger button.
- Focus trapping worked inside the drawer.
- Drawer title, placeholder, empty state, and item titles updated with the active language.
- 10 categories + “All” filter = 11 pills, with correct counts: 200 total and 20 per strand.
- Category filter returned 20 entries.
- Search returned matching results.
- Modal opened; next/previous navigation updated the counter correctly.
- Theme switch changed `data-theme`.
- Image download completed with “All images are saved for offline use.”
- Service worker became active with cache `earth-proofs-shell-v1.4.0`.
- Offline reload still rendered 200 cards and showed “Offline.”
- No console errors, page errors, or failed requests.
- All JS files passed `node --check`; all SVGs passed XML validation.
- Default theme was dark: obsidian `#0e1116`, parchment text `#e6e1d6`, antique-gold `#d4af37`.
- Light theme switched correctly to parchment `#f7f4eb`, sepia ink `#231f1a`, bronze-gold `#9a7218`.
- Computed Arabic font was Amiri; computed English font was Inter.
- Search clear, modal close, and drawer close rendered as 34 px × controls with translated `aria-label`/`title`.
- Mobile viewports 320/360/390 px: no horizontal overflow; header controls wrap cleanly.

### Live app tests

- https://ucfzem.github.io/earth-proofs-pwa/ returned HTTP 200.
- `index.html`, `style.css`, `app.js`, `translations.js`, `data.json`, `manifest.json`, `sw.js`, PNG icon, and SVG diagram all returned HTTP 200.
- Live deployment contains `langContainer`, `renderLanguageSelector`, `lang-animating`, `hamburgerBtn`, `proofsDrawer`, the Amiri/Inter import, and `earth-proofs-shell-v1.4.0`.
- Live page rendered 200 cards with `عدد المداخل: 200`.
- Live category counts were correct: 200 total and 20 per strand.
- Live default theme was dark with the Nuzhat palette; light theme toggle produced the parchment palette.
- Live Arabic typography loaded Amiri; English typography loaded Inter.
- Live × close controls were present in the search bar, modal, and drawer.
- All four flag pills switched language and direction correctly; animation class was applied each time.
- Live hamburger drawer listed 200 proofs; filtering and direct jump to proof 200 worked.
- LTR drawer opened on the left; RTL drawer opened on the right.
- Mobile drawer was 340 px wide at 390 px viewport and listed all 200 proofs.
- Search for `water` returned 23 entries; modal next/previous navigation worked.
- Service worker controlled the page; manifest had 3 icons and `display: standalone`.
- Cache `earth-proofs-shell-v1.4.0` contained 28 cached requests, including 6 Google Fonts requests.
- Offline Arabic reload still used Amiri after the second controlled load.
- Offline live reload rendered 200 cards with correct counts and showed “دون اتصال.”
- Offline live drawer still listed all 200 proofs.
- Mobile viewport 390 px: no horizontal overflow; four pills remained on one row.
- No console errors, page errors, or failed requests.
- Lighthouse was not run in this environment; manifest, SW registration/control, install metadata, and offline behavior were verified directly.

### Live works tests

- https://ucfzem.github.io/works/ rendered 31 Maktaba cards.
- New entry appeared exactly once with the correct name, tag, URL, and icon.
- Icon loaded at 192×192.
- Known older entries were still present: أسرار الكون، عجائب المخلوقات، رحلة ابن بطوطة، المعشبة المغربية.
- No console errors on the works page.

## Key files in the app

```text
index.html
style.css
translations.js
app.js
data.json
manifest.json
sw.js
favicon.svg
.nojekyll
README.md
images/
  water.svg
  horizon.svg
  physics.svg
  astronomy.svg
  atmosphere.svg
  geometry.svg
  observation.svg
  navigation.svg
  space.svg
  polar.svg
  icon-192.png
  icon-512.png
  icon-maskable-512.png
  apple-touch-icon.png
```

## Important implementation notes

- Dataset is 200 entries with the shape `{id, categoryKey, image_url, translations:{ar,en,fr,es:{title,content,scientific_note}}}`.
- Images are original category diagrams reused across each strand.
- App caches:
  - `earth-proofs-shell-v1.4.0`
  - `earth-proofs-images-v1`
- Works cache:
  - `works-v1.0.31`
- Namespaced storage keys:
  - `earth-proofs-lang`
  - `earth-proofs-theme`
- No existing app folders in `ucfzem.github.io` were modified.
- Only `works/index.html` and `works/sw.js` were changed in the link-tree repository.
- Backups of the pre-change works files were saved locally before editing.
- `translations.js` now stores `name`, `initial`, and `flag` metadata for each language.
- The language selector is rendered dynamically by `app.js`; click handling is delegated from `#langContainer`.
- Language transitions animate `.brand-title` and `#cardsGrid` via the `.lang-animating` class.
- Drawer markup is `#proofsDrawer`; it is opened by `#hamburgerBtn` and filtered through `#drawerSearchInput`.
- Drawer items are real buttons with delegated click handling, `aria-label`s, focus trapping, Escape/backdrop close, and focus restoration.
- Reduced-motion preferences are respected by the existing global animation guard.
- The Service Worker keeps the established `earth-proofs-shell-*` naming and robust cache strategies rather than renaming to a generic cache key; version `v1.4.0` forces the update and removes older shell caches.
- Google Fonts CSS is cached as an opaque response; font files are cached via stale-while-revalidate.
- `manifest.json` and the `theme-color` meta now use the Nuzhat dark theme colors.

## Shared links

- App: https://ucfzem.github.io/earth-proofs-pwa/
- App repository: https://github.com/ucfzem/earth-proofs-pwa
- Link tree: https://ucfzem.github.io/works/
- Link-tree repository: https://github.com/ucfzem/ucfzem.github.io
- Conversation backup: this file in `ucfzem/session-backups`


## Addendum — old-device/TV fixes after this backup

The app has since been updated beyond the state described above:
- `ad82808` → `0790121` — old-device/Smart-TV optimization, D-pad navigation, focus restoration, color-mix fallbacks, app `v1.2.0`, SW `earth-proofs-shell-v1.5.0`.
- `0790121` → `25b53bf` — eager image loading for old devices, TV/lite `content-visibility` fallback, app `v1.2.1`, SW `earth-proofs-shell-v1.5.1`.

Current verified state:
- 200 card images and the modal image use `loading="eager"`.
- No WebP is used; diagrams are SVG.
- TV/lite mode restores `content-visibility: visible`.
- Live test: 200/200 images loaded, drawer/modal D-pad works, no console errors.

Detailed combined backup:
https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-12-old-devices-tv-and-arabic-fonts.md
