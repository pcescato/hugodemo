---
title: "L'après-Pocket : chronique d'une migration réussie"
date: "2025-09-02T13:07:41+00:00"
lastmod: "2025-09-02T13:07:41+00:00"
draft: false
description: "Des milliers d'articles, des années de veille, et pas un e-mail de Mozilla pour prévenir. Pocket s'en allait sur la pointe des pieds, parce que tout a une fin. Les bonnes choses surtout.
"
categories:
  - Tutoriels
author: githubcopilotchallenge
---

La fermeture de Pocket ne signait pas seulement la perte d’un service pratique, mais aussi la disparition d’un véritable dépôt personnel de contenus marquants.

Bon, j’ai tout récupéré dans un fichier CSV, mais je n’avais plus ce service dont je me servais au quotidien, j’avais besoin d’une solution, rapidement.

Je me suis donc lancé dans…

## La chasse aux alternatives

### Première tentative : le Play Store

15 applications testées, toutes avec des notes supérieures à 4,3. Résultat ? Aucune ne sortait vraiment du lot. Entre interfaces datées, fonctionnalités absentes et implémentations bancales, c’était le règne de la médiocrité.

### Deuxième approche : l’IA à la rescousse

J’ai interrogé ChatGPT, Claude et Gemini avec la même question : *“Quelles sont les meilleures alternatives à Pocket ?”*

Réponse quasi-unanime : [Raindrop.io](https://raindrop.io) revenait systématiquement en tête des recommandations.

## Raindrop.io : l’alternative séduisante… mais limitée

Avec ses applis Andoid / iOS, ses extensions Chrome / Firefox, son interface claire et moderne, son organisation par catégories et par tags, la possibilité de rajouter des observations… Raindrop avait tout pour plaire. Je m’en suis servi quelques semaines, et l’expérience était plutôt fluide et agréable.

En parcourant un article tech, je découvre [Fabric](https://fabric.so/?via=pascal) et sa promesse alléchante : la possibilité d’avoir des résumés quotidiens, hebdomadaires ou mensuels générés par l’IA. Comme ils le disent sur leur site, *c’est comme une newsletter personnelle de tout ce que vous avez enregistré, créé ou capturé récemment*.

Et j’ai voulu utiliser l’API raindrop pour faire la même chose. Et là, le vernis a commencé à s’écailler. À sérieusement s’écailler.

Dès que l’on veut construire des automatisations ou intégrer ses données dans un workflow plus large, les limitations deviennent rédhibitoires. Pas de récupération globale sauf à passer par un fichier CSV, pas de filtres pour récupérer par date… C’est du “tout ou rien” par catégorie ou par tag, point barre.

## 3. Fabric.so : quand l’IA vous fait miroiter… votre porte-monnaie

Génial sur le papier. Le hic ? **4€67 par mois minimum.**

Ce n’est pas tant le prix qui pose problème – la fonctionnalité peut valoir ce tarif – mais l’accumulation. Chaque service, chaque abonnement vient s’additionner au précédent, c’est une **rente mensuelle** qui s’installe sournoisement.

Et je n’avais pas exactement ce que je voulais. Ok, je récupérais mes liens, et j’aurais eu droit à mon résumé (quotidien / hebdomadaire / mensuel). Mais c’était la seule fonctionnalité qui m’intéressait vraiment.

Je n’ai pas pris, mais l’idée des résumés automatisés m’avait séduit. Il fallait juste trouver un autre moyen de l’implémenter.

Et au fil de mes recherches, j’ai découvert…

## 4. Wallabag : la révélation open source

Dire que [Wallabag](https://fr.wikipedia.org/wiki/Wallabag) m’a séduit au premier regard serait mentir. Au premier regard, une interface minimaliste, un site plutôt dépouillé. Pas de quoi s’emballer. Mais projet open source, possibilité d’auto-hébergement… ça, ça me parlait.

Et en grattant un peu la surface, on découvre une richesse fonctionnelle impressionnante cachée sous cette simplicité apparente.

### Ce que Wallabag offre vraiment

- **une sauvegarde complète** des articles, des images, du contenu offline
- **une organisation avancée** avec des tags, des favoris, des annotations, le surlignage…
- **des applications natives** sur iOS et Android et **des extensions navigateurs** pour Chrome et Firefox, des **applis de bureau** pour Linux, Windows, MacOS…
- **une API complète** avec webhooks pour l’automatisation
- **l’auto-hébergement** permettant un contrôle total sur tes données
- **l’import depuis Pocket** et migration quasi transparente

Ironie du sort, l’application, anciennement nommée *Poche*, a dû changer de nom et de logo début 2014 à la suite d’un conflit avec l’application *Pocket*…

Et j’ai fait ce que je fais habituellement : j’ai testé. En commençant par…

### L’installation avec Docker

Pour l’expérimentation, j’ai choisi **de déployer Wallabag avec Docker** pour assurer reproductibilité et isolation. L’idée est de l’installer **en duo avec n8n**, avec **Traefik comme reverse proxy** et **PostgreSQL pour les bases de données** Wallabag et n8n.

Première opération, tu crées un répertoire pour ton application :

```
mkdir /root/projet/
cd /root/projet/
```

Tu crées un fichier `.env` pour stocker les données sensibles :

```
# Domaines
# Ces variables définissent les noms de domaine que tu as associés à tes applications.
# C'est ce que les utilisateurs taperont dans leur navigateur pour y accéder.
WALLABAG_HOST=wallabag.mondomaine.tld
N8N_HOST=n8n.mondomaine.tld

# PostgreSQL
# C'est la section pour la base de données PostgreSQL, qui est partagée par Wallabag et n8n.
# Le fait d'avoir des utilisateurs et des bases de données séparées est une bonne pratique de sécurité.
N8N_POSTGRES_USER=n8nuser
N8N_POSTGRES_PASSWORD=motdepassen8n
N8N_POSTGRES_DB=n8ndb
WALLABAG_POSTGRES_USER=wallabaguser
WALLABAG_POSTGRES_PASSWORD=motdepassewallabag
WALLABAG_POSTGRES_DB=wallabagdb
# Le port de la base de données.
POSTGRES_PORT=5432

# Traefik
# Cette section concerne Traefik.
# L'e-mail est obligatoire pour le renouvellement des certificats SSL par Let's Encrypt.
EMAIL_LE=ton-email@example.com
```

Pourquoi un fichier `.env` d’ailleurs ? Son intérêt est triple :

- **Sécurité et confidentialité** : Tu ne partages jamais tes mots de passe et autres informations sensibles sur des plateformes comme GitHub. Le fichier `.env` reste en local et n’est pas versionné.
- **Facilité d’utilisation** : Si tu dois changer un domaine ou un mot de passe, tu n’as qu’un seul endroit à modifier. Pas besoin de fouiller dans plusieurs fichiers `docker-compose.yml` ou `traefik.yml`.
- **Portabilité** : Si tu veux déployer la même stack sur un autre serveur, il te suffit de copier les fichiers de configuration et de remplir le nouveau fichier `.env` avec les informations du nouvel environnement. C’est une démarche très pragmatique et efficace.

C’est une approche qui te donne **une grande flexibilité** et **simplifie énormément** la maintenance de ton infrastructure.

Tu continues avec un fichier de configuration pour Traefik (`traefik.yml`) :

```
# entryPoints: C'est ici que l'on définit les points d'entrée de notre réseau.
# Ce sont les portes par lesquelles le trafic arrive dans Traefik.
entryPoints:
  # 'web': C'est le point d'entrée pour le trafic HTTP standard.
  # On le fait écouter sur le port 80.
  web:
    address: ":80"
  # 'websecure': C'est le point d'entrée pour le trafic HTTPS sécurisé.
  # Il écoute sur le port 443. C'est le point d'entrée que nous utiliserons
  # pour les applications (Wallabag, n8n) avec leurs certificats SSL.
  websecure:
    address: ":443"

# providers: Les fournisseurs sont les "yeux" de Traefik.
# Ils lui disent où chercher les services à exposer.
providers:
  # 'docker': On utilise le fournisseur Docker. Traefik va surveiller le démon Docker
  # pour détecter les conteneurs qui ont les labels spécifiques de Traefik.
  docker:
    # 'exposedByDefault: false' : C'est une bonne pratique de sécurité.
    # Cela signifie que seuls les conteneurs avec le label "traefik.enable=true"
    # seront gérés par Traefik.
    exposedByDefault: false

# certificatesResolvers: C'est la section pour les certificats SSL.
# Elle configure Traefik pour qu'il puisse obtenir des certificats gratuitement
# via Let's Encrypt.
certificatesResolvers:
  # 'le': C'est le nom de notre "résolveur de certificats". On peut l'appeler comme on veut,
  # mais 'le' (pour Let's Encrypt) est une convention courante.
  le:
    acme:
      # 'email': Traefik a besoin d'un e-mail pour s'enregistrer auprès de Let's Encrypt.
      email: ${EMAIL_LE}
      # 'storage': C'est l'emplacement où Traefik va stocker les certificats qu'il a obtenus.
      # Ce chemin doit correspondre au volume "traefik_data" défini dans le docker-compose.yml.
      storage: /letsencrypt/acme.json
      # 'httpChallenge': On utilise le challenge HTTP pour la vérification de la propriété du domaine.
      # Let's Encrypt va essayer d'accéder à un fichier sur le port 80, et si ça fonctionne,
      # il nous délivre le certificat. C'est pour ça qu'il faut que le port 80 soit ouvert.
      httpChallenge:
        entryPoint: web
```

Enfin, tu crées un fichier `docker-compose.yml` pour monter une stack Docker complète :

```
services:
  # Traefik est le "cerveau" de notre architecture. Il agit comme un reverse proxy.
  # Son job est de diriger les requêtes web vers les bons services (Wallabag et n8n)
  # et de gérer automatiquement les certificats SSL.
  traefik:
    image: traefik:latest
    command:
      # On force Traefik à utiliser notre fichier de configuration "traefik.yml".
      - "--configFile=/etc/traefik/traefik.yml"
      # Cette option active l'API de Traefik pour le débogage et la surveillance.
      - "--api.insecure=true"
    ports:
      # On expose le port 80 pour le trafic HTTP et le port 443 pour le HTTPS.
      # C'est ce qui rend nos applications accessibles depuis l'extérieur.
      - "80:80"
      - "443:443"
      # Le port 8080 expose l'interface de Traefik pour vérifier son bon fonctionnement.
      - "8080:8080"
    volumes:
      # C'est la ligne magique qui permet à Traefik de "voir" les autres conteneurs Docker.
      # Il peut ainsi s'auto-configurer en fonction des labels qu'on a mis sur chaque service.
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
      # Ce volume est crucial, il stocke les certificats SSL que Let's Encrypt génère.
      # "traefik_data" est le nom du volume que l'on déclare plus bas.
      - "traefik_data:/letsencrypt"
      # On monte notre fichier de configuration Traefik à l'intérieur du conteneur.
      - "./traefik.yml:/etc/traefik/traefik.yml:ro"

  ---
  # Wallabag est notre application de lecture différée.
  # L'image est "wallabag/wallabag" car c'est la version officielle sur Docker Hub.
  wallabag:
    image: wallabag/wallabag
    env_file:
      # On utilise un fichier .env pour toutes les variables sensibles (mots de passe, noms de domaine...).
      # C'est une bonne pratique pour ne pas les exposer directement dans le code.
      - .env
    environment:
      # On configure Wallabag pour qu'il utilise PostgreSQL comme base de données.
      - DATABASE_DRIVER=pdo_pgsql
      - DATABASE_URL=postgres://${WALLABAG_POSTGRES_USER}:${WALLABAG_POSTGRES_PASSWORD}@postgres:${POSTGRES_PORT}/${WALLABAG_POSTGRES_DB}
    labels:
      # Ces labels sont la clé de la communication entre Traefik et Wallabag.
      # Traefik va les lire pour savoir comment router le trafic vers ce service.
      - "traefik.enable=true"
      # La règle de routage : si la requête vient du nom de domaine spécifié dans WALLABAG_HOST,
      # alors Traefik la redirige vers ce conteneur.
      - "traefik.http.routers.wallabag.rule=Host(`${WALLABAG_HOST}`)"
      # On indique à Traefik de n'utiliser que le point d'entrée sécurisé (HTTPS).
      - "traefik.http.routers.wallabag.entrypoints=websecure"
      # On lui dit d'utiliser notre "certresolver" (le) pour obtenir un certificat SSL pour ce domaine.
      - "traefik.http.routers.wallabag.tls.certresolver=le"
    depends_on:
      # Wallabag a besoin que la base de données PostgreSQL soit démarrée avant lui.
      - postgres

  ---
  # n8n est notre outil d'automatisation.
  n8n:
    image: n8nio/n8n:latest
    env_file:
      - .env
    environment:
      # On configure les variables d'environnement de n8n pour qu'il se connecte à sa propre base de données PostgreSQL.
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=${POSTGRES_PORT}
      - DB_POSTGRESDB_DATABASE=${N8N_POSTGRES_DB}
      - DB_POSTGRESDB_USER=${N8N_POSTGRES_USER}
      - DB_POSTGRESDB_PASSWORD=${N8N_POSTGRES_PASSWORD}
    labels:
      # On applique les mêmes labels qu'à Wallabag pour que Traefik gère le routage et le SSL pour n8n.
      - "traefik.enable=true"
      - "traefik.http.routers.n8n.rule=Host(`${N8N_HOST}`)"
      - "traefik.http.routers.n8n.entrypoints=websecure"
      - "traefik.http.routers.n8n.tls.certresolver=le"
    depends_on:
      # n8n aussi dépend de la base de données PostgreSQL.
      - postgres

  ---
  # PostgreSQL est notre base de données.
  # On utilise une seule instance de PostgreSQL qui va gérer deux bases de données distinctes, une pour Wallabag et une pour n8n.
  postgres:
    image: postgres:latest
    env_file:
      - .env
    volumes:
      # Ce volume est essentiel. Il garantit que les données de la base de données ne seront pas perdues si le conteneur est supprimé.
      # On lie le volume "wallabag_data" au répertoire de stockage de PostgreSQL.
      - wallabag_data:/var/lib/postgresql/data

---
# Volumes
# Les volumes sont des espaces de stockage persistants pour les données des conteneurs.
# Ils sont gérés par Docker et ne sont pas supprimés avec les conteneurs.
volumes:
  # Ce volume va stocker les données de la base de données PostgreSQL.
  wallabag_data:
  # Ce volume va stocker les certificats SSL de Traefik.
  traefik_data:
```

Cette configuration te donne :

- **Wallabag** accessible via HTTPS avec certificat Let’s Encrypt automatique
- **n8n** prêt pour tes automatisations futures
- **Deux bases PostgreSQL** persistantes, une pour n8n, une pour Wallabag.
- **Isolation complète** via Docker

## 5. Le comparatif final

<figure class="wp-block-table"><table><thead><tr><th>Critère</th><th>Raindrop.io</th><th>Fabric</th><th>Wallabag</th></tr></thead><tbody><tr><td>**Interface**</td><td>Moderne, intuitive</td><td>Moderne, IA intégrée</td><td>Minimaliste, spartiate mais efficace</td></tr><tr><td>**Prix**</td><td>de gratuit à 3€19 / mois</td><td>de gratuit à 4€67 / mois minimum</td><td>Gratuit (+ VPS : 5€99 / mois)</td></tr><tr><td>**API**</td><td>Très limitée</td><td>Pas d’API en version gratuite</td><td>Complète + webhooks</td></tr><tr><td>**Hébergement**</td><td>Cloud uniquement</td><td>Cloud uniquement</td><td>Self-hosted possible</td></tr><tr><td>**Automatisation**</td><td>Basique</td><td>Résumés IA intégrés</td><td>Totale via n8n</td></tr><tr><td>**Contrôle des données**</td><td>Limité</td><td>Aucun</td><td>Total</td></tr><tr><td>**Migration Pocket**</td><td>Import possible</td><td>Import limité</td><td>Import complet</td></tr></tbody></table>

</figure>Donc ce n’est pas complètement gratuit, mais [sur un VPS à 5€99](https://githubcopilotchallenge.tsw.ovh/go/layer7), tu peux héberger Wallabag, n8n, Nextcloud et d’autres services. Et tu économises.

## Mon workflow actuel

Avec Wallabag et n8n en place, j’ai pu reconstituer (et améliorer) mon système de lecture différée :

- **Sauvegarde** : extension navigateur ou partage depuis mobile
- **Organisation** : tags automatiques par domaine + tags manuels par projet
- **Automatisation** : workflow n8n qui génère un résumé hebdomadaire de mes lectures
- **Coût total** : 5€99 / mois de VPS Layer7, et moins de 5 centimes pour l’analyse IA via openrouter pour une trentaine d’articles.

L’interface peut sembler datée, mais elle est claire et conviviale.

<figure class="wp-block-image aligncenter size-full">[![lecteur wallabag](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/reader-wallabag.webp)](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/reader-wallabag.webp)<figcaption class="wp-element-caption">lecteur wallabag</figcaption></figure>Trouver l’information ? Facile entre la recherche et les filtres :

<figure class="wp-block-image aligncenter size-full">[![recherche et filtres - wallabag](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/recherche-filtres-wallabag.webp)](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/recherche-filtres-wallabag.webp)<figcaption class="wp-element-caption">recherche et filtres – wallabag</figcaption></figure>Et pouvoir annoter et surligner est un vrai luxe :

<figure class="wp-block-image aligncenter size-full">[![surlignage et annotations - wallabag](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/surlignage-annotations-wallabag.webp)](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/surlignage-annotations-wallabag.webp)<figcaption class="wp-element-caption">surlignage et annotations – wallabag</figcaption></figure>Le tout avec un contrôle total de mes données et une flexibilité d’automatisation que n’offraient ni Pocket ni ses alternatives commerciales.

Et voici le résumé

Pour autant, je suis bien conscient d’avoir un profil “légèrement atypique”. Wallabag ne convient pas à tout le monde, on pourrait même dresser un tableau en fonction du profil…

## Guide de choix selon votre profil

<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Ta situation</th><th>Outil recommandé</th><th>Pourquoi</th></tr></thead><tbody><tr><td>**Usage simple, pas de technique**</td><td>Raindrop.io</td><td>Interface parfaite, apps natives impeccables</td></tr><tr><td>**Besoin d’automatisation avancée**</td><td>**Wallabag + n8n**</td><td>API complète, self-hosted, évolutif</td></tr><tr><td>**Résumés IA clé en main**</td><td>Fabrik</td><td>Fonctionnalités uniques, mais abonnement</td></tr><tr><td>**Contrôle total des données**</td><td>**Wallabag**</td><td>Open source, auto-hébergé</td></tr></tbody></table>

</figure>## Conclusion : plus qu’une migration, une évolution

Cette recherche forcée m’a finalement mené vers une solution plus robuste et plus flexible que Pocket ne l’était. Wallabag prouve qu’une alternative open source peut non seulement remplacer un service propriétaire, mais le dépasser en termes de fonctionnalités et de contrôle.

La fermeture de Pocket était une contrainte. Elle s’est transformée en opportunité d’améliorer mon workflow de lecture et de reprendre le contrôle de mes données.

Et je te prépare un article détaillé sur les automatisations n8n que j’ai mises en place avec Wallabag. Parce que sauvegarder, c’est bien. Mais sauvegarder pour sauvegarder n’a pas de sens. Lire, c’est bien aussi, mais avoir une vue d’ensemble, une synthèse avec les tendances, et en prime quelques idées… c’est meiux encore…

*Vous avez vécu la même migration depuis Pocket ? Partagez votre retour d’expérience en commentaire – je compile les meilleures astuces pour un prochain article !*