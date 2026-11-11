# ROLLBACK — Tanger d'Antan

Current deployed version (2026-11-11, nav-layout fix):
- Repo: `ucfzem/tanger`
- Commit: `2b5eb2f927d2070da06feb74011b3b77d5588a3f`
- `index.html` blob SHA: `10956f77b6f1789aec3cd461a5c5fa0958334376`
- Size: 103,070 bytes
- SHA-256: `8e8dd82dbbd2a06e258e3279bf8ad8d2a827e74b8431c0d380f1d8ac8b8fc425`

This version keeps the original nav geometry (text-only `FR EN ES AR` buttons) and adds only non-layout features (localized title/aria, skip link, RTL slider math, perf/a11y). Theme, colors, fonts, photos, audio unchanged.

## Known-good targets

### 1. Original pre-upgrade site (smallest, safest)
- Commit: `1b48b12689f5e07a7bc9093d5d7343abeb0b285c`
- Blob: `c88421028e59ff04260bb049dbd8a42e938232e7`
- Size: 94,750 bytes
- SHA-256: `819ab056813ec6a8b128c918d64bed5c28b93121669eaad9d19d7a2883473014`

### 2. First additive upgrade (flags/names — nav regression, do NOT keep)
- Commit: `43696e74f5926d54ee956d242ad2a7d67db95ac1`
- Blob: `6bab46d201ca1912760568354b94157f59e6d102`
- Size: 104,716 bytes

### 3. Rollback commit applied 2026-11-11
- Commit: `0df94114af022250b7367dd32b79f0b6becf67ad` (restored blob `c884210…`)

## Option 1 — git
```bash
git clone https://github.com/ucfzem/tanger.git
cd tanger
git checkout 1b48b12 -- index.html        # original site
# or: git checkout 2b5eb2f -- index.html  # current fixed version
git commit -m "Rollback index.html"
git push origin main
```

## Option 2 — GitHub Contents API (repo is heavy; no clone needed)
1. `GET https://api.github.com/repos/ucfzem/tanger/contents/index.html?ref=<commit>`
2. Take the returned `content` (base64) and `sha`; `PUT` it back to the same path with `"branch":"main"` and the current blob `sha`.

## Post-rollback check
```bash
curl -sL "https://ucfzem.github.io/tanger/?ts=$(date +%s)" | sha256sum
# original: 819ab056813ec6a8b128c918d64bed5c28b93121669eaad9d19d7a2883473014
# current fixed: 8e8dd82dbbd2a06e258e3279bf8ad8d2a827e74b8431c0d380f1d8ac8b8fc425
```
Worker `https://ucfzem.azer-tyu199p.workers.dev/tanger/` proxies the same GitHub Pages content, so it follows automatically.
