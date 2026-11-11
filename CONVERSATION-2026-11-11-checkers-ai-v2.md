# Checkers Master — amélioration de l'IA (3 prompts, minimax renforcé)

## Objectif
Rendre l'IA « Expert » redoutable : meilleure évaluation, anti-effet d'horizon, profondeur accrue sans freezer le navigateur.

## Fichier modifié
- `checkers/ai.js` uniquement (3 commits séparés, un par prompt).

---

## PROMPT 1 — Évaluation enrichie (commit `7f6574d`)
`evaluateBoard(boardState, aiColor)` conserve les valeurs (Pion=100, Dame=300, bonus centre/avancement) et ajoute :

- **Sécurité / pièces menacées** : nouvelle méthode `isThreatened(boardState, r, c, piece, enemyColor)` qui détecte si une pièce peut être capturée au prochain tour (4 diagonales, direction de capture des pions respectée, dames volantes incluses).
  - Pion IA menacé : **-50** ; Dame IA menacée : **-150**.
  - Pièce adverse menacée : **+50** (pion) / **+150** (dame).
- **Ligne de fond** : **+10** par pion resté sur sa rangée de départ (Blancs rangée 7, Noirs rangée 0).
- **Pions connectés** : **+5** par pion ayant un allié sur une diagonale adjacente.
- Performance : détection de menace en O(cases × 4), sans génération lourde de coups.

---

## PROMPT 2 — Quiescence Search (commit `586293a`)
- `minimax`, à `depth === 0`, appelle désormais `quiescenceSearch(...)` au lieu de `evaluateBoard` directement (le cas « aucun coup légal » reste une évaluation terminale).
- Nouvelle méthode `quiescenceSearch(boardState, alpha, beta, currentTurn, aiColor, qDepth)` :
  - Arrêt à `qDepth >= 4` (anti-boucle) → évaluation.
  - Ne considère que les **captures** (`legalMoves.filter(m => m.captured)`).
  - Aucune capture possible → évaluation (« stand pat »).
  - Sinon évalue le plateau puis simule chaque capture avec élagage alpha-bêta (maximise/minimise selon `currentTurn === aiColor`).
  - Gère les prises multiples (`isMultiJump` conserve le tour).
- L'IA voit maintenant les échanges de pions jusqu'au bout avant de décider.

---

## PROMPT 3 — Profondeur accrue + garde-fou (commit `6fcb9cb`)
Variante sûre choisie (le Web Worker complet a été écarté : la classe `CheckersAI` dépend de l'instance de jeu `this.game`, non transposable directement dans un worker sans dupliquer tout le moteur).

- `depthMap` : niveau 4 **5 → 7**, niveau 5 **6 → 8** (pénalité dames espagnoles conservée).
- **Garde-fou temporel anti-freeze** : `this.deadline = Date.now() + budgetMs` avec budget 6000 ms (niv. 5), 3000 ms (niv. 4), 1200 ms (niv. ≤3). `minimax` et `quiescenceSearch` retournent l'évaluation dès le dépassement → le navigateur ne gèle jamais.
- `console.log` d'avertissement au niveau 5 : `IA Expert : réflexion jusqu'à N coups (budget 6000 ms max)...`

---

## Vérification live (GitHub Pages)
- `ai.js` : HTTP 200, **8773 octets**, contient `quiescenceSearch` (×4) et `5: 8` ✅

## Commits
- `7f6574d` — évaluation enrichie
- `586293a` — quiescence search
- `6fcb9cb` — profondeur 4=7 / 5=8 + garde-fou temporel

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- IA : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- Code : https://github.com/ucfzem/ucfzem.github.io/tree/main/checkers
- Backup : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-ai-v2.md
