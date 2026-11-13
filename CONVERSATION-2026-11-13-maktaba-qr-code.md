# Conversation Backup — Maktaba QR Code & Landing Page (2026-11-13)

## Status

Completed, pushed, deployed to GitHub Pages, verified live, and backed up.

## User request

- Create a high-resolution QR code linking exclusively to `https://ucfzem.github.io/works/Maktaba/`.
- Use error correction level H (30%).
- Put a clean two-line center wordmark in the QR code: `UcfZem` / `Maktaba`.
- Save `.svg` and `.png` assets inside `works/Maktaba/`.
- Optionally add a lightweight standalone HTML preview if none existed.
- Do not modify, move, rename, or delete any existing webapp or file.
- Then push/deploy, save this conversation as a GitHub Markdown backup, and share all links.

## Delivered files

Added only these new files under `works/Maktaba/`:

```text
works/Maktaba/index.html
works/Maktaba/maktaba-qr.svg
works/Maktaba/maktaba-qr.png
```

No existing files or webapp folders were changed.

## QR code details

- Target URL: `https://ucfzem.github.io/works/Maktaba/`
- Error correction: level H (30%)
- QR version: 5, 37 modules
- Output size: 2160 × 2160 px
- Quiet zone: 4 modules
- Center wordmark: `UcfZem` (teal) and `Maktaba` (deep ink green), converted to vector paths so the SVG is font-independent
- Format: true vector SVG plus PNG rendered from that SVG

## Landing page

`works/Maktaba/index.html` is a standalone preview with:

- The QR code, using SVG with PNG fallback
- The exact target URL
- Local download links for the SVG and PNG assets
- No external scripts, no external fonts, no trackers

## Commit

Repository: https://github.com/ucfzem/ucfzem.github.io

- `d563c36` — `feat(works): add Maktaba QR code and landing page`

Commit link: https://github.com/ucfzem/ucfzem.github.io/commit/d563c36

## Verification performed

### Local

- PNG and SVG both decode to exactly `https://ucfzem.github.io/works/Maktaba/`.
- SVG was rasterized independently and decoded successfully.
- PNG still decoded after downscaling to 800, 512, 400, 320, 256, 200, and 150 px.
- Center text bounding box measured at 384 × 160 px inside the 480 × 288 px center panel, visually centered.
- Local HTTP checks returned 200 for the landing page, SVG, and PNG.
- HTML parsed cleanly; the only absolute URL in the page is the intended Maktaba target.
- `git status` showed only `?? works/Maktaba/` before commit; no tracked files were changed.

### Live

- https://ucfzem.github.io/works/Maktaba/ returned HTTP 200.
- Live page title: `Maktaba · UcfZem QR Code`.
- Live PNG returned HTTP 200, `image/png`, 2160 × 2160, and decoded to the exact target URL.
- Live SVG returned HTTP 200, `image/svg+xml`, rasterized, and decoded to the exact target URL.
- Raw GitHub versions of all three files returned HTTP 200.

## Live links

- Maktaba landing page: https://ucfzem.github.io/works/Maktaba/
- QR PNG: https://ucfzem.github.io/works/Maktaba/maktaba-qr.png
- QR SVG: https://ucfzem.github.io/works/Maktaba/maktaba-qr.svg
- GitHub folder: https://github.com/ucfzem/ucfzem.github.io/tree/main/works/Maktaba
- Code commit: https://github.com/ucfzem/ucfzem.github.io/commit/d563c36
- Works index: https://ucfzem.github.io/works/
- Works Maktaba section: https://ucfzem.github.io/works/#f-maktaba

## Backup links

- This backup: https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-13-maktaba-qr-code.md
- Raw backup: https://raw.githubusercontent.com/ucfzem/session-backups/main/CONVERSATION-2026-11-13-maktaba-qr-code.md

## Notes

- GitHub Pages deployed automatically after the push to `main`.
- No secrets are stored in this document.
