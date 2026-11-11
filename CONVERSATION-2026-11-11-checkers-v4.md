# Checkers Master — remplacement par la version v4 (zip Droppy)

## Demande
Remplacer l'ancien jeu Checkers par la version fournie dans `checkers-v4.zip` (partage Droppy `id=1b1157c9`), puis déployer, sauvegarder et partager les liens.

## Source du fichier
- Page : https://ucfzem.github.io/droppy/get.html?id=1b1157c9
- API : https://ucfzem.azer-tyu199p.workers.dev/droppy/list/1b1157c9 → `checkers-v4.zip` (sans mot de passe)
- Téléchargement : https://ucfzem.azer-tyu199p.workers.dev/droppy/dl/1b1157c9/checkers-v4.zip

## Contenu du zip (6 fichiers)
- `index.html` (6,1 Ko) — inclut l'option **Dames espagnoles (plateau inversé)**
- `game.js` (45,2 Ko) — moteur complet : thèmes clair/sombre, mode espagnol, IA, en ligne, annulation
- `ai.js` (4,3 Ko)
- `style.css` (12,7 Ko)
- `sw.js` (1,1 Ko)
- `manifest.json` (0,7 Ko)

## Opérations effectuées (commit `581a8e9`)
1. Sauvegarde locale de l'ancien jeu : `/tmp/checkers-backup-v3-20261111-093340.tar.gz`
2. Copie des 6 fichiers du zip dans `checkers/` (remplace l'ancienne version)
3. **Sécurité anti-cache** appliquée par-dessus la v4 :
   - `sw.js` passé en stratégie **réseau d'abord** (network-first), cache renommé `checkers-master-v5`
   - `index.html` : `navigator.serviceWorker.register('./sw.js', { updateViaCache: 'none' })` + `registration.update()`
4. Vérification syntaxe (`node -c`) : OK
5. Commit + push sur `ucfzem.github.io`, puis vérification live

## Vérification live (GitHub Pages)
- `game.js` : HTTP 200, 45 234 octets (v4) ✅
- `index.html` : HTTP 200, 6 194 octets, contient « Dames espagnoles » + `updateViaCache` ✅
- `sw.js` : `checkers-master-v5` ✅
- `style.css` : HTTP 200, 12 750 octets ✅
- `ai.js` : HTTP 200, 4 326 octets ✅
- `manifest.json` : HTTP 200, 720 octets ✅

## Fonctionnalités de la v4
- Thèmes clair / sombre persistants
- Mode **Dames espagnoles** (plateau inversé, captures arrière, dames volantes)
- IA 5 niveaux, mode 2 joueurs local, mode en ligne (PeerJS)
- Annulation de coup, sons, PWA hors-ligne

## Commit
- `581a8e9` — `feat(checkers): remplace par Checkers Master v4 (thèmes + dames espagnoles) + SW réseau-d'abord v5`

## Liens
- Jeu : https://ucfzem.github.io/checkers/
- Code : https://github.com/ucfzem/ucfzem.github.io/tree/main/checkers
- HTML : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/index.html
- JS : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/game.js
- IA : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/ai.js
- CSS : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/style.css
- SW : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/sw.js
- Manifest : https://github.com/ucfzem/ucfzem.github.io/blob/main/checkers/manifest.json
- Backup conversation : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-11-checkers-v4.md
