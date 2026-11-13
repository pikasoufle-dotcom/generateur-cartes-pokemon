# 🎴 Générateur de Cartes Pokémon

Application web autonome (un seul fichier HTML) pour créer, personnaliser et exporter des cartes Pokémon façon TCG — avec effet holographique, retournement 3D, génération d'image par IA, et galerie locale.

---

## ✨ Fonctionnalités

- **Deux types de cartes** : Pokémon (PV, attaques, faiblesse/résistance/retraite) et Dresseur (effet libre).
- **Trois sources d'image** :
  - ✨ Génération par IA (Pollinations)
  - 🎨 Illustration officielle (PokeAPI / official-artwork)
  - 📁 Image personnelle (compressée en JPEG, max 800×800)
- **Effets holographiques** : Regular (arc-en-ciel), Cosmos, Galaxy — avec parallaxe à la souris.
- **Retournement 3D** de la carte au clic (face avant / dos officiel).
- **Export PNG** haute résolution (×2) via `html2canvas`.
- **Galerie locale** : sauvegarde, chargement, duplication, suppression, filtres (type de carte, type élémentaire) et tris (date, nom, type).
- **PV automatiques** récupérés depuis PokeAPI, écrasables manuellement.

---

## 🚀 Déploiement sur GitHub Pages

1. **Renomme le fichier** en `index.html` (indispensable pour que GitHub Pages le serve à la racine).
2. Pousse-le sur la branche `main` de ton dépôt.
3. Va dans **Settings → Pages**.
4. *Source* : `Deploy from a branch` → branche `main` → dossier `/ (root)` → **Save**.
5. Attends ~1 minute, puis ouvre :
   ```
   https://TON_PSEUDO.github.io/NOM_DU_REPO/
   ```

### Alternatives rapides (sans configuration)

- **htmlpreview** :
  ```
  https://htmlpreview.github.io/?https://github.com/TON_PSEUDO/NOM_DU_REPO/blob/main/index.html
  ```
- **raw.githack** :
  ```
  https://raw.githack.com/TON_PSEUDO/NOM_DU_REPO/main/index.html
  ```

---

## 🖥️ Utilisation locale

Aucune compilation, aucun serveur requis : ouvre simplement `index.html` dans un navigateur moderne.

> ⚠️ Certaines API (PokeAPI, Pollinations, html2canvas via CDN) requièrent une connexion Internet. Sans réseau, la génération IA et les PV automatiques échoueront, mais les cartes manuelles et l'image importée restent fonctionnelles.

---

## 🔌 Dépendances externes (chargées à la volée)

| Service | Usage | URL |
|---|---|---|
| html2canvas | Export PNG | `cdnjs.cloudflare.com` |
| PokeAPI | PV et illustration officielle | `pokeapi.co` |
| PokeAPI Sprites | Artwork officiel (fallback) | `raw.githubusercontent.com/PokeAPI/sprites` |
| Pollinations | Génération d'image IA | `image.pollinations.ai` |
| Pokémon TCG | Dos de carte | `images.pokemontcg.io` |

Toutes les ressources sont en **HTTPS** — pas de blocage mixed-content.

---

## 💾 Stockage local

Les cartes sauvegardées sont conservées dans `localStorage` sous la clé `pokemon-cards-saved`.

- **Quota** : ~5 Mo par origine (navigateur).
- Les images importées sont encodées en **base64** dans le JSON → une dizaine de cartes avec image uploadée suffit à saturer le quota. Un message d'erreur explicite s'affiche alors.
- ⚠️ **Les cartes ne sont pas partagées entre origines** : une carte sauvegardée en local (`file://`) n'apparaîtra pas sur la version GitHub Pages, et inversement. Changer d'URL = galerie vide.

---

## 📝 Notes techniques

### Compression d'image
Les fichiers importés sont redimensionnés côté client via `<canvas>` (max 800 px, JPEG qualité 0.85) avant stockage.

### PV automatiques
Lors d'une génération Pokémon, le champ PV est rempli depuis `stats.hp.base_stat` de PokeAPI. Toute saisie manuelle désactive ce mode (`dataset.auto = '0'`). Si le nom change entre génération et sauvegarde, les PV ne sont pas hérités (protection contre les valeurs obsolètes).

### Export PNG — correctif recommandé
`html2canvas` ne gère pas `backface-visibility: hidden`. Le dos de la carte peut se retrouver superposé à la face avant à l'export. Ajouter dans le CSS :

```css
.pokemon-card.is-exporting .card__back { display: none !important; }
```

L'état `is-exporting` est déjà géré dans le script — c'est un simple ajout CSS.

---

## 🌐 Compatibilité

Testé sur les navigateurs modernes (Chrome, Firefox, Edge, Safari récents). 
Nécessite : `fetch`, `canvas.toDataURL`, `CSS 3D transforms`, `localStorage`.

---

## 📄 Licence

Projet personnel à but créatif. 
Pokémon et les cartes officielles sont des marques de Nintendo / Creatures Inc. / GAME FREAK. 
Ce projet n'est affilié ni à Nintendo ni à The Pokémon Company.
