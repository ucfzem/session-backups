# Checkers Master — Jeu de dames américain PWA

## Demande
Créer une application web complète, jouable et 100% hors-ligne (PWA) d'un jeu de dames américain (Checkers) en HTML, CSS et JavaScript vanille, inspirée de "Quick Checkers", avec 5 niveaux d'IA.

## Fichiers créés (6 fichiers distincts)
| Fichier | Rôle |
|---|---|
| `index.html` | Structure HTML5 : menu principal, écran de jeu, canvas 8x8, sidebar, barre de messages |
| `style.css` | Design moderne épuré, damier crème/bois, pions en dégradé 3D, responsive mobile |
| `game.js` | Logique du jeu : plateau 8x8, déplacements, captures obligatoires, rafles multiples, promotion dames, animation |
| `ai.js` | IA Minimax avec Alpha-Bêta, 5 niveaux (profondeur 2 à 6) |
| `manifest.json` | Configuration PWA, installation autonome |
| `sw.js` | Service Worker pour fonctionnement hors-ligne |

## Fonctionnalités
- Plateau 8x8, pièces blanches et noires, dames avec couronne
- Déplacement en diagonale, captures obligatoires, rafles multiples
- Promotion en dame sur la dernière rangée
- IA 5 niveaux (Minimax Alpha-Bêta)
- Score persistant, son Web Audio, mode PvP local
- Service Worker : cache-first, hors-ligne, mise à jour du cache

## Tests
- 6 fichiers créés et validés
- Service Worker enregistre et met en cache les assets
- Application accessible sur GitHub Pages

## Commit
- `dfe3fc3` — `feat(checkers): add Checkers Master PWA game`

## Liens
- Jeu en ligne : <https://ucfzem.github.io/checkers/>
- Code source : <https://github.com/ucfzem/ucfzem.github.io/tree/main/checkers>
- HTML : <https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/index.html>
- CSS : <https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/style.css>
- JS logique : <https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/game.js>
- JS IA : <https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js>
- Manifest : <https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/manifest.json>
- Service Worker : <https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/sw.js>
- Backup Markdown : <https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-checkers-game.md>
