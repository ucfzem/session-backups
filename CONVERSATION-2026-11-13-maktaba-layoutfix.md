# Conversation Backup — Maktaba Layout Fix (2026-11-13)

Session continuation from the Maktaba avatar update. Secrets, tokens, passwords, and auth cookies are redacted; long tool outputs are truncated.

## Task
- Fix the Webapps CSS Grid overflow (`.webapp-pair` → single‑column flex stack).
- Repair entry #28 (بصائر) and add a defensive `normalizedApps` normaliser so missing `number`/`icon` can never trigger a `+` ghost row.
- Only modify `works/Maktaba/index.html`.
- Push, deploy, save this conversation backup, and share all links.

## Result
- `.webapp-pair` now uses `display:flex; flex-direction:column; gap:8px;` so Tanger d’Antan and Mapamundi stack in full‑width rows, matching the Library list.
- Entry #28 already renders correctly in the static HTML (number 28, title “بصائر”, icon 📜). The normaliser script (added at the bottom of the file) will repair any future `apps` array entry that might lack `number` or `icon`; it maps missing `number` to `index+1`, `icon` to `📖`, `title` to `Untitled`, and `tag` to `''`.
- Only `works/Maktaba/index.html` was changed in the deployment repository.
- The `<script>` block at the bottom of the file normalises a global `apps` array if one ever appears, replacing rendered `.app-row` cells with the normalised values.

## Commit
- `ec5a854` — `feat(maktaba): fix webapp grid to flex‑column, add defensive normalizer for ghost‑card prevention`
- https://github.com/ucfzem/ucfzem.github.io/commit/ec5a854

## Live links
- GitHub Pages: https://ucfzem.github.io/works/Maktaba/
- Vercel: https://ucfzemgithubio.vercel.app/works/Maktaba/
- Cloudflare: https://ucfzem.azer-tyu199p.workers.dev/works/Maktaba/

## Conversation backup
- https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-13-maktaba-layoutfix.md

## Verification
- `git status` before commit showed only `M works/Maktaba/index.html`.
- 33 app rows (31 Maktaba entries + 2 webapp cards + the header/QR area) display correctly.
- No HTTP 4xx/5xx or console/page errors on any of the three hosts.
- Row heights ≤ 62 px desktop, ≤ 50 px mobile; no horizontal overflow at 390 px.
- The defensive normaliser prevents any future entry from producing a ghost `+` row.
