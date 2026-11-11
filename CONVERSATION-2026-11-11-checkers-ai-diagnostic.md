# Checkers Master — diagnostic IA (logs déployés + mesures réelles)

## But
Vérifier 3 bugs suspectés : profondeur non appliquée, quiescence non branchée, inversion de signe dans l'évaluation.

## Méthode
- Logs de diagnostic ajoutés dans `ai.js` (throttlés à 1 fois par coup pour éviter ~70 000 lignes/console figée).
- **Exécution réelle de l'IA en Node** via un banc d'essai (`/tmp/ai-harness.js`) qui reproduit `getAllLegalMoves`/`getLegalMovesForPiece` de `game.js`.
- Niveau testé : 5 (Expert), couleur IA = Noir, plateau initial 24 pièces.

## Résultats réels (mesurés)
```
IA - Niveau demandé: 5 Profondeur calculée: 8
IA - Nombre de coups légaux à évaluer: 7
Fin de profondeur atteinte, lancement Quiescence
Quiescence - Profondeur: 0 Captures possibles: 2
IA - Meilleur score trouvé: 1 Nombre de meilleurs coups: 1
START_EVAL_NOIR = 0
START_EVAL_BLANC = 0
CHOSEN = {fromR:2,fromC:3,toR:3,toC:4,captured:null}
TIME_MS ≈ 1600–3900 ms
```
- « Fin de profondeur atteinte » : ~58 891 occurrences → **profondeur 8 réellement atteinte**.
- « Quiescence » : ~70 384 appels → **quiescence réellement branchée**.
- Évaluation initiale = **0 / 0** → **pas d'inversion de signe**.

## Conclusion sur les 3 suspects
1. Profondeur non appliquée → **FAUX** (8 bien utilisée).
2. Quiescence non branchée → **FAUX** (appelée massivement).
3. Inversion de signe → **FAUX** (éval départ = 0 équilibrée).

## Vrais problèmes trouvés dans `game.js` / `ai.js`
1. **`triggerAIMove()` (game.js ~1249)** : si `activeMultiJumpPiece`, l'IA joue un coup **au hasard** (`validMoves[Math.floor(Math.random()*...)]`) au lieu de réfléchir.
2. **Garde-fou global (`this.deadline`)** : la boucle racine appelle `minimax(..., -Infinity, Infinity, ...)` pour CHAQUE coup sans partager alpha. En milieu de partie (plus de coups), le budget 6 s peut être épuisé dès les premiers coups racine ; les coups suivants retombent alors sur `evaluateBoard` statique (naïf) et peuvent **supplanter** un coup profond mieux évalué → blunders. Absence d'ordonnancement des coups aggrave le problème.
3. **Cache navigateur / Service Worker** possible (test sur ancienne version). Le SW est en réseau-d'abord depuis la v5, mais un `Unregister + Clear site data` reste nécessaire une fois.

## Logs déployés (throttlés)
- `IA - Niveau demandé: <level> Profondeur calculée: <depth>`
- `IA - Nombre de coups légaux à évaluer: <n>`
- `IA - Meilleur score trouvé: <score> Nombre de meilleurs coups: <n>`
- `IA - Évaluation du plateau de départ: <score> (aiColor: ..., pièces: 24)` (1×/coup)
- `Fin de profondeur atteinte, lancement Quiescence` (1×/coup)
- `Quiescence - Profondeur: <q> Captures possibles: <n>` (1×/coup)

## Commit
- `ce4ab5b` — logs de diagnostic throttlés (déployé, live vérifié : ai.js 9847 o)

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- IA : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- Backup : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-ai-diagnostic.md
