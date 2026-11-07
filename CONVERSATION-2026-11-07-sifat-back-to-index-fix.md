# Conversation — Correction du bouton Retour à l'index

**Date :** 2026-11-07
**Projet :** sifat-al-safwa (GitHub Pages)

## Problème
Le bouton "Retour à l'index" renvoyait vers la page précédente au lieu de l'index du livre.
La cible était `#controls` (barre de recherche) au lieu de `#list` (main contenant les chapitres).

## Solution
- Cible corrigée vers `#list`.
- Lien `<a href="#list">` avec défilement fluide.
- Suppression de tout mécanisme de retour arrière (`history.back()`).

## Test
Local Playwright check after scrolling down and clicking the button:

```text
{ top: 72.96875, scrollY: 250, hash: '#list', href: '#list' }
OK true
```

## Commit site
`f54ff58` — fix: bouton retour à l'index cible #list

## Fichiers modifiés
- `sifat-al-safwa/index.html`
- `sifat-al-safwa/sw.js`
