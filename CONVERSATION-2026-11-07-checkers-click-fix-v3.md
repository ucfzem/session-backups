# Checkers Master — anti-cache + réparation boutons (version 3)

## Problème
Le jeu restait bloqué (pions immobiles) et l'interface pouvait être figée. Deux causes traitées :

1. **Service Worker en cache-d'abord** servait l'ancien `game.js` même après déploiement → l'utilisateur testait du vieux code.
2. **Régression CSS introduite** : `pointer-events: none` appliqué par erreur à `.controls-card` et `.status-card`, rendant inactifs les boutons « Recommencer / Sons / Menu Principal ».

## Corrections (commit `35e7c0c`)

### `checkers/sw.js`
- Cache bumpé `v2` → `v3`.
- Stratégie **réseau d'abord** (network-first) : en ligne, le navigateur récupère toujours la dernière version de `game.js`, `index.html`, `style.css` ; le cache ne sert qu'en mode hors-ligne. Fin du problème de code obsolète.

### `checkers/index.html`
- Enregistrement SW avec `{ updateViaCache: 'none' }` + `registration.update()` pour forcer la vérification de mise à jour.

### `checkers/style.css`
- `pointer-events: none` retiré de `.status-card` et `.controls-card` → boutons de contrôle de nouveau cliquables.
- `pointer-events: none` conservé **uniquement** sur `.message-card` (aucun bouton dedans).
- `#board` conserve `touch-action: none`.

## Vérifications
- `node -c game.js` et `node -c sw.js` : OK.

## Procédure de test (IMPORTANT, à cause du cache PWA)
1. Ouvrir https://ucfzem.github.io/checkers/
2. F12 → onglet *Application* → *Service Workers* → « Unregister », puis *Storage* → « Clear site data »
3. Recharger deux fois (le nouveau SW v3 s'installe)
4. Cliquer un pion blanc → console doit afficher :
   - `CLIC DETECTE`
   - `Coordonnées reçues: X Y`
   - `Pièce cliquée: blanc Tour actuel: blanc`
5. Cliquer une case diagonale → le pion doit se déplacer.

## Commit
- `35e7c0c` — `fix(checkers): répare boutons contrôles + SW réseau-d'abord (anti-cache)`

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- Code : https://github.com/ucfzem/ucfzem.github.io/tree/main/checkers
- JS : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/game.js
- CSS : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/style.css
- SW : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/sw.js
- Backup : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-checkers-click-fix-v3.md