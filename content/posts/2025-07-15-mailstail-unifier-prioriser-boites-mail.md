---
title: mailstail ou comment unifier et prioriser vos boîtes mail
date: "2025-07-15T23:38:31+00:00"
lastmod: "2026-02-06T17:06:10+00:00"
draft: false
description: "Jongler entre plusieurs boîtes mail devient vite un enfer. J'ai développé mailstail, une interface légère qui centralise Gmail, Yahoo, IMAP et autres, avec un scoring automatique pour prioriser l'important. Un PoC open source pour reprendre le contrôle de sa messagerie."
tags:
  - client mail intelligent
  - Docker
  - Exchange
  - Gmail
  - IMAP
  - NLP
  - OAuth2
  - open source
  - prototype
  - Python FastAPI
  - React Vite
  - spaCy
categories:
  - PoC
author: githubcopilotchallenge
image: /images/1/featured.webp
image_formats:
  - /images/1/featured.webp
  - /images/1/featured.avif
---

J’ai récemment trouvé, quelque part sur le web, des infos concernant [gmailtail](https://github.com/c4pt0r/gmailtail) – un petit outil malin capable d’extraire au format JSON les courriels reçus dans votre boîte Gmail.

Mais cet outil est limité à Gmail. Or moi, ce que je voulais, c’était un **client mail intelligent** transversal : Gmail, Yahoo, Exchange, Free, Orange, ou tout simplement ma boîte mail perso en IMAP, hébergée chez un fournisseur classique.

Alors j’ai posé mon idée.

<figure class="wp-block-image size-large">![](/images/1/019a2f73-a181-7dcc-bcdf-050d0bb51814_0-683x1024.webp)</figure>Puis j’ai commencé à prototyper un outil, que j’ai appelé **mailstail**. Une **interface mail multi-comptes** légère, intelligente, capable de me montrer en un coup d’œil ce qui est important… ou pas.

## Objectifs fonctionnels

Ce que je voulais pour cette **interface mail multi-comptes**, c’était très simple (du moins en apparence) :

- voir les derniers courriels reçus (toutes boîtes confondues)
- **prioriser les emails** automatiquement (important / moyen / faible)
- répondre directement depuis l’interface
- supprimer un indésirable en un clic
- éventuellement : proposer une réponse automatique (LLM) ou un résumé intelligent

## Panorama des solutions existantes

Aujourd’hui, si vous consultez vos mails, vous avez plus ou moins deux choix :

- soit vous utilisez l’interface web du fournisseur (Gmail, Outlook, Free, etc.)
- soit vous passez par un **client mail** lourd comme Thunderbird ou Outlook

Mais dès que vous avez plus d’une boîte mail, ça devient un enfer :

J’ai une adresse perso, une autre “officielle” (fournisseurs de services, impôts, CAF, avocat…), une adresse universitaire, une adresse pro… et chaque jour, je dois passer d’un onglet à l’autre, ou d’un compte à l’autre.

Soit je passe d’onglet en onglet dans un navigateur web, en ayant devant les yeux une masse informe de courriels venus d’ici et d’ailleurs, soit je consulte mon application (Thunderbird), mais dans chaque dossier, j’ai la même masse informe de courriels à traiter. Et en plus, je ne pense pas forcément à consulter chaque dossier.

Autant dire que la découverte de cet outil m’a interpelé, et que j’ai eu envie de développer une **interface mail intelligente** qui me permette d’optimiser mon temps de consultation.

Je passe :

- d’une masse informe de mails dans tous les sens,
- sans aucun repère visuel,
- sans aucune aide à la décision

à :

- une information structurée,
- une visualisation rapide des messages importants
- avec une possibilité d’interagir en temps réel

## Les outils d’analyse automatique pour emails

En creusant, j’ai découvert plusieurs briques technologiques pour le **scoring automatique** des emails :

- **Lettria** : puissante, mais un peu usine à gaz pour un besoin simple
- **spaCy** : rapide, fiable, personnalisable (et open source)
- **CamemBERT** : efficace, mais plus lourd (Transformers, GPU, RAM…)
- **Mailparser.io** : intéressant pour des formulaires ou flux structurés

Mon choix : rester léger avec **spaCy NLP**, en intégrant un système de **scoring pondéré** basé sur les mots-clés et les entités nommées.

## Architecture globale

Un seul mot d’ordre : simplicité et robustesse.

<div class="wp-block-merpress-mermaidjs diagram-source-mermaid">```
graph TD
  A["Récupération IMAP/API"] --> B["Scoring spaCy"]
  B --> C["Affichage frontend (React)"]
  C --> D["Réponse manuelle ou auto"]
  C --> E["Suppression / action"]
```

</div>## Choix techniques initiaux

Le **backend** est construit en **Python**, un langage que je maîtrise bien et qui offre un excellent écosystème pour tout ce qui est traitement de texte, NLP et **gestion d’emails** (IMAP, encodage, parsing multipart…). C’est aussi un langage bien documenté, idéal pour construire une base robuste et évolutive.

Pour la couche API, j’ai opté pour **FastAPI**. Pourquoi FastAPI et pas Flask ? D’abord pour la vitesse de développement, ensuite pour la détection automatique des types, la documentation Swagger intégrée, et enfin parce qu’il est conçu pour du asynchrone – ce qui me permettra plus tard de scaler ou de passer à du multi-thread sans tout refaire.

Côté **frontend**, le choix s’est porté sur **React** pour sa large communauté et sa souplesse. Associé à **Vite** (plutôt que Webpack), le combo permet une compilation rapide, un rechargement ultra-fluide en développement, et une **interface responsive** moderne mais légère. J’avais aussi envisagé Vue, mais React reste à mes yeux plus robuste et plus maintenu à long terme.

Enfin, **Docker** s’est imposé pour encapsuler le tout. Avec **Docker Compose**, je peux builder, lancer, déployer et mettre à jour l’ensemble du projet (frontend + backend + proxy) en une seule ligne. Ce n’était pas indispensable au début, mais c’est vite devenu essentiel pour tester sur différents environnements.

## Architecture technique détaillée

Un seul mot d’ordre : simplicité et robustesse.

**Récupération IMAP/API → Scoring spaCy → Affichage frontend (React) → Réponse manuelle ou auto → Suppression / action**

- **IMAP + OAuth2** pour compatibilité avec Gmail, Yahoo, etc.
- Fallback avec mot de passe (Free, Orange, etc.)
- Affichage **multi-comptes mail**, avec icône colorée selon la source
- **Priorisation automatique** par niveau d’importance
- Logs des mails analysés pour audit ou rétroaction

## Détails techniques

L’un des enjeux dès le départ était de faire tourner ce projet même sur des configurations modestes (ex. : VPS à 1 vCPU, 512 Mo de RAM). Il fallait donc éviter les solutions trop lourdes ou trop exigeantes.

Côté backend, j’ai préféré un traitement séquentiel simple au départ, plutôt que d’implémenter de l’asynchrone ou du multithread trop tôt. **FastAPI** permet de faire du async proprement, mais encore fallait-il ne pas compliquer inutilement la logique de traitement dès la première version. La structure actuelle pourrait facilement basculer vers du async ou de la file de tâches plus tard, mais ce n’est pas une priorité à ce stade.

Pour les **connexions IMAP**, j’ai intégré un gestionnaire de session capable de distinguer les connexions **OAuth2** (pour Gmail, Outlook, Yahoo…) des connexions par mot de passe applicatif plus classiques. Le principe est simple : si les variables d’environnement contiennent un **token OAuth2** valide, on utilise ce mode d’authentification, sinon, on revient à une authentification classique. Le backend est donc capable d’ajuster dynamiquement sa méthode d’authentification sans que l’utilisateur n’ait à faire le moindre choix manuel.

Le **parsing des emails** a nécessité pas mal de soin pour gérer toutes les variantes : encodage bizarre, contenus multipart avec pièces jointes, HTML crades, etc. Là encore, un traitement robuste mais léger était le bon compromis.

Côté **scoring intelligent**, spaCy est rapide, mais il a fallu calibrer la pondération pour obtenir des résultats pertinents sans tomber dans le sur-apprentissage ou le scoring gadget. J’ai aussi loggué tous les mails traités dans un fichier .jsonl pour pouvoir analyser et ajuster le scoring a posteriori.

Sur le **frontend React**, React m’a permis de découper proprement l’affichage des messages (aperçu, détail, réponse), et d’ajouter facilement des fonctionnalités interactives (bouton « Répondre », champ rich text caché par défaut, priorisation par couleur). Le choix de **Vite** a permis un cycle de développement très fluide et des builds rapides, même sur machine modeste.

### Backend FastAPI

Le **backend FastAPI** est le cœur du traitement de l’application. Il est conçu pour recevoir des requêtes REST via FastAPI, traiter les mails (connexion IMAP ou via API), extraire leur contenu, appliquer un **scoring automatique** et renvoyer les données sous forme de JSON au frontend.

**FastAPI** a été choisi pour :

- sa rapidité de développement,
- son support natif de l’asynchrone (utile pour gérer plusieurs **connexions IMAP** en parallèle à terme),
- sa documentation automatique (Swagger UI très utile en développement),
- et sa très bonne intégration avec Pydantic pour la validation des modèles.

Les **routes principales** exposées sont :

- **GET /api/messages** : récupère les mails triés et scorés
- **POST /api/reply** : envoie une réponse à un mail donné

Le backend est également responsable de :

- l’**authentification OAuth2** (avec refresh automatique),
- la **gestion multi-comptes**,
- l’**analyse des emails** avec spaCy (entités + mots-clés pondérés),
- le logging dans un fichier JSONL (audit, débogage, entraînement futur).

Il est conçu pour tourner dans un conteneur **Docker** isolé, avec son modèle spaCy préchargé et monté via volume.

Fonctionnalités principales :

- **Authentification dynamique** selon les credentials disponibles (mot de passe ou OAuth2)
- **Parsing robuste** des mails (multipart, charset, encodage…)
- **Scoring pondéré** (keywords + entités via spaCy)
- Exposition des mails via **API REST** /api/messages
- Support de la réponse via /api/reply

#### Logging JSONL pour le scoring

Chaque mail analysé est loggué sous forme d’un objet JSON compact, stocké ligne par ligne dans un fichier `.jsonl`. Cela permet :

- de conserver un historique léger et exploitable,
- de recalculer le scoring avec de nouveaux paramètres,
- de visualiser les entités détectées et le niveau de priorité attribué,
- d’entraîner éventuellement un modèle de classification supervisée à l’avenir.

Chaque entrée comprend typiquement : `timestamp`, `expediteur`, `sujet`, `score_final`, `entites`, `mots_cles_detectes`, `id_message`.

### Frontend React

Le **frontend React** repose sur React, avec **Vite** comme outil de compilation. L’objectif était d’avoir une **interface légère** à charger, réactive, sans fioritures.

Les choix structurants :

- utilisation de **composants React** légers,
- chargement progressif des contenus (aperçu → détail → réponse),
- intégration de **Quill.js** pour le champ de réponse en rich text,
- design **responsive** et compact pour pouvoir afficher l’essentiel même sur petit écran.

Parmi les éléments notables de l’**interface mail** :

- un indicateur coloré : 🔴 🟠 ⚪ pour la priorité,
- une icône indiquant le compte d’origine (**Gmail**, **Yahoo**, etc.),
- une gestion des mails par aperçu, avec les 3 premières lignes visibles avec un bouton ou un lien « développer / replier »,
- un bouton “Répondre” qui déclenche le chargement conditionnel de **Quill.js** (non monté par défaut, pour ne pas alourdir le DOM).

L’ensemble communique avec le backend via fetch sur les routes **/api/messages** et **/api/reply**. Les erreurs de token, de quota ou de format de mail sont gérées proprement côté interface.

Fonctionnalités principales :

- Liste des mails avec code couleur + source visuelle
- Affichage des 3 premières lignes avec bouton « Voir la suite »
- Champ de réponse rich text intégré (**Quill.js**)
- **Composants réactifs** et légers (Vite)

### Docker &amp; Nginx

Avant de choisir **Nginx**, j’ai pris le temps d’évaluer ce qu’offraient Apache et OpenLiteSpeed. Apache, aurait pu faire l’affaire. Mais il est plus lourd, plus verbeux, et nécessite plus de configuration pour un simple **reverse proxy**.

Quant à OpenLiteSpeed, [que je connais bien](https://tsw.ovh/cyberpanel-openlitespeed-lscache/), il est rapide et moderne, mais sa configuration en container est un peu capricieuse, surtout quand il s’agit d’un projet découpé en microservices.

Finalement, j’ai choisi **Nginx** pour sa simplicité, sa stabilité, et sa documentation ultra-complète. Il s’est imposé comme le bon choix pour :

- Servir le **frontend React** compilé,
- Proxifier les requêtes vers /api/ vers **FastAPI**,
- Offrir un point d’entrée unique (idéal pour la gestion des CORS et des certificats SSL).

Il est intégré au projet via **Docker Compose**, avec une configuration minimale mais robuste, et peut être remplacé ou ignoré si l’on déploie sur une stack différente (par exemple un hébergement managé avec son propre reverse proxy).

### Gestion du rafraîchissement des tokens OAuth2

L’un des points cruciaux avec **OAuth2**, c’est que le **token d’accès** fourni par le provider (comme **Google** ou **Yahoo**) a une durée de vie limitée – souvent autour d’une heure. Pour éviter de devoir redemander une autorisation à l’utilisateur à chaque fois, il faut donc gérer un **refresh token**.

Dans **mailstail**, j’ai mis en place un mécanisme automatique dans le backend :

- Le **token d’accès** est vérifié à chaque appel.
- Si le token est expiré, une requête est faite vers le serveur **OAuth2** (via requests.post) pour obtenir un nouveau token d’accès à partir du **refresh token**.
- Les nouveaux tokens sont mis à jour dans un petit fichier de cache sécurisé, de manière transparente pour l’utilisateur.

Cela permet de maintenir une connexion stable et continue, sans intervention manuelle.

## Défis rencontrés &amp; solutions apportées

<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Défi</th><th>Solution</th></tr></thead><tbody><tr><td>Rafraîchissement des **tokens OAuth2**</td><td>Automatisé dans backend</td></tr><tr><td>**Parsing HTML/texte** complexe</td><td>Fallback vers contenu brut</td></tr><tr><td>Calibrage du **scoring**</td><td>Pondération progressive selon entités</td></tr><tr><td>Quotas **Gmail API**</td><td>Fallback **IMAP** natif sécurisé</td></tr><tr><td>**UI lisible** malgré la complexité</td><td>Composants **React** modulaires + progressive disclosure</td></tr></tbody></table>

</figure>## Perspectives d’évolution

- Moteur de règles personnalisables (par l’utilisateur)
- Envoi direct depuis l’**interface mail**
- Résumés + propositions de réponse via LLM (optionnel)
- Multilingue
- Portage mobile + notifications push

## Pourquoi aucune solution “clé en main” ne suffisait ?

Parce qu’aucune ne proposait à la fois :

- une **interface ultra-légère** et rapide
- le support **multi-comptes IMAP**
- un **scoring intelligent** mais lisible
- un **backend open-source** personnalisable

## Conclusion

Ce projet m’a permis de concilier plusieurs choses :

- mes besoins d’utilisateur exigeant
- mes compétences techniques (**Python**, **NLP**, **React**)
- mon envie de rester maître de mes données

Et surtout : il m’a fait **gagner du temps**.

📦 Le projet est libre, sous licence MIT.
🔗 \[ Dépôt GitHub : <https://github.com/pcescato/mailstail> \]
💬 Suggestions bienvenues, issues ouvertes !

Tu veux toi aussi reprendre le contrôle sur ta messagerie ? Alors bienvenue dans **mailstail**.