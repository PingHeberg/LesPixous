# CLAUDE.md — Les Pixous

Guide pour Claude (et tout contributeur) qui travaille sur ce dépôt. Tout est rédigé en français, comme le reste du projet.

## ⚠️ À savoir avant tout

**Ce dépôt est la vitrine du projet, pas son code source.**

- Le jeu lui-même (un fichier `.html` unique d'environ 8 700 lignes) **n'est pas versionné ici**. Il est distribué à part (Discord, store PingHeberg).
- Le dépôt contient : la documentation (`README.md`), les captures d'écran et un catalogue de GIFs « concepts graphiques ».
- Rien dans le dépôt ne porte le nom « Pixous Survival ». Si une tâche porte sur ce mode ou sur le code du jeu, **demander le fichier source** plutôt que de deviner son contenu.
- Ce qui suit sur l'architecture du jeu vient du `README.md`. C'est la référence tant que le code n'est pas dans le dépôt.

## Le projet en bref

Les Pixous est un **overlay Twitch interactif en pixel art** conçu pour OBS. Chaque viewer qui écrit dans le chat fait apparaître un petit personnage (un « Pixou ») qui vit sa vie : il mange, pêche, se fait des amis, se bat, gagne de l'or et participe à des événements pilotés par des commandes `!`.

- Auteur : Appo (PingHeberg). Chaîne principale : twitch.tv/hynatadj, aussi chez DrDrenelle_live.
- Version actuelle : **v2** (« plus vivants, plus intelligents »).
- Pistes futures annoncées : modes saisonniers (Noël, été), version pour d'autres streamers, **version Steam à l'étude**.
- Licence : tous droits réservés (code, sprites, concept). Ne pas ajouter de licence open source.

## Arborescence du dépôt

```
.
├── README.md                    # Documentation complète du jeu (référence fonctionnelle)
├── 75d04d3e-…-fb928.png         # Visuel d'en-tête du README (1672×941, nom auto-généré)
├── screenshots/                 # Captures en jeu, 1920×1080, une par événement/mode
│   └── raid.png, boss.png, traque.png, nuit_hantee.png, halloween.png, …
└── gifs/                        # Catalogue d'animations (concepts graphiques)
    ├── README.md                # Présentation du catalogue (HTML dans du Markdown)
    ├── index.html               # Galerie web autonome (CSS inline, sans JS)
    ├── manifest.json            # Métadonnées de chaque animation
    ├── animations/NN_nom.gif    # GIFs 288×288, fond transparent, en boucle
    ├── apercus/NN_nom_apercu.png# Image fixe de chaque GIF
    └── visuels/catalogue_NNN.png# Planches de présentation par collection
```

## Architecture du jeu (d'après le README)

| Élément | Choix technique |
|---|---|
| Langage | HTML + JavaScript pur, **aucune dépendance, aucun framework, aucun build** |
| Livrable | **Un seul fichier `.html`** (~8 700 lignes) |
| Rendu | Canvas 2D, résolution cible 1920×1080 |
| Sprites | Pixel art **dessiné en code** : des chaînes de caractères converties en pixels. Persos en **12×14 px**, agrandis (×3 à ×8) |
| Chat | WebSocket IRC Twitch **anonyme** (`justinfan`), lecture seule : tags, `USERNOTICE` (raids, subs), bits |
| Sauvegarde | `localStorage` du navigateur d'OBS : or, objets, poissons, persos, caractères, niveaux, maire, champion, bonbons, réglages |
| Réglages | Panneau intégré (**F1** ou **²**) à 5 onglets : En direct, Temps, Systèmes, Réglages, Viewers |
| Serveur | Aucun. Pas de bot, pas de token, pas de compte |

### Concepts clés du code

- **Système d'« actes »** : chaque activité est un petit scénario (aller quelque part → faire l'action → finir) qui peut être **interrompu proprement** par un événement. Toute nouvelle activité doit suivre ce modèle.
- **Un seul grand événement à la fois.** Hors événement, le Pixou obéit tout de suite aux commandes. `!stop` arrête tout, toujours.
- **Événement auto** : si le chat ne fait rien, un événement v2 se lance tout seul (~35 min par défaut, réglable de 10 à 120 min).
- **Besoins** : faim, soif, énergie, fun. **8 personnalités** (farceur, gourmand, sportif, rêveur, timide, bagarreur, curieux, fêtard) qui modifient le comportement.
- **Mémoire des viewers** : XP, niveaux (badges bronze/argent/or aux niveaux 5/10/25), séries de jours, titres (Maire, Champion, Roi/Reine d'Halloween).
- **Cycle de vie** : arrivée en parachute au premier message, départ après 10 min de silence (paramètre `afk`), données conservées.
- **Droits** : viewers / modos / streamer seulement (`!resetall`, démon, etc.).
- **Saisons** : Halloween s'active tout seul du 15 octobre au 2 novembre (forçable par `!halloween on/off/auto/fin`).
- **Monde** : jour/nuit à l'heure réelle, météo (soleil, pluie, neige, orage), terrains (herbe, sable, pierre, neige, lave, nuit, aucun).

### Configuration (en haut du fichier du jeu)

```js
const DEFAULT_CHANNEL = 'machaine';   // chaîne Twitch en minuscules
const PARTNERS = [ ... ];             // partenaires (facultatif, pigeon partenaire)
const DISCORD_LINK = 'discord.gg/xxxx';
const SITE_LINK = '';
```

C'est **la seule zone qu'un streamer doit modifier**. Ne rien ajouter qui oblige à toucher au reste du fichier pour installer le jeu.

## Lancer le jeu

### Dans OBS (usage réel)

1. Renseigner `DEFAULT_CHANNEL` (et les liens) en haut du fichier `.html`.
2. OBS : **Sources → + → Navigateur → Fichier local**, 1920 × 1080, placé en bas de la scène.
3. Clic droit sur la source → **Interagir** → **F1** pour ouvrir le panneau.
4. Écrire dans le chat : le Pixou descend en parachute.

### Dans un navigateur (test local)

Ouvrir le fichier directement, en passant les options dans l'adresse :

```
pixous.html?channel=machaine&scale=6&afk=15&ground=40&partners=0
```

| Paramètre | Effet |
|---|---|
| `channel` | chaîne Twitch à écouter |
| `scale` | taille des Pixous |
| `afk` | minutes de silence avant le départ |
| `ground` | décalage vertical du sol |
| `partners=0` | désactive le pigeon partenaire |

Astuce : la sauvegarde est dans le `localStorage` du navigateur utilisé. Un test dans Chrome ne partage pas les données d'OBS. Vider le stockage du site pour repartir de zéro.

### La galerie de GIFs

```bash
cd gifs && python3 -m http.server 8000
```

Puis ouvrir `http://localhost:8000/`. On peut aussi ouvrir `gifs/index.html` directement : la page n'a ni JS ni dépendance.

## Conventions

### Langue et ton
- **Tout en français** : README, commentaires, commandes de chat (`!peche`, `!caractere`…), textes affichés, messages de commit.
- Les commandes de chat s'écrivent **sans accents ni majuscules** (`!fantome`, `!deguisement`, `!donneror`) pour être faciles à taper.
- Le ton du README est familier et vivant (tutoiement, emojis en tête de section). Le garder.

### Documentation
- Le `README.md` est la **source de vérité fonctionnelle**. Toute nouvelle commande, événement, perso ou réglage doit y être ajouté dans la bonne section (commandes viewers, commandes modos, panneau F1, raccourcis…), avec le même format de tableau.
- Mettre à jour le **sommaire** du README si une section est ajoutée.
- Chaque gros événement a sa capture dans `screenshots/` (1920×1080, nom court en minuscules sans accent, ex. `nuit_hantee.png`), référencée dans la galerie du README.

### Catalogue de GIFs (`gifs/`)
- Nommage : `NN_nom` en minuscules, avec `_`, sans accents (`01_blob_violet`, `08_dinosaure`). Le numéro continue la série.
- Pour chaque nouvelle animation, ajouter **quatre choses** :
  1. `animations/NN_nom.gif` (288×288, fond transparent, en boucle)
  2. `apercus/NN_nom_apercu.png`
  3. une entrée dans `manifest.json`
  4. une ligne dans `gifs/README.md` **et** une carte dans `gifs/index.html`
- Champs du manifeste : `id`, `nom`, `animation`, `categorie`, `description`, `fichier`, `apercu`, `largeur`, `hauteur`, `frames`, `duree_ms`, `fond_transparent`, `boucle`, `statut`. Mettre aussi à jour `version_catalogue` et `mise_a_jour` (format `AAAA-MM-JJ`).
- `statut: "concept_graphique"` : ces GIFs **ne remplacent pas** les sprites du jeu (qui sont dessinés en code). Ne pas les présenter comme des assets du jeu.
- Pixel art : toujours `image-rendering: pixelated` à l'affichage.
- **Ne pas committer d'archives `.zip`** (elles ont déjà été retirées de l'historique récent).

### Code du jeu (quand il sera dans le dépôt)
- Zéro dépendance, zéro build, zéro serveur : ne pas introduire de npm, de bundler ou de bibliothèque externe.
- Un seul fichier HTML autonome. Les sprites restent en code (chaînes → pixels), pas en images externes.
- Toute nouvelle clé de `localStorage` doit avoir son bouton de remise à zéro dans l'onglet « Réglages » du panneau F1.
- Toute nouvelle fonction doit pouvoir se couper (onglet « Systèmes ») et se régler (onglet « Temps ») sans toucher au code.
- Un nouvel événement doit : respecter la règle « un seul à la fois », s'arrêter sur `!stop` et `!<evenement> stop`, avoir son bouton dans l'onglet « En direct » et, si possible, un raccourci clavier libre.

### Git
- Messages de commit en français, à l'infinitif ou au présent (« Ajout de 4 nouveaux Pixous animés au catalogue »).
- Les gros binaires (PNG d'en-tête de 3,6 Mo) sont versionnés directement, sans Git LFS.
