# Masail Nafi ibn al-Azraq — Works Maktaba addition

## Request

- Add the Masail Nafi ibn al-Azraq webapp to the Works link tree, in the Maktaba folder with the other unlocked webapps.
- Do it carefully; previous mistakes removed many webapps.

## Care check

- Before committing, `git diff` showed:
  - one new line in `works/index.html`,
  - one cache-version line in `works/sw.js`,
  - only new files under `masail-nafi-ibn-azraq/`.
- No existing Maktaba cards were removed or reordered.

## Build

Created a static PWA app in `masail-nafi-ibn-azraq/`:

- `index.html` — RTL reading UI, search/highlight, word/root/number index, 4 UI languages (ar/fr/en/es), online/offline badge.
- `masail.json` — parsed from Turath book 23622 pages; **249 masail** (IDs 1–250, one source number missing), 193 with structured poetry.
- `manifest.json`, `sw.js`, icons.

## Tests

- Local app search test: searching `عزين` returned 1 card; opening the first item rendered 1 reader card.
- Works Playwright check: `masailInMaktaba true`.
- Live check after push: new site loads and live Works HTML contains the Masail card.

## Commit

- `5a0c3f1` — `feat(masail): add Masail Nafi ibn al-Azraq app and Works Maktaba card`

## Links

- Works: <https://ucfzem.github.io/works/>
- Maktaba folder: <https://ucfzem.github.io/works/#f-maktaba>
- Masail app: <https://ucfzem.github.io/masail-nafi-ibn-azraq/>
- GitHub app HTML: <https://github.com/ucfzem/ucfzem.github.io/blob/main/masail-nafi-ibn-azraq/index.html>
- GitHub Works HTML: <https://github.com/ucfzem/ucfzem.github.io/blob/main/works/index.html>
- Raw app data: <https://raw.githubusercontent.com/ucfzem/ucfzem.github.io/main/masail-nafi-ibn-azraq/masail.json>
- Backup md: <https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-masail-nafi-ibn-azraq.md>
