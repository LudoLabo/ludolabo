# LudoLabo — contexte pour Claude

Site vitrine de **LudoLabo**, créateur de contenu « jeux de société » (YouTube @LudoLabo, TikTok/Instagram @ludolabo_).
DA : laboratoire scientifique × jeu de société (papier millimétré, éprouvettes, tableau périodique des catégories, tampons de verdict).
Projet développé par Pierre (GitHub : Enixas) pour son ami. Répondre en français.

## Stack
- Site **100 % statique** : HTML/CSS/JS vanilla, aucune dépendance, aucun build. S'ouvre en double-cliquant `index.html`.
- Hébergement : **Netlify** relié au dépôt GitHub `Enixas/ludolabo` (branche `main`), redéploiement automatique à chaque push. URL : https://ludolabo.netlify.app
- Les données sont des fichiers `.js` qui définissent des variables globales (`window.LUDOLABO_*`), pas du JSON, pour fonctionner aussi en `file://`.

## Fichiers
| Fichier | Rôle | Qui le modifie |
|---|---|---|
| `data/config.js` | Liens réseaux, email, photo, catégories (tableau périodique) | Humain |
| `data/youtube.js` | Vidéos YouTube (longues + Shorts) | **Robot uniquement — ne jamais éditer à la main** |
| `data/videos.js` | Ajouts manuels TikTok/Insta, corrections ou masquage de vidéos YouTube (`{ lien, titre }`, `cache: true`) | Humain |
| `data/rapports.js` | Fiches « rapport d'expérience » par jeu | Humain |
| `js/main.js` | Tout le rendu (pages identifiées par `<body data-page>`), fusion auto + manuel, lien auto vidéo→rapport si le nom du jeu est dans le titre | Dev |
| `css/style.css` | Design, tokens en `:root`, mode sombre auto | Dev |
| `scripts/sync-youtube.mjs` | Robot de synchro (Node ≥ 18, sans dépendance). Avec `YT_API_KEY` : toute la chaîne via YouTube Data API ; sans : flux RSS (15 dernières) | Dev |
| `.github/workflows/sync-youtube.yml` | Lance la synchro toutes les 3 h et commit `data/youtube.js` | Dev |

Les pages HTML partagent le même header/footer (dupliqués dans chaque fichier) : penser à modifier les 6 pages.

## Règles de travail
- **Toujours `git pull` avant de modifier** : le robot commit régulièrement `data/youtube.js`.
- Tester en local : ouvrir `index.html`, ou `npx serve .` / extension Live Server.
- Tester la synchro en local : `node scripts/sync-youtube.mjs` (mode RSS) ou `YT_API_KEY=… node scripts/sync-youtube.mjs`. Ne jamais commiter la clé : elle est dans les secrets GitHub (`YT_API_KEY`).
- Les fiches de `data/rapports.js` sont encore des **exemples** (Azul, Cascadia…) à remplacer par de vrais avis.
- Mobile first : vérifier à 390 px de large, sans scroll horizontal.
