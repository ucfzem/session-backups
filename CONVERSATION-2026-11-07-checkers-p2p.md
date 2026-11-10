# Checkers Master — mode multijoueur Pair-à-Pair (PeerJS)

## Demande
Ajouter un mode multijoueur en ligne Peer-to-Peer au jeu de dames Checkers Master (`ucfzem.github.io/checkers/`), utilisant la librairie PeerJS.

## Changements

### `checkers/index.html`
- Ajout du CDN PeerJS : `<script src="https://unpkg.com/peerjs@1.5.2/dist/peerjs.min.js"></script>`
- Ajout de l'option `<option value="online">En ligne</option>` dans le sélecteur `#menuGameMode`
- Ajout du groupe `#menuOnlineGroup` (caché par défaut) contenant :
  - `<input id="menuRoomCode">` pour saisir un code de salon
  - `<button id="btnCreateRoom">Créer une partie</button>` — génère un code 4 lettres et crée le salon
  - `<button id="btnJoinRoom">Rejoindre</button>` — se connecte au salon saisi
  - `<div id="menuRoomCodeDisplay">` affiche le code généré (côté hôte)

### `checkers/game.js`
- Ajout des propriétés PeerJS : `peer`, `peerConnection`, `myColor`, `isHost`, `roomCode`
- `setupPeerEvents()` : initialise le pair, gère les événements `open`, `connection`, `error`, `disconnected`, `close`
- `setupConnectionEvents(conn)` : gère l'ouverture de connexion, la réception de données (`data.type === 'move'`), la fermeture et les erreurs
- `createRoom()` : génère un code 4 lettres, crée `new Peer(code)` en tant qu'hôte (Blanc), affiche le code
- `joinRoom()` : crée un peer anonyme, se connecte au code saisi en tant qu'invité (Noir)
- `generateRoomCode()` : génère un code aléatoire de 4 caractères alphanumériques
- `handleCanvasClick` : bloque le clic si ce n'est pas le tour du joueur local en mode online (`this.turn !== this.myColor`)
- `switchTurn(move)` : après un coup local en mode online, envoie `{ type: 'move', move }` via `peerConnection.send()`
- `handleRemoteMove(move)` : applique le coup distant localement (animation + mise à jour du plateau) sans renvoyer sur le réseau
- `finalizeRemoteMove(move, piece)` : finalise le coup distant (couronne, rafles multiples, changement de tour)
- `checkMandatoryCaptures()` : affiche un message adapté en mode online ("Tour de vous / l'adversaire")

## Règles du mode online
- Le créateur de salle est l'hôte et joue les Blancs (commence).
- L'invité qui rejoint joue les Noirs.
- Les coups sont synchronisés en temps réel via PeerJS (WebRTC).
- Les rafles multiples et la promotion dame sont gérées des deux côtés.
- Si la connexion se coupe, un message s'affiche.

## Test
- Fichiers servis avec HTTP 200 sur GitHub Pages.
- `index.html` contient bien `peerjs.min.js`, l'option "En ligne", `btnCreateRoom`, `btnJoinRoom`.

## Commit
- `2438817` — `feat(checkers): add PeerJS online multiplayer mode`

## Liens
- Jeu en ligne : https://ucfzem.github.io/checkers/
- Code source : https://github.com/ucfzem/ucfzem.github.io/tree/main/checkers
- HTML : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/index.html
- JS logique : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/game.js
- IA : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- CSS : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/style.css
- Manifest PWA : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/manifest.json
- Service Worker : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/sw.js
- Backup Markdown : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-07-checkers-p2p.md
