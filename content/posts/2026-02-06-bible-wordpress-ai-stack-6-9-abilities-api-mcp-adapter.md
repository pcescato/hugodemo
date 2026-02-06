---
title: "Bible WordPress AI Stack 6.9+ : Abilities API & MCP Adapter"
date: "2026-02-06T19:43:41+00:00"
lastmod: "2026-02-06T19:43:41+00:00"
draft: false
description: "WordPress, l’IA et le MCP : démystifier le nouveau langage Quand on lit que «&nbsp;WordPress supporte désormais le MCP et les Abilities API&nbsp;», la plupart des utilisateurs lèvent un sourcil..."
categories:
  - Non classé
author: githubcopilotchallenge
image: /images/1342/featured.webp
image_formats:
  - /images/1342/featured.webp
  - /images/1342/featured.avif
---

## WordPress, l’IA et le MCP : démystifier le nouveau langage

![](/images/1342/unnamed-8-1024x555.webp)

Quand on lit que « WordPress supporte désormais le MCP et les Abilities API », la plupart des utilisateurs lèvent un sourcil : *MCP ? Abilities ?* Cela ressemble à du jargon d’ingénieur sorti tout droit d’un film de science-fiction.

Et pourtant, ce qui se cache derrière ces termes est probablement l’une des évolutions les plus importantes de WordPress depuis l’introduction de Gutenberg.

![](/images/1342/mermaid-1024x777.webp)

## Pourquoi ce changement ?

Le web évolue rapidement : les utilisateurs ne veulent plus simplement lire du contenu ou passer une commande, ils veulent **dialoguer** avec les sites web.

Les grands modèles de langage (LLM) comme ChatGPT, Claude ou Mistral ont montré qu’une IA pouvait comprendre des requêtes complexes et y répondre avec pertinence. Mais jusqu’à présent, **WordPress ne pouvait pas leur parler directement**.

C’est là qu’interviennent deux briques majeures introduites par la fondation WordPress :

- **Abilities API** : une couche qui expose les fonctionnalités de WordPress (créer un article, consulter une commande, récupérer un utilisateur, etc.) comme des *compétences* que l’IA peut invoquer.
- **MCP (Model Context Protocol)** : un protocole standardisé qui permet aux agents IA de se connecter à différentes applications (WordPress, Slack, GitHub, etc.) sans devoir tout recoder.

![](/images/1342/Gemini_Generated_Image_5596oj5596oj5596-1024x559.webp)

## Une analogie simple

Imagine que ton site WordPress devienne un **collaborateur virtuel**.

Jusqu’à présent, un chatbot sur ton site se limitait à donner des réponses pré-écrites. Avec MCP et Abilities API, ton chatbot peut :

- consulter directement ta base de données de produits,
- créer un brouillon d’article pour un rédacteur,
- vérifier le statut d’une commande,
- ou même appeler une API tierce, le tout sans quitter WordPress.

Et surtout, tout cela se fait **sans compromettre la sécurité** : les Abilities sont déclarées explicitement, et les permissions WordPress restent actives.

## Le rôle du MCP dans l’écosystème IA

![](/images/1342/Gemini_Generated_Image_mg2ycnmg2ycnmg2y-1024x559.webp)

Le protocole MCP est **un connecteur universel pour l’IA**, commun à tous les agents IA. Il permet de connecter un modèle de langage (Claude, GPT, Mistral…) à des outils, services et sources de données externes sans passer par un plugin spécifique à chaque service.

Tu branches ton modèle d’IA sur ton site WordPress, et il sait immédiatement quelles actions il peut effectuer.

## Ce que tu vas apprendre dans ce guide

![](/images/1342/analyse-de-donnees-1024x681.webp)

Ce guide t’expliquera comment :

1. Installer une stack WordPress optimisée pour accueillir MCP et Abilities API ;
2. Comprendre comment ces briques communiquent entre elles ;
3. Créer ta première *ability* — une commande IA capable d’interagir avec ton site.

L’objectif n’est pas seulement d’avoir un site « IA-ready », mais de **comprendre** comment tout fonctionne, pour pouvoir ensuite créer tes propres extensions et agents personnalisés.

> En résumé : le MCP n’est pas une technologie de plus, c’est une nouvelle grammaire entre les IA et les logiciels.
> WordPress, en l’adoptant, se positionne au cœur du web conversationnel à venir.

# Architecture et concepts de la stack WordPress AI

![](/images/1342/newplot-19.webp)

Après avoir compris le pourquoi, voyons **comment** tout cela s’articule. Le duo *Abilities API + MCP Adapter* transforme WordPress en un serveur capable de dialoguer avec des agents IA de manière standardisée.

## Les trois piliers de l’écosystème

### 1. Abilities API

C’est le cœur du dispositif côté WordPress.
Elle permet d’exposer certaines actions WordPress sous forme de *compétences* (abilities) que l’IA peut invoquer en toute sécurité.

Exemples :

- `get_recent_posts` : récupérer les derniers articles publiés ;
- `wc_get_order_status` : obtenir le statut d’une commande WooCommerce ;
- `create_draft_post` : créer un brouillon d’article.

Chaque ability possède :

- un nom unique ;
- une description ;
- un schéma d’entrée et de sortie (comme une mini-API JSON) ;
- une fonction PHP d’exécution ;
- une règle de permission.

### 2. MCP Adapter

Ce plugin agit comme un **interprète universel**.
Il traduit les requêtes venant d’un agent IA compatible *Model Context Protocol* (Claude, GPT, Mistral, etc.) vers le langage des Abilities API, puis renvoie la réponse au modèle.

Autrement dit, il permet à WordPress de « parler MCP ».

### 3. Le client IA (agent LLM)

C’est le cerveau de l’ensemble.
Un modèle comme **Claude Desktop**, **ChatGPT via wrapper MCP**, ou **Mistral API** peut désormais se connecter à WordPress et exécuter des actions grâce à ces capacités exposées.

## Flux de communication

```
graph TD
  A[Utilisateur ou Agent IA] -->|Requête naturelle| B["LLM (Claude / GPT / Mistral)"]
  B -->|Appel MCP| C[MCP Adapter]
  C -->|Exécution| D[Abilities API]
  D -->|Action PHP sécurisée| E[WordPress Core / WooCommerce]
  E -->|Résultat JSON| D
  D -->|Réponse structurée| C
  C -->|Retour formaté| B
  B -->|Réponse en langage naturel| A
```

Ce schéma illustre la chaîne complète :

1. L’utilisateur ou l’agent envoie une requête.
2. Le modèle de langage (LLM) décide s’il doit utiliser un *tool* MCP (ex. : récupérer les 5 derniers articles).
3. Le MCP Adapter relaie cette requête à WordPress via l’Abilities API.
4. WordPress exécute la fonction PHP associée et renvoie le résultat.
5. Le LLM reformule la réponse dans un langage compréhensible pour l’humain.

## Pourquoi cette architecture est importante

- **Interopérabilité** : un même agent IA peut se connecter à WordPress, GitHub ou Notion sans modification.
- **Sécurité** : les abilities respectent les permissions WordPress, aucune action non déclarée n’est exécutable.
- **Extensibilité** : chaque développeur peut créer ses propres abilities comme des mini-plugins.
- **Contrôle local** : aucune donnée n’est transmise à un tiers sans ton accord — tout passe par ton instance WordPress.

## Un pont sûr entre WordPress et l’IA

Les anciennes intégrations d’IA (via API ou webhook) exposaient souvent des endpoints trop larges.
Le MCP, lui, **normalise la communication** et impose un cadre d’échange clair :

- authentification par jeton (Bearer) ;
- description stricte des actions autorisées ;
- traçabilité complète des appels.

Ainsi, WordPress devient un **fournisseur d’API contrôlées**, prêtes à être appelées par des agents intelligents sans risque d’accès excessif.

## Et maintenant ?

Maintenant que tu comprends comment la communication s’organise entre WordPress et le monde des IA, on peut passer à la pratique, en installant la stack complète WordPress AI, étape par étape, pour activer Abilities API et MCP Adapter sur ton propre site.

# Installation manuelle — Stack WordPress AI

## ⚙️ Prérequis système

<table><thead><tr><th>Élément</th><th class="has-text-align-center" data-align="center">Minimum</th><th class="has-text-align-center" data-align="center">Recommandé</th></tr></thead><tbody><tr><td>**CPU**</td><td class="has-text-align-center" data-align="center">2 vCPU</td><td class="has-text-align-center" data-align="center">4 vCPU</td></tr><tr><td>**RAM**</td><td class="has-text-align-center" data-align="center">2 Go</td><td class="has-text-align-center" data-align="center">8 Go</td></tr><tr><td>**Stockage**</td><td class="has-text-align-center" data-align="center">20 Go SSD</td><td class="has-text-align-center" data-align="center">50 Go SSD</td></tr><tr><td>**OS**</td><td class="has-text-align-center" data-align="center">Ubuntu 24.04 ou Debian 12</td><td class="has-text-align-center" data-align="center">idem</td></tr><tr><td>**Accès**</td><td class="has-text-align-center" data-align="center">root/sudo + nom de domaine valide</td><td class="has-text-align-center" data-align="center"></td></tr></tbody></table>

Nous utiliserons :

- **MariaDB 12.0**
- **OpenLiteSpeed + PHP 8.4** (dernière version stable)
- **Composer** et **WP‑CLI**
- **UFW** (un pare-feu) et **Fail2Ban** (un framework de prévention contre les intrusions)

## Étape 1 — Préparation du système

```
apt update && apt upgrade -y
apt install -y wget curl git unzip \
  software-properties-common apt-transport-https ca-certificates gnupg lsb-release \
  ufw fail2ban
```

### Pare‑feu UFW

```
ufw allow OpenSSH
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 7080/tcp  # Interface admin d’OpenLiteSpeed
ufw --force enable
```

Active aussi **Fail2Ban** :

```
systemctl enable --now fail2ban
```

## Étape 2 — Installation et sécurisation de MariaDB 12.0

```
curl -LsS https://r.mariadb.com/downloads/mariadb_repo_setup | \
  bash -s -- --mariadb-server-version="mariadb-12.0"
apt update
apt install -y mariadb-server mariadb-client
systemctl enable --now mariadb
```

### Sécurisation rapide

```
mysql_secure_installation
```

### Création de la base de données WordPress

```
CREATE DATABASE wp_mcp CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'MotDePasseFort';
GRANT ALL PRIVILEGES ON wp_mcp.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
```

## Étape 3 — Installation d’OpenLiteSpeed + PHP 8.4

```
wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | bash
apt update
apt install -y openlitespeed lsphp84 \
  lsphp84-mysql lsphp84-curl lsphp84-mbstring lsphp84-json lsphp84-intl \
  lsphp84-opcache lsphp84-zip lsphp84-gd lsphp84-xml
systemctl enable --now lsws
```

### Configuration PHP

Fichier : `/usr/local/lsws/lsphp84/etc/php/8.4/litespeed/php.ini`

```
upload_max_filesize = 64M
post_max_size = 64M
memory_limit = 512M
max_execution_time = 300
opcache.enable = 1
opcache.memory_consumption = 256
date.timezone = Europe/Paris
```

Redémarre OpenLiteSpeed après modification :

```
systemctl restart lsws
```

## Étape 4 — Installation de Composer et WP‑CLI

```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
chmod +x /usr/local/bin/composer
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar && mv wp-cli.phar /usr/local/bin/wp
```

Vérifie :

```
composer --version
wp --info
```

## Étape 5 — Installation de WordPress

```
mkdir -p /var/www/ton-domaine/html
cd /var/www/ton-domaine/html
wp core download --allow-root
wp config create \
  --dbname=wp_mcp --dbuser=wp_user --dbpass='MotDePasseFort' --dbhost=localhost \
  --dbcharset=utf8mb4 --dbcollate=utf8mb4_unicode_ci --allow-root
wp core install --url="https://ton-domaine" \
  --title="WordPress AI Stack" \
  --admin_user=admin --admin_password='MDPAdminFort' \
  --admin_email=admin@ton-domaine --skip-email --allow-root
```

### Permissions correctes

Sous OpenLiteSpeed, le service web s’exécute souvent sous `www:www` (et non `nobody:nogroup`).

```
chown -R www:www /var/www/ton-domaine
find /var/www/ton-domaine -type d -exec chmod 755 {} \;
find /var/www/ton-domaine -type f -exec chmod 644 {} \;
```

## Étape 6 — Installation des Building Blocks

```
cd /var/www/ton-domaine/html/wp-content/plugins
git clone https://github.com/WordPress/abilities-api.git
git clone https://github.com/WordPress/mcp-adapter.git
cd abilities-api && composer install --no-dev --optimize-autoloader
cd ../mcp-adapter && composer install --no-dev --optimize-autoloader
cd /var/www/ton-domaine/html
wp plugin activate abilities-api --allow-root
wp plugin activate mcp-adapter --allow-root
wp plugin list --allow-root
```

## Étape 7 — Test d’une *ability*

```
wp_register_ability('get_latest_posts', [
  'label' => 'Obtenir les derniers articles',
  'description' => 'Retourne N derniers posts',
  'input_schema' => ['type' => 'object','properties'=>['count'=>['type'=>'integer','default'=>5]]],
  'permission_callback' => function(){ return true; },
  'execute_callback' => function($args){
    $posts = get_posts(['numberposts'=>$args['count'] ?? 5,'post_status'=>'publish']);
    return array_map(fn($p)=>['id'=>$p->ID,'title'=>$p->post_title,'url'=>get_permalink($p->ID)], $posts);
  }
]);
```

```
wp eval "print_r(wp_execute_ability('get_latest_posts', ['count'=>3]));" --allow-root
```

## Étape 8 — Configuration d’un serveur MCP

```
add_action('mcp_adapter_init', function($adapter){
  $adapter->create_server(
    'my-site-server', 'store-api', 'mcp', 'Assistant e‑commerce',
    'Agent IA connecté à WooCommerce', 'v1.0.0', [\RestTransport::class],
    ErrorLogMcpErrorHandler::class, NullMcpObservabilityHandler::class,
    ['wc_get_order_status'], ['get_latest_posts'], ['support_agent_prompt']
  );
});
```

Endpoint :

```
https://ton-domaine/wp-json/store-api/mcp
```

## Vérifications finales

```
wp plugin list --allow-root
systemctl status mariadb lsws
curl -I https://ton-domaine
```

Ta stack WordPress + OpenLiteSpeed + PHP 8.4 + Abilities/MCP est prête.