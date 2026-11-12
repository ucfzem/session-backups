# Tanger — correction des images de la page Histoire (Commons, era-correct)

Date : 2026-11-12  
Dépôt site : `ucfzem/tanger`  
Commit site : `9d942a4` — `histoire: replace mismatched photos, era-correct figures only`

## 1. Objectif

Corriger les images mal associées de `histoire/index.html` avec une règle stricte :

- les photos de la galerie ne peuvent être utilisées que si leur `id`, `year`, `title_en` et `src` correspondent exactement à `DEFAULT_PHOTOS` dans `index.html` ;
- une photo de galerie ne peut pas être placée sous un chapitre dont la période ne contient pas son année, sauf exceptions explicites ;
- les images anciennes doivent provenir de Wikimedia Commons, avec licence libre, description vérifiée et crédit ;
- supprimer la figure et ses 4 clés de traduction si aucun candidat ne passe ;
- ne pas toucher à `DEFAULT_PHOTOS`, à la galerie, à la navigation ou au thème.

## 2. Résultat final

- 22 figures au départ → **20 figures**.
- **12 nouvelles images Commons** dans `images/histoire/`.
- **8 photos de galerie** replacées selon `DEFAULT_PHOTOS`.
- **2 figures supprimées** :
  - `ch7.s3.fig` : seule photo disponible datée 1912+, incompatible avec le chapitre XVIIIe–XIXe ;
  - `ch10.s3.fig` : aucun candidat Commons fiable pour l’usine Renault de Tanger.
- **8 clés de traduction supprimées** (4 langues × 2 figures).
- **80/80 captions modifiées** en `ar`, `en`, `fr`, `es` (20 figures × 4 langues).
- Toutes les images Commons : JPEG, largeur ≥ 800 px, poids ≤ 250 Ko.
- Aucun changement de `DEFAULT_PHOTOS`, galerie, nav, thème ou service worker.

## 3. Photos de galerie replacées (section A)

| Emplacement | id | Année | Titre `DEFAULT_PHOTOS` | Fichier |
|---|---:|---:|---|---|
| `ch1.s1.fig` | 33 | 1970 | Phare de Charf | `images/phare-charf-1970.jpg` |
| `ch5.fig` | 14 | 1965 | Puerta del Mar | `images/puerta-del-mar-1965.jpg` |
| `ch7.fig` | 15 | 1889 | Villa Harris | `images/bab-fahs-1933.jpg` |
| `ch8.fig` | 38 | 1932 | Zone Internationale de Tanger | `images/zone-internationale-1932.jpg` |
| `ch8.s3.fig` | 5 | 1950 | Le Titan | `images/port-1930.jpg` |
| `ch8.s5.fig` | 37 | 1984 | Paul Bowles au Café Hafa | `images/paul-bowles-cafe-hafa-1984.jpg` |
| `ch9.fig` | 54 | 1960 | Rue de la Liberté | `images/rue-de-la-liberte-1960.jpg` |
| `ch11.s2.fig` | 7 | 1960 | L’Entrée de la Kasbah | `images/medina-1945.jpg` |

Exceptions assumées et indiquées dans les captions : `ch1.s1` (photo vers 1970), `ch5` (photo de 1965), `ch8.s5` (photo de 1984).

## 4. Nouvelles images Commons (section C)

| Emplacement | Fichier local | Fichier Commons | Auteur | Licence |
|---|---|---|---|---|
| `ch1.s4.fig` | `punic-amphora.jpg` | Exhibit in the archeological museum of the Iberian city of Ullastret 04.jpg | Kritzolina | CC BY-SA 4.0 |
| `ch2.fig` | `lixus-roman-theater.jpg` | Roman theater at Lixus archaeological site.jpg | Ideophagous | CC BY-SA 4.0 |
| `ch3.fig` | `great-mosque-tangier.jpg` | Tangier Grand Mosque.jpg | Bahaedinee | CC BY-SA 4.0 |
| `ch4.fig` | `tangier-medina-kasbah.jpg` | Kasbah, Tánger, Marruecos, 2015-12-11, DD 26.JPG | Diego Delso | CC BY-SA 4.0 |
| `ch4.s3.fig` | `ibn-battuta-tomb-tangier.jpg` | Morocco Tangier IbnBattuta.jpg | Boubloub | CC BY-SA 4.0 |
| `ch6.fig` | `hollar-tangier-view-1.jpg` | Wenceslas Hollar - Part of Tangier from above (State 2).jpg | Wenceslaus Hollar | Public domain |
| `ch6.s3.fig` | `hollar-tangier-view-2.jpg` | Wenceslas Hollar - Tangier from the S.E. (State 2).jpg | Wenceslaus Hollar | Public domain |
| `ch10.fig` | `tangier-contemporary-panorama.jpg` | Tangier pano - Maged.jpg | Maged | CC BY 2.0 |
| `ch10.s2.fig` | `tanger-med-port.jpg` | PortofTangerMed1.jpg | Adam Cli | CC BY-SA 4.0 |
| `ch10.s5.fig` | `tangier-beach-2017.jpg` | Plage Municipale, Tangier, November, 2017.jpg | Alexey Komarov | CC BY-SA 4.0 |
| `ch11.s4.fig` | `american-legation-tangier.jpg` | American Legation of Tangier 01.jpg | National Register Staff | Public domain |
| `book.fig` | `kasbah-museum-tangier.jpg` | Kasbah Museum of Tangier 17092021 001.jpg | Yamen | CC BY-SA 4.0 |

Chaque caption contient le crédit : `— Wikimedia Commons, <auteur>, <licence>`.

## 5. Faux positif « map » — American Legation

Le premier candidat `Museo del Antiguo Legado Estadounidense… DD 44-46 HDR.JPG` a été rejeté par le contrôle interdisant le mot `map`.

Cause exacte : la catégorie Commons **`Tone-mapped HDR images of buildings`** contient le sous-mot **`map`** dans **`mapped`**. Ce n’était donc pas un vrai problème de contenu.

Remplacement retenu :

- `File:American Legation of Tangier 01.jpg`
- auteur : National Register Staff
- licence : Public domain
- dimensions : 1368 × 1054
- fichier final : `images/histoire/american-legation-tangier.jpg` (156 404 octets)

## 6. Vérifications

### Fichiers et traductions

- `node --check` sur le script inline : OK.
- Extraction et évaluation de `T` : 4 langues, **337 clés par langue**.
- `ch7.s3.fig` et `ch10.s3.fig` absents des 4 langues et du HTML.
- 20 figures, 20 images, aucun `alt` vide.
- Captions HTML initiales = valeurs `T.ar`.
- 80/80 anciennes/nouvelles captions différentes ; différences complètes dans `/tmp/histoire-work/caption-diff.txt`.
- Toutes les images de `images/histoire/` : JPEG, ≥ 800 px, ≤ 250 Ko, fichiers non corrompus.

### Playwright 390 px

Testé sur GitHub Pages, dans `fr`, `en`, `es`, `ar` :

- 20 figures ;
- 20 images avec `naturalWidth > 0` ;
- aucun HTTP ≥ 400 ;
- aucune erreur console / `pageerror` ;
- `scrollWidth = clientWidth = 390` dans les 4 langues ;
- aucun débordement horizontal ;
- bascule de langue OK, RTL arabe OK.

### Live

- GitHub Pages : HTTP 200, page mise à jour après le commit `9d942a4`.
- Worker Cloudflare : HTTP 200, page mise à jour.
- Images Commons live : HTTP 200 avec les tailles attendues.

## 7. Points de vigilance conservés

- `ch3.fig` utilise `Tangier Grand Mosque.jpg` : Commons écrit « Grand Mosque », la caption française/anglaise utilise « Grande Mosquée / Great Mosque ».
- `ch10.fig` utilise `Tangier pano - Maged.jpg`, description « Panoramic view of Tangier, May 2015 » : panorama contemporain retenu comme vue de la ville.
- Les gravures de Hollar sont datées « Unknown date » dans les métadonnées du fichier, mais appartiennent à la série des vues de Tanger de 1669–1670 ; caption conservée selon la source historique.

## 8. Liens

- Site : https://ucfzem.github.io/tanger/
- Page Histoire : https://ucfzem.github.io/tanger/histoire/
- Worker site : https://ucfzem.azer-tyu199p.workers.dev/tanger/
- Worker Histoire : https://ucfzem.azer-tyu199p.workers.dev/tanger/histoire/
- Dépôt : https://github.com/ucfzem/tanger
- Commit : https://github.com/ucfzem/tanger/commit/9d942a4
- Dossier images : https://github.com/ucfzem/tanger/tree/main/images/histoire
- Backup conversation : https://github.com/ucfzem/session-backups/blob/main/CONVERSATION-2026-11-12-tanger-images-commons-fix.md
- Backup raw : https://raw.githubusercontent.com/ucfzem/session-backups/main/CONVERSATION-2026-11-12-tanger-images-commons-fix.md
