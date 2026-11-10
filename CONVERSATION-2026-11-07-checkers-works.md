# Checkers Master — ajout dans Works / Maktaba (déverrouillé)

## Demande
Ajouter le jeu "Checkers Master" (PWA dames américaines) au link tree `ucfzem.github.io/works`, dans la section déverrouillée, en dehors du dossier Maktaba, avec une icône soignée. Puis push, déployer, sauvegarder le backup md et partager les liens.

## Changement
- `works/index.html` : ajout d'une entrée dans le tableau `publicProjects` (section déverrouillée), hors du tableau `maktaba` :
  - `num: 18`, emoji `♟️`, nom "Checkers Master", tag "Jeux", icône SVG inline (pièce de dame bleue foncée avec reflet), URL `https://ucfzem.github.io/checkers/`, `newTab: true`.
- `works/sw.js` : cache PWA bumpé de `works-v1.0.29` à `works-v1.0.30`.

## Tests
- Local (Playwright) : `checkersInWorks: true` (entrée présente dans la page Works).
- Live GitHub Pages : `LIVE_OK` — l'entrée `Checkers Master` est présente dans le HTML servi.

## Commit site
- `3f93f3c` — `feat(works): add Checkers Master to unlocked publicProjects`

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- Works (link tree) : https://ucfzem.github.io/works/
- Code HTML Works : https://github.com/ucfzem/ucfzem.github.io/blob/main/works/index.html
- Backup md : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-checkers-works.md
