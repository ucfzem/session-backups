# Sifat Al-Safwa — Works/Maktaba listing — 2026-11-07

## Request

- Put the latest Sifat Al-Safwa webapp in the Works link tree, in the Maktaba folder with the other unlocked webapps.
- Do it carefully, choose a good icon, then push/deploy/save backup and share links.

## Change

The Sifat Al-Safwa card was already present in `maktaba`; this pass only refreshed its card metadata:

- `صفة الصفوة` fallback emoji changed from generic `📖` to `💎` (elite/companion biographies), keeping the custom `icon.svg`.
- Works service worker cache bumped from `works-v1.0.27` to `works-v1.0.28`.

## Care check

Before committing, `git diff` was inspected: only one line in `works/index.html` and the cache string in `works/sw.js` changed. No Maktaba entries were removed.

## Test

Local Playwright check:

```text
sifatInMaktaba true
```

Live GitHub check after push:

```text
LIVE_OK
{ emoji: "💎", name: "صفة الصفوة", tag: "Biographies", icon: "https://ucfzem.github.io/sifat-al-safwa/icon.svg", url: "https://ucfzem.github.io/sifat-al-safwa/", newTab: true }
```

## Commit

- `f71eb93` — `feat(works): refresh Sifat Al-Safwa Maktaba card icon`

## Links

- Works: <https://ucfzem.github.io/works/>
- Maktaba folder: <https://ucfzem.github.io/works/#f-maktaba>
- Sifat Al-Safwa: <https://ucfzem.github.io/sifat-al-safwa/>
- GitHub Works HTML: <https://github.com/ucfzem/ucfzem.github.io/blob/main/works/index.html>
- Raw Works HTML: <https://raw.githubusercontent.com/ucfzem/ucfzem.github.io/main/works/index.html>
- Backup md: <https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-sifat-works-maktaba.md>
