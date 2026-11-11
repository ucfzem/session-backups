# Checkers Master — correctif rafles / inversion de tour (IA)

## Symptôme
Même en « Expert », l'IA restait très facile à battre malgré profondeur 8 + quiescence.

## Cause racine trouvée
Dans `minimax`, lors d'une **rafle (multi-prise)**, le code faisait :
```js
const nextTurn = currentTurn === COLOR_WHITE ? COLOR_BLACK : COLOR_WHITE; // inversé même si isMultiJump
const nextMaximizing = isMultiJump ? maximizingPlayer : false;
```
→ `maximizingPlayer` restait celui du même camp, mais `currentTurn` passait à l'adversaire.
Résultat : l'IA générait les coups de l'adversaire **en les évaluant comme les siens** (perspective inversée). Comme les prises sont obligatoires et fréquentes aux dames, l'IA se trompait constamment sur les échanges → jeu faible.

De plus, les continuations de rafle n'étaient pas restreintes à la même pièce.

## Correctif
Les rafles sont désormais traitées comme **un tour atomique complet** :
- `getTurnMoves(color, board)` génère toutes les séquences de prise complètes (via `expandCaptureSequences`), une séquence = un coup de recherche.
- `applySequence(board, sequence)` applique toute la rafle d'un coup.
- Le tour alterne **toujours** (`maximizingPlayer = !maximizingPlayer`, `currentTurn` inversé) → perspective min/max cohérente.
- Règle du couronnement respectée : une pièce couronnée arrête la rafle (`justCrowned`).
- `getBestMoveFromList` (utilisé pendant une rafle en cours de jeu) expanse aussi les continuations et renvoie la **première** étape de la meilleure séquence ; le jeu poursuit ensuite pas à pas.
- Ordonnancement : prises et rafles longues d'abord.

## Tests locaux (Node, modèle identique à game.js)
```
Niveau 5 → profondeur 8, 7 coups, éval départ 0/0
Double rafle: TURN_MOVES=1, seq steps=2, DOUBLE_JUMP_SEQUENCE=true
CHOSEN_FIRST={fromR:2,fromC:1,toR:4,toC:3,captured:{r:3,c:2}}  CHOSE_CAPTURE=true (exit 0)
Temps: ~4,5 s (budget 6 s)
```

## Commit / déploiement
- Commit : `09ea72f` — fix(ai): rafles = tours atomiques (corrige inversion de tour en multi-prise + perspective min/max)
- Live vérifié : `checkers/ai.js` = 13091 octets (contient `getTurnMoves`).

## Rappel
Tester en navigation privée ou *Unregister + Clear site data* pour écarter le cache.

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- ai.js : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- Backup : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-ai-multijump-fix.md
