# 👾 Les Pixous

**Un monde en pixel art qui prend vie avec ton chat Twitch.**

Chaque viewer qui écrit dans le chat fait apparaître son petit personnage en parachute en bas de l'écran. Il vit sa vie tout seul (il mange, pêche, danse, se fait des amis, se bagarre, gagne de l'or), il se souvient des gens, il réagit à ce qui se dit… et il participe à des événements que le chat contrôle avec des commandes `!`.

Un seul fichier HTML, zéro installation, zéro serveur : une source navigateur dans OBS et c'est parti.

> Créé par **Appo** (PingHeberg) pour la chaîne [twitch.tv/hynatadj](https://www.twitch.tv/hynatadj), où ils tournent en direct.
> Aussi en service chez [DrDrenelle_live](https://www.twitch.tv/drdrenelle_live).

---

## 💡 L'idée

HynataDj streame **sans micro**. Pas de voix, pas de face cam : tout l'échange avec le chat passe par l'écrit. Il fallait donc quelque chose qui rende le chat **visible, vivant et amusant**, pour que chaque message compte et que les viewers aient une raison de rester, de revenir et de parler entre eux.

Les Pixous sont nés de ça : au lieu d'un simple overlay de chat, un **petit monde** où chaque viewer a un corps, un caractère, un inventaire, des amis, une histoire… et où le streamer est un personnage parmi les autres.

Principes de départ, jamais abandonnés :

- **Zéro friction** : un fichier `.html`, pas de bot à héberger, pas de token, pas de compte. Connexion anonyme au chat Twitch en lecture seule.
- **Le chat joue, le streamer regarde** : tout se déclenche par des commandes, les modos lancent les événements, le streamer n'a rien à cliquer en plein jeu.
- **Rien n'est perdu** : or, objets, poissons, caractère, niveau… tout est sauvegardé dans le navigateur d'OBS.
- **Ça doit vivre tout seul** : même quand personne ne parle, il se passe quelque chose à l'écran.

---

## 🛠️ Comment c'est fait

| | |
|---|---|
| **Langage** | HTML + JavaScript pur, aucune dépendance, aucun framework |
| **Rendu** | Canvas 2D, sprites pixel art dessinés en code (chaînes de caractères → pixels) |
| **Chat** | WebSocket IRC Twitch anonyme (`justinfan`), lecture des tags, des USERNOTICE (raids, subs) et des bits |
| **Sauvegarde** | `localStorage` du navigateur OBS |
| **Réglages** | Panneau intégré (touche **F1**) : activer/désactiver chaque fonction, fréquence des événements, taille, terrain, météo |
| **Taille** | ~8 500 lignes dans un seul fichier |

Les personnages sont des **sprites 12×14 pixels** animés (marche, saut, danse, sommeil, combat, pêche, parachute…), avec un système d'« actes » : chaque activité est un petit scénario (aller quelque part → faire quelque chose → finir) qui peut être interrompu proprement par un événement.

---

## 📜 Les étapes, de la v1 à la v2

### Étape 1 : les avatars (le tout début)
Un viewer écrit → un perso apparaît et marche. Pseudo au-dessus de la tête, couleur du chat Twitch, disparition après 10 min de silence. Premières commandes : `!perso`, `!coucou`, `!jump`, `!danse`, `!dodo`.

### Étape 2 : la vie
- Faim et soif, nourriture à acheter (`!faim`, `!soif`, `!acheter`, `!boutique`)
- L'**or** comme monnaie de tout le reste (`!or`, `!top`)
- Pêche dans l'étang avec 7 poissons dont le doré (`!peche`, `!poissons`)
- Potager, cabane à construire ensemble, bain, fusées qui explosent une fois sur deux
- Un chat, des papillons, des lucioles, le jour, la météo réelle (pluie, neige, orage)
- Amitiés et rivalités : les Pixous se parlent (45+ dialogues avec mimiques), se font des câlins, des high five… ou se regardent de travers

### Étape 3 : les combats et les objets
- Duels (`!duel`, `!accept`) avec 6 styles d'armes, bagarre générale
- Boutique d'objets et de familiers (balai, grimoire, chauve-souris, corbeau, œil volant…) avec leur animation propre (`!magasin`, `!equiper`)
- Le **démon**, perso réservé au streamer, prêtable à un viewer

### Étape 4 : les événements
Le chat devient un jeu :
- 🐎 Course de chevaux avec paris et cotes
- ⚽ Match de foot en deux équipes, 🎤 concert DJ, 🔥 feu de camp, 🙈 cache-cache, 🎁 cadeau du ciel, 🏠 chantier
- 🐦 Le pigeon partenaire qui fait passer les partenaires du stream
- 🏹 **La Grande Chasse** : tir à l'arc sur lapins, canards, renards, sangliers, cerfs et le sanglier d'or

### Étape 5 : les gros modes de jeu
- 🩸 **La Traque** (façon Dead by Daylight) : le streamer devient un monstre géant, les viewers réparent des générateurs, se cachent dans des casiers, se décrochent des crochets et fuient par les sorties
- 👻 **La Nuit Hantée** (façon Phasmophobia) : maison hantée, fantôme caché, 6 appareils (EMF, thermomètre, radio, UV, caméra, carnet), santé mentale, chasse, vote du bon fantôme

### Étape 6 : version publique et version DrDrenelle
Le fichier est nettoyé pour être installable par n'importe quel streamer : chaîne, partenaires, Discord et liens en haut du fichier, rien d'autre à toucher. Une version dédiée est installée chez DrDrenelle_live, avec les droits croisés entre les deux chaînes.

### Étape 7 : Halloween, le premier mode saisonnier
Actif tout seul du 15 octobre au 2 novembre : cimetière, Pixous déguisés, 4 persos de saison, bonbons, sorts, citrouille géante à sculpter, citrouille cachée, zombies, concours de costumes, bal des monstres, magasin à -50 %… et une **couronne dorée toute l'année** pour le roi ou la reine des bonbons.

### Étape 8 : les Pixous v2, « plus vivants, plus intelligents »
La refonte du cerveau :
- 🧠 **8 personnalités** (farceur, gourmand, sportif, rêveur, timide, bagarreur, curieux, fêtard) qui changent vraiment le comportement : les farceurs font des farces, les sportifs courent, les timides restent à l'écart, les bagarreurs provoquent des duels
- ⚡ **Besoins** : énergie (ils s'endorment vraiment) et fun (ils cherchent une activité)
- 🎨 **13 activités d'ambiance** : lire, téléphone, cerf-volant, jonglage, guitare, peinture, étirements, étoiles, flaques, boules de neige, pique-nique, farces, inspection
- 💬 **Compréhension du chat** : `mdr` → ils rigolent, `gg` → ils applaudissent, `<3` → des cœurs, MAJUSCULES → ils sursautent, une question → un voisin répond, `@pseudo` → ton Pixou va voir l'autre
- 💾 **Mémoire** : fête de bienvenue aux nouveaux, « tu nous as manqué » aux absents, séries de jours, **niveaux et XP** avec badges bronze / argent / or
- 🌙 **Jour et nuit à l'heure réelle** : étoiles, lanternes, aube
- 📺 **Événements Twitch** : raids (jusqu'à 15 visiteurs en parachute), subs (bannière, Pixou géant, or), bits (pluie d'or)
- 🎉 **8 nouveaux événements** : quiz à zones A/B/C, tir à la corde, boss dragon, chasse au trésor chaud/froid, tournoi de duels avec ceinture de champion, pluie de météores, invasion OVNI, élection du maire (écharpe 24 h et revenu)
- ⏰ Un événement se lance tout seul toutes les ~35 min si le chat ne fait rien

---

## 🎮 Ce que ça donne en live

Un chat de 10 personnes = 10 Pixous qui vivent, parlent, se taquinent et sautent de joie quand un sub arrive. Un modo tape `!boss` et tout le monde se met à spammer `!attaque` contre un dragon. Un nouveau arrive, les voisins viennent lui dire bonjour. Quelqu'un dit « bonne nuit » et son Pixou se fait saluer avant de partir.

Le but n'a jamais changé : **que le chat ait envie de parler.**

---

## 📦 Installation (pour les streamers qui ont le fichier)

1. Ouvre le fichier dans un éditeur de texte et remplis le haut :
   ```js
   const DEFAULT_CHANNEL = 'machaine';   // ta chaîne Twitch en minuscules
   const PARTNERS = [ ... ];             // tes partenaires (facultatif)
   const DISCORD_LINK = 'discord.gg/xxxx';
   ```
2. Dans OBS : **Sources → + → Navigateur → Fichier local**, 1920 × 1080, en bas de la scène.
3. Clique sur **Interagir** puis **F1** pour le panneau de réglages.
4. Écris dans ton chat : ton Pixou arrive en parachute.

Les commandes complètes (viewers, événements, modos, streamer) sont dans le [Wiki](../../wiki).

---

## 🗺️ Et après

- Modes saisonniers suivants (Noël, été)
- Nouveaux événements et nouveaux persos
- Version pour d'autres streamers, avec édition des réglages sans toucher au code
- Une version **Steam** est à l'étude

---

## 📄 Licence et crédits

Les Pixous sont une création originale d'**Appo** (PingHeberg). Code, sprites et concept : tous droits réservés.
Pour installer les Pixous sur ta chaîne, passe par le [Discord](https://discord.gg/NRxyeycYnQ) ou par [PingHeberg](https://store.pingheberg.com).

🛡️ Modos permanents des Pixous : [Xeryth](https://www.twitch.tv/xeryth_fox)
💜 Partenaires : [PingHeberg](https://store.pingheberg.com) · [DrDrenelle_live](https://www.twitch.tv/drdrenelle_live) · Team Pasta Box
