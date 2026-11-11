# ROLLBACK — Tanger d'Antan

The 2026-11-11 upgrade of `ucfzem/tanger` → `index.html` was **additive** (theme, colors, fonts, photos, audio unchanged). If anything must be reverted:

## Previous known-good version
- Repo: `ucfzem/tanger`
- Commit: `1b48b12689f5e07a7bc9093d5d7343abeb0b285c`
- `index.html` blob SHA: `c88421028e59ff04260bb049dbd8a42e938232e7`
- Size: 94,750 bytes
- SHA-256: `819ab056813ec6a8b128c918d64bed5c28b93121669eaad9d19d7a2883473014`

## Option 1 — git
```bash
git clone https://github.com/ucfzem/tanger.git
cd tanger
git checkout 1b48b12 -- index.html
git commit -m "Rollback index.html to pre-upgrade version"
git push origin main
```

## Option 2 — GitHub Contents API (repo is heavy; no clone needed)
1. `GET https://api.github.com/repos/ucfzem/tanger/contents/index.html?ref=1b48b12`
2. Take the returned `content` (base64) and `sha`; `PUT` it back to the same path with `"branch":"main"` and the current blob `sha`.

## Option 3 — single-attribute rollback
The upgrade is isolated in clearly marked blocks:
- `<html ... data-lang="fr">` + ROLLBACK comment on line 1.
- CSS block starting `/* ═══ TANGER UPGRADE (ADDITIVE) ...` before `</style>`.
- Language buttons markup, `data-i18n-aria` attributes, skip link.
- `TITLES` map, `applyLang` additions, `setPlayAria`, IO fallback, RTL slider branches.
- Final `<script>` (low-end + TV keys) before `</body>`.

Removing these blocks restores the previous behavior without touching content.

## Post-rollback check
```bash
curl -sL "https://ucfzem.github.io/tanger/?ts=$(date +%s)" | sha256sum
# expect 819ab056813ec6a8b128c918d64bed5c28b93121669eaad9d19d7a2883473014 after Pages propagates
```
Worker `https://ucfzem.azer-tyu199p.workers.dev/tanger/` proxies the same GitHub Pages content, so it follows automatically.
