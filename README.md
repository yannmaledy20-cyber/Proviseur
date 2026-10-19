# Proviseur — application + multijoueur

Un seul serveur Node (sans dépendance) sert le jeu **et** le multijoueur : ligue en ligne, défis, salle des proviseurs, joueurs en ligne, applaudissements. Les codes de lycée (hors ligne) fonctionnent toujours.

## Lancer en local
    node server.js        # puis ouvrir http://localhost:3000
Ouvrez deux navigateurs (ou une fenêtre privée) pour jouer à deux.

## Mettre en ligne (obligatoire pour jouer à distance)
Le serveur doit être hébergé en HTTPS pour que vos amis s'y connectent et pour que l'installation de l'app soit proposée.
- **Render / Railway / Fly.io** : déposez ce dossier sur GitHub, créez un « Web Service » Node, commande de démarrage `node server.js`. Ajoutez un **disque persistant** monté sur `/data` et la variable `DATA_DIR=/data`, sinon la ligue est effacée à chaque redéploiement.
- **Docker** : `docker build -t proviseur . && docker run -p 3000:3000 -v proviseur-data:/data proviseur`
- **VPS** : `node server.js` derrière Caddy ou nginx (désactivez la mise en tampon pour `/api/stream`).

## Installer comme application
Ouvrez l'adresse HTTPS du jeu : Chrome/Edge proposent « Installer », Safari iOS : Partager → « Sur l'écran d'accueil ». Le jeu solo marche hors ligne.
Pour les stores (Play Store / App Store), emballez `public/` avec **Capacitor** et mettez l'URL du serveur dans `public/config.js` (`window.PROVISEUR_SERVER`).

## Sécurité intégrée
Identité anonyme par jeton secret (un joueur ne peut écrire que sur sa propre fiche), validation côté serveur, limites de débit (messages, défis, applaudissements), défis visibles seulement des deux joueurs concernés.
Variable optionnelle : `CORS_ORIGIN` pour restreindre les origines autorisées.

## Phase de test privée
Définissez `ACCESS_CODES="code-anna,code-paul,code-marc"` (un code par testeur). Sans code valide, personne n ouvre le jeu ni l API. Pour couper un testeur, retirez son code et redémarrez. Sans cette variable, l accès est libre : à utiliser au lancement officiel.
