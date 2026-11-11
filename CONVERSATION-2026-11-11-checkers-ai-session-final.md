# Conversation — Checkers Master : diagnostic & correctifs IA (2026-11-11, session finale)

## Objectif de la session
Rendre `ucfzem.github.io/checkers/` fonctionnel, puis diagnostiquer et corriger une IA niveau 5 jugée « très facile » malgré profondeur 8 + quiescence.

## Chronologie
1. **Logs de diagnostic** ajoutés dans `ai.js` (niveau/profondeur, nb de coups, éval plateau de départ, lancement quiescence). Throttlés à 1×/coup pour éviter ~70 000 lignes par réflexion.
2. **Mesures réelles** via un banc Node (`/tmp/ai-harness.js`) reproduisant `getAllLegalMoves`/`getLegalMovesForPiece` de `game.js`.
3. **Conclusion diagnostic** : les 3 suspects étaient faux.
   - Profondeur 8 réellement atteinte (~58 891 fins de profondeur).
   - Quiescence réellement branchée (~70 384 appels).
   - Éval de départ = 0/0 → aucune inversion de signe.
4. **Vrais défauts trouvés** :
   - `triggerAIMove()` jouait un coup **aléatoire** pendant les prises multiples.
   - Boucle racine sans alpha partagé + garde-fou de temps → repli statique naïf sur les coups restants (blunders).
5. **Correctifs appliqués** :
   - Prises multiples → `getBestMoveFromList(...)` (vraie recherche).
   - Racine → **iterative deepening** 2→8, alpha partagé, ordonnancement des coups, budget sûr (itération interrompue abandonnée, on garde la dernière profondeur complète).
6. **Tests** : niveau 5 → profondeur 8 ; test prise multiple → `CAPTURE_CHOSEN=true` (exit 0).
7. **Déploiement vérifié live** : ai.js 11492 o, game.js 45254 o (contiennent `getBestMoveFromList`).

## Commits du site (`ucfzem/ucfzem.github.io`, branche main)
```
61edc20 fix(ai): recherche des prises multiples + iterative deepening (alpha partagé, ordonnancement, budget sûr)
ce4ab5b chore(ai): logs de diagnostic (profondeur, coups, éval départ, quiescence) throttlés
6fcb9cb feat(ai): profondeur 4=7 / 5=8 + garde-fou temporel anti-freeze
586293a feat(ai): quiescence search (exploration des captures, anti-effet d'horizon)
7f6574d feat(ai): évaluation enrichie (pièces menacées, ligne de fond, pions connectés)
581a8e9 feat(checkers): remplace par Checkers Master v4 (thèmes + dames espagnoles) + SW réseau-d'abord v5
35e7c0c fix(checkers): répare boutons contrôles + SW réseau-d'abord (anti-cache)
12dfc60 fix(checkers): 5 correctifs sécurité (cache SW, pointer-events, isAnimating sécurisé, logs debug)
```

## Commits backup (`ucfzem/session-backups`, branche main)
```
20227c8 Add Checkers AI fixes report
eff75df Add Checkers AI diagnostic report
bd1c3b8 Add Checkers AI v2 improvement backup
cb57655 Add Checkers v4 replacement backup
c0b5043 Add Checkers anti-cache v3 backup
50d433f Add Checkers 5 fixes backup
1b47ea9 Add Checkers click fix backup
b311172 Add Checkers P2P multiplayer backup
```

## Fichiers clés (site)
- `checkers/index.html` — UI, option « Dames espagnoles », enregistrement SW anti-cache (`updateViaCache: 'none'` + `registration.update()`).
- `checkers/game.js` (1263 lignes) — `triggerAIMove` ~1249, `getLegalMovesForPiece` ~1117, `getAllLegalMoves` ~1160, `keepMaxCaptures` ~1181.
- `checkers/ai.js` — `getBestMove`, `getBestMoveFromList`, `searchWithMoves`, `searchRoot`, `orderMoves`, `minimax`, `quiescenceSearch`, `evaluateBoard`, `isThreatened`.
- `checkers/sw.js` — réseau-d'abord, `CACHE_NAME = 'checkers-master-v5'`.
- `checkers/style.css` — `touch-action: none` sur `#board`, `pointer-events: none` sur `.message-card`.
- `checkers/manifest.json`.

## Détails techniques importants
- `game.js` utilise `this.variant` (`'spanish'`/`'american'`), pas `gameMode`, pour le mode espagnol ; `ai.js` lit `this.game.variant` pour `spanishPenalty`.
- `aiLevel` transmis par `btnStartGame` (`parseInt(this.menuAiDifficulty.value,10)`) → `triggerAIMove` → `getBestMove`.
- Profondeurs : `{1:2, 2:3, 3:4, 4:7, 5:8}` (moins 1 en espagnol niveau ≥4). Budgets : 1200/3000/6000 ms.
- Éval : positif = bon pour l'IA ; pions/dames menacés −50/−150, etc.
- Cache : tester en navigation privée ou *Unregister + Clear site data* après déploiement.

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- Dépôt site : https://github.com/ucfzem/ucfzem.github.io
- `checkers/ai.js` : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- `checkers/game.js` : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/game.js
- Dépôt backups : https://github.com/ucfzem/session-backups
- Rapport diagnostic : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-ai-diagnostic.md
- Rapport correctifs : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-ai-fix.md

## Prochaines pistes (optionnel)
- Mesurer la force réelle en parties auto-play (IA vs IA) après correctifs.
- Ajouter une table de transposition (positions répétées) pour renforcer encore le niveau 5.
- Améliorer l'ordonnancement (killer moves / historique) pour exploiter pleinement le budget 6 s.
