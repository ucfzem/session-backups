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
- Latest app commit: `6335a1b` — `feat(i18n): use flag pills and smooth language transition`
- Earlier app commits:
  - `345482e` — `fix(i18n): isolate language storage and polish responsive typography`
  - `60eb632` — `feat: add multilingual cosmic proofs encyclopedia`

### Features

- 200 numbered entries, 10 strands × 20 entries.
- Languages: Arabic (RTL), English, French, Spanish.
- Dynamic flag pills: 🇲🇦 AR, 🇺🇸 EN, 🇫🇷 FR, 🇪🇸 ES, with accessible labels and pressed state.
- Fade-and-slide transition on the header title and card grid when changing language.
- Instant search by number, title, or keyword.
- Category filtering.
- Accessible detail modal with previous/next navigation.
- Light and dark themes.
- Responsive desktop/mobile layout, no horizontal overflow at 320, 360, or 390 px.
- Arabic is the default language; browser auto-detection is intentionally disabled.
- Language/theme storage keys are namespaced to avoid collisions on the shared `ucfzem.github.io` origin.
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
- 10 categories + “All” filter = 11 pills.
- Category filter returned 20 entries.
- Search returned matching results.
- Modal opened; next/previous navigation updated the counter correctly.
- Theme switch changed `data-theme`.
- Image download completed with “All images are saved for offline use.”
- Service worker became active with cache `earth-proofs-shell-v1.2.0`.
- Offline reload still rendered 200 cards and showed “Offline.”
- No console errors, page errors, or failed requests.
- All JS files passed `node --check`; all SVGs passed XML validation.
- Mobile viewports 320/360/390 px: no horizontal overflow; 390 px header fits in two rows.

### Live app tests

- https://ucfzem.github.io/earth-proofs-pwa/ returned HTTP 200.
- `index.html`, `style.css`, `app.js`, `translations.js`, `data.json`, `manifest.json`, `sw.js`, PNG icon, and SVG diagram all returned HTTP 200.
- Live deployment contains `langContainer`, `renderLanguageSelector`, `lang-animating`, and `earth-proofs-shell-v1.2.0`.
- Live page rendered 200 cards.
- All four flag pills switched language and direction correctly; animation class was applied each time.
- Search for `water` returned 23 entries; modal next/previous navigation worked.
- Service worker controlled the page; manifest had 3 icons and `display: standalone`.
- Cache `earth-proofs-shell-v1.2.0` contained 21 shell assets.
- Offline live reload rendered 200 cards and showed “Hors ligne.”
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
  - `earth-proofs-shell-v1.2.0`
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
- Reduced-motion preferences are respected by the existing global animation guard.

## Shared links

- App: https://ucfzem.github.io/earth-proofs-pwa/
- App repository: https://github.com/ucfzem/earth-proofs-pwa
- Link tree: https://ucfzem.github.io/works/
- Link-tree repository: https://github.com/ucfzem/ucfzem.github.io
- Conversation backup: this file in `ucfzem/session-backups`
