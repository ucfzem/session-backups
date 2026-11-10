# Checkers Master — correction du bug de déplacement (tactile mobile)

## Demande
Corriger le bug de déplacement sur le jeu de dames américain (Checkers Master) déployé sur `ucfzem.github.io/checkers/`. Le clic ne fonctionnait pas correctement sur mobile (écran haute densité type Samsung S23 Ultra).

## Cause racine
Dans `checkers/game.js`, le gestionnaire `handleCanvasClick` (clic souris) appliquait déjà la mise à l'échelle des coordonnées (`scaleX = canvas.width / rect.width`, `scaleY = canvas.height / rect.height`), mais l'écouteur `touchstart` (tactile mobile) utilisait les coordonnées brutes sans redimensionnement :
```js
this.processClick(touch.clientX - rect.left, touch.clientY - rect.top);
```
Sur un écran haute densité (DPR 2 ou 3), les coordonnées tactiles ne correspondaient pas aux cases du canvas, rendant le jeu injouable sur mobile.

## Correction
Ajout de `scaleX`/`scaleY` dans `touchstart`, identique à `handleCanvasClick` :
```js
const scaleX = this.canvas.width / rect.width;
const scaleY = this.canvas.height / rect.height;
const x = (touch.clientX - rect.left) * scaleX;
const y = (touch.clientY - rect.top) * scaleY;
this.processClick(x, y);
```

## Tests
- Tous les fichiers Checkers retournent HTTP 200 sur GitHub Pages :
  - index.html : 200
  - game.js : 200
  - sw.js : 200
- Le `touchstart` applique maintenant la même mise à l'échelle que le clic souris.

## Commit
- `ef181b1` — `fix(checkers): tactile mobile scaling (scaleX/scaleY)`

## Liens
- Jeu en ligne : https://ucfzem.github.io/checkers/
- Code source : https://github.com/ucfzem/ucfzem.github.io/tree/main/checkers
- HTML : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/index.html
- JS logique : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/game.js
- IA : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- CSS : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/style.css
- Manifest PWA : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/manifest.json
- Service Worker : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/sw.js
- Backup Markdown : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-checkers-fix.md
