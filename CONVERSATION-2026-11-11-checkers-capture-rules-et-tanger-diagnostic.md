# Checkers — règles de rafle (recapture / case visitée) + diagnostic page Tanger

## 1. Checkers Master — correctif règles de rafle

### Symptôme
Pendant une rafle, le moteur pouvait théoriquement re-sauter une pièce déjà capturée ou revenir sur une case déjà visitée. La simulation IA ne transmettait pas l'historique de la rafle.

### Correctif (`checkers/game.js`, `checkers/ai.js`)
- `getLegalMovesForPiece(r, c, board, capturedPieces = [], visitedSquares = null)`.
- Chaque `move` contient désormais :
  - `capturedPieces` : pièces capturées accumulées pendant la rafle ;
  - `visitedSquares` : cases d'atterrissage accumulées, case de départ incluse.
- Un saut est refusé si la pièce du milieu est déjà dans `capturedPieces` ou si la case d'arrivée est déjà dans `visitedSquares`.
- `finalizeMove` transmet ces tableaux aux continuations de rafle.
- `sequenceLength` / `chainAfter` (prise maximale espagnole) transmettent aussi ces tableaux.
- `ai.js` : `collectCapturedPieces` / `collectVisitedSquares` + `expandCaptureSequences` passent l'historique à `getLegalMovesForPiece`.
- `applyMoveOnSimulation` retire déjà la pièce capturée à chaque étape (conservé).
- **P2P non modifié** : `finalizeRemoteMove` reste inchangé.

### Tests
Harnais `/tmp/capture-rules-test.js` charge le vrai `game.js` + `ai.js` :
- saut simple : `capturedPieces` et `visitedSquares` corrects ;
- recapture d'une pièce déjà prise : refusée ;
- atterrissage sur case visitée : refusé ;
- rafle à 2 sauts : tableaux accumulés (2 prises, 3 cases) ;
- cycle de 6 sauts : l'IA plafonne à 5 sauts et ne revient jamais au départ ;
- pièces retirées progressivement du plateau simulé ;
- dame volante espagnole : atterrissage visité et pièce déjà prise refusés.

**Résultat : 22 assertions OK.**

### Commit / live
- Commit : `93e7a25` — `fix(checkers): interdit recapture et case visitée en rafle (game + IA)`
- Live vérifié :
  - `game.js` sha256 `c01a913772cb7e1733a246984420306b7f83441be56d86df9cc1c9ed00cf2eda`
  - `ai.js` sha256 `d8dc58b1e91f8b7f552daf078fdf3912824aae14066e6631713555888398db35`

## 2. Page Tanger d'Antan — diagnostic + ajout section « Histoire »

### Vérifications effectuées
- Dépôt réel : `ucfzem/tanger`, un seul fichier `index.html` (aucun `tangier.html`).
- Sections présentes dans le HTML : `.hero`, `#galerie` + `.gallery-section`, `#musique`, `#partager`, puis footer/lightbox.
- Aucune section « Histoire / suite de l'histoire » dans le fichier actuel **ni dans aucun commit** de l'historique.
- Rendu live vérifié avec Playwright (FR et AR, 1440×900) :
  - hauteur de page 5 928 px (FR) / 6 550 px (AR) ;
  - galerie : 60 images chargées (`naturalWidth` > 0) ;
  - `#musique` et `#partager` visibles (`display:block`, `opacity:1`) ;
  - aucune erreur console / `pageerror`.
- Le dépôt Tanger ne contient **aucun service worker** (`sw.js`, `serviceWorker`, `CACHE_NAME` absents). L'incrément de cache n'est donc pas applicable.
- `_worker.js` (proxy Cloudflare) force déjà `cache-control: no-cache, no-store, must-revalidate`.

### Cause exacte
La deuxième section historique n'était pas masquée par le CSS, le JS ou le cache : **elle n'existait pas dans le code source**. La page ne contenait que le hero, la galerie, la musique et le partage.

### Correctif appliqué (section « Histoire » ajoutée)
- Nouvelle section `#histoire` placée après la navigation, avant la galerie.
- Contenu FR / EN / ES / AR : introduction, 3 paragraphes historiques et 4 jalons (1923, 1940–1950, 1956, aujourd'hui).
- Lien de navigation « Histoire / History / Historia / التاريخ » + skip link mis à jour vers `#histoire`.
- CSS dédié responsive (4 colonnes → 2 → 1), bordures adaptées au RTL, polices arabes Amiri.
- `console.log("Section 2 chargée !")` ajouté au début du script principal.
- Photos, galerie et lecteur musique non modifiés.

### Tests
- Local : 12 contrôles × 4 langues × 3 viewports (desktop/tablet/mobile), tous OK.
- Live : 12 contrôles × 4 langues × 2 domaines (GitHub Pages + worker), tous OK.
- Vérifié : section visible, traductions, timeline, RTL, galerie 60 images, musique, skip link, aucune erreur JS, aucun débordement horizontal.

### Commit / live
- Commit : `841d3e7` — `Add Histoire de Tanger section (FR/EN/ES/AR) + nav link + responsive timeline`
- Fichier live : 114 736 octets, sha256 `bcd22337c09cac204ed5e7f94e1be4a3dd915890c67f2a8e8083f025815e27ca`
- GitHub Pages et worker Cloudflare servent exactement le même fichier.

## Liens
- Checkers live : https://ucfzem.github.io/checkers/
- Commit Checkers : https://github.com/ucfzem/ucfzem.github.io/commit/93e7a25
- Tanger live : https://ucfzem.github.io/tanger/
- Tanger worker : https://ucfzem.azer-tyu199p.workers.dev/tanger/
- Commit Tanger : https://github.com/ucfzem/tanger/commit/841d3e7
- Dépôt Tanger : https://github.com/ucfzem/tanger
- Backup : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-capture-rules-et-tanger-diagnostic.md
