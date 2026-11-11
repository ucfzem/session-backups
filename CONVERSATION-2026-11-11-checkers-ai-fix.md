# Checkers Master — correctifs IA (prises multiples + iterative deepening)

## Contexte
Suite au diagnostic (logs + mesures Node) : profondeur 8, quiescence et évaluation de départ saines. Les vrais défauts étaient ailleurs.

## Correctif 1 — prises multiples (`game.js`, `triggerAIMove`)
Avant : lors d'un enchaînement de prises, l'IA jouait `validMoves[Math.random()]`.
Après : `this.aiEngine.getBestMoveFromList(this.board, this.turn, this.aiLevel, this.validMoves)` → l'IA réfléchit réellement parmi les prises légales.

## Correctif 2 — recherche racine (`ai.js`)
- Remplacement de la boucle racine naïve par **iterative deepening** (profondeur 2 → maxDepth).
- **Alpha partagé** entre coups racine (élagage).
- **Ordonnancement des coups** (prises/dames/centre d'abord).
- **Budget de temps sûr** : une itération interrompue est **abandonnée** ; on conserve le meilleur coup de la dernière profondeur *entièrement* terminée → plus de repli statique qui fait perdre un coup déjà trouvé.
- Nouvelle méthode `getBestMoveFromList(...)` + `searchWithMoves(...)` + `searchRoot(...)` + `orderMoves(...)`.

## Tests locaux (Node, logique identique à game.js)
```
IA - Niveau demandé: 5 Profondeur calculée: 8 Coups: 7
IA - Meilleur score trouvé: 1 coup: {fromR:2,fromC:3,toR:3,toC:4,captured:null}
START_EVAL_NOIR=0 / START_EVAL_BLANC=0
```
Test prise multiple (Noir 1 pièce vs Blanc 3, 2 prises possibles) :
```
RESULT={"fromR":3,"fromC":3,"toR":1,"toC":1,"captured":{"r":2,"c":2}}
CAPTURE_CHOSEN=true   (exit 0)
```

## Commit
- `61edc20` — fix(ai): recherche des prises multiples + iterative deepening
- Fichiers live vérifiés : ai.js 11492 o, game.js 45254 o (contiennent `getBestMoveFromList`).

## Rappel cache
Tester en navigation privée ou *Unregister + Clear site data* pour éviter l'ancienne version.

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- ai.js : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- game.js : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/game.js
- Backup : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-ai-fix.md
