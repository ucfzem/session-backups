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

## 3. Correctif bugs galerie + drapeaux (2026-11-12)

### Bug 1 — galerie coupée / images invisibles
- Cause : les `.gallery-item` démarraient à `opacity:0` et n'étaient révélés que par un `IntersectionObserver` au scroll. Un défilement rapide ou un saut direct vers `#galerie` pouvait sauter des éléments, qui restaient invisibles. `content-visibility:auto` + `contain-intrinsic-size` aggravait le rendu partiel.
- Correctif : suppression de l'état `opacity:0` / `translateY`, suppression de l'observer de fade-in et de `content-visibility:auto`. Les 60 éléments sont visibles dès le rendu, quel que soit le scroll.
- Vérifié : HTML valide (aucune balise non fermée), 60 `<img>` présents, aucun `max-height` / `overflow:hidden` / `display:none` sur la galerie.

### Bug 2 — drapeaux absents
- Cause : les boutons de langue étaient volontairement passés en texte seul (FR/EN/ES/AR) lors d'un correctif précédent de largeur de nav.
- Correctif : ajout des emojis directement dans le HTML — 🇫🇷 FR, 🇬🇧 EN, 🇪🇸 ES, 🇲🇦 AR — avec `.lang-flag` (emoji font, `margin-inline-end`, sans overflow).
- Vérifié de 320 px à 1920 px, FR et AR, sans débordement horizontal.

### Service Worker
- Tanger n'a **aucun** `sw.js` et la page live ne possède aucun service worker de contrôle (`navigator.serviceWorker.controller === null`, zéro registration).
- Le SW racine de `ucfzem.github.io` (scope `/`) ignore `/tanger/` car Tanger n'est pas dans sa liste `APPS`.
- Il n'y a donc pas de version `v5 → v6` à incrémenter pour Tanger. Le worker Cloudflare envoie déjà `no-cache` ; GitHub Pages/CDN peut conserver l'ancien HTML quelques minutes, un rechargement forcé suffit.

### Commit / live
- Commit : `0fcfffe` — `Fix gallery blank rows and add language flag emojis`
- Fichier live : sha256 `3a59222e75792e046927390662ac5db590f985c586ce08d171e63c418a2a31e7`
- GitHub Pages et worker servent le même fichier.

## 4. Page complète « Histoire de Tanger » (2026-11-12)

### Source et intégration
- Page fournie via `https://ucfzem.github.io/droppy/get.html?id=e2a57f56`.
- Zip réel : `https://ucfzem.azer-tyu199p.workers.dev/droppy/dl/e2a57f56/tanger-histoire-update.zip` (sha256 `25142a57665e00e24997b3a0c3a79a39bc41b33acab985ec55ff8a522128fe22`).
- Fichiers intégrés dans `ucfzem/tanger` :
  - `index.html` : lien nav `histoire/`, bouton « Lire l'histoire complète → », traductions `history_more` FR/EN/ES/AR ;
  - `sitemap.xml` : ajout de `https://ucfzem.github.io/tanger/histoire/` (42 URLs) ;
  - `histoire/index.html` : page complète, 12 chapitres, 22 figures, 17 liens de table des matières.

### Images
- Les 24 références `images.openai.com` renvoyaient **HTTP 400 publiquement** (pas seulement dans la sandbox).
- Elles ont été remplacées par des photos locales `../images/...` : 2 références hero + 22 figures.
- Hero local : `../images/cap-spartel-1920.jpg`.
- Vérifié : aucune URL `images.openai.com` restante, aucun HTTP ≥ 400 sur les images, 22/22 figures chargées après scroll.

### Tests locaux
- HTML valide sur `index.html` et `histoire/index.html`.
- 342 clés `data-i18n` : aucune manquante dans les 4 langues (AR/EN/FR/ES).
- FR/EN/ES : zéro caractère arabe résiduel après changement de langue ; AR : rendu RTL correct.
- Page histoire : thème sombre `body rgb(10, 8, 5)`, 12 chapitres visibles, `content-visibility` désactivé, 22 images locales, TOC non sticky (17 liens), 4 drapeaux, aucun overflow, aucune erreur JS, mode low-end sombre.
- Page principale : nav `histoire/`, bouton traduit, 60 images de galerie visibles/chargées, 4 drapeaux, aucun overflow, aucune erreur JS.
- Faux négatifs de test corrigés : images lazy comptées avant scroll, TOC attendu à tort ≥ 18 alors que 17 est correct, sélecteur `.flag` au lieu de `.lang-flag`.

### Live vérifié
- Commit : `a893be0` — `Add full Tangier history page with local archive images`
- `index.html` : 115 114 octets, sha256 `e326255751780e1b8c4800b784994b4bfa02761605aaf6270f1f6c557a3548f6`
- `histoire/index.html` : 269 196 octets, sha256 `aa82f856b5ef497991aa7393e9382c2f61a3475ef9ea00881e8b1fc2dae2a424`
- GitHub Pages et worker Cloudflare servent exactement les mêmes fichiers.
- 16 combinaisons testées en live (2 domaines × 4 langues × desktop/mobile), toutes OK.
- Persistance de langue testée sur les 2 domaines : EN depuis la page principale → page histoire, puis AR depuis l'histoire → page principale.
- Aucun service worker ajouté ou modifié.

## Liens
- Checkers live : https://ucfzem.github.io/checkers/
- Commit Checkers : https://github.com/ucfzem/ucfzem.github.io/commit/93e7a25
- Tanger live : https://ucfzem.github.io/tanger/
- Tanger worker : https://ucfzem.azer-tyu199p.workers.dev/tanger/
- Histoire live : https://ucfzem.github.io/tanger/histoire/
- Histoire worker : https://ucfzem.azer-tyu199p.workers.dev/tanger/histoire/
- Commit Tanger (section Histoire) : https://github.com/ucfzem/tanger/commit/841d3e7
- Commit Tanger (correctif galerie + drapeaux) : https://github.com/ucfzem/tanger/commit/0fcfffe
- Commit Tanger (page complète Histoire) : https://github.com/ucfzem/tanger/commit/a893be0
- Dépôt Tanger : https://github.com/ucfzem/tanger
- Backup : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-capture-rules-et-tanger-diagnostic.md
