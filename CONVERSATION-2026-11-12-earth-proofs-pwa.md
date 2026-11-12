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
- App commit: `60eb632` — `feat: add multilingual cosmic proofs encyclopedia`

### Features

- 200 numbered entries, 10 strands × 20 entries.
- Languages: Arabic (RTL), English, French, Spanish.
- Instant search by number, title, or keyword.
- Category filtering.
- Accessible detail modal with previous/next navigation.
- Light and dark themes.
- Responsive desktop/mobile layout, no horizontal overflow at 390 px.
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
- 10 categories + “All” filter = 11 pills.
- Category filter returned 20 entries.
- Search returned matching results.
- Modal opened, counter showed `1 / 200`, next button moved to `2 / 200`.
- Language switch to English changed the heading and direction.
- Theme switch changed `data-theme`.
- Image download completed with “All images are saved for offline use.”
- Service worker became active and cached 21 shell assets.
- Offline reload still rendered 200 cards and showed “Offline.”
- No console errors, page errors, or failed requests.
- All JSON files parsed; all JS files passed `node --check`; all SVGs passed XML validation.
- Mobile viewport 390 px: no horizontal overflow.

### Live app tests

- https://ucfzem.github.io/earth-proofs-pwa/ returned HTTP 200.
- `data.json` returned HTTP 200 with 200 entries.
- Manifest, icons, and SVG diagrams returned HTTP 200.
- Live page rendered 200 cards.
- Images rendered correctly after lazy loading.
- Search/filter/modal/language switching worked.
- Service worker cache contained 21 shell assets.
- Offline live reload rendered 200 cards and showed “Offline.”
- No console errors.

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
  - `earth-proofs-shell-v1.0.0`
  - `earth-proofs-images-v1`
- Works cache:
  - `works-v1.0.31`
- No existing app folders in `ucfzem.github.io` were modified.
- Only `works/index.html` and `works/sw.js` were changed in the link-tree repository.
- Backups of the pre-change works files were saved locally before editing.

## Shared links

- App: https://ucfzem.github.io/earth-proofs-pwa/
- App repository: https://github.com/ucfzem/earth-proofs-pwa
- Link tree: https://ucfzem.github.io/works/
- Link-tree repository: https://github.com/ucfzem/ucfzem.github.io
- Conversation backup: this file in `ucfzem/session-backups`
