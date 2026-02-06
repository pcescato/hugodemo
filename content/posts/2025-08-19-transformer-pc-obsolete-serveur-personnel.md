---
title: Transformer un vieux PC ou laptop en serveur personnel
date: "2025-08-19T11:50:22+00:00"
lastmod: "2025-08-19T11:50:22+00:00"
draft: false
description: "Besoin d'un serveur de test pour tes projets perso ? Un vieux PC ou un vieux laptop  fera l'affaire. Coût : zéro. Temps nécessaire : moins de 30mn. Gain ? Énorme ! "
tags:
  - benchmark
  - low cost
  - performances
  - VPS
categories:
  - Tutoriels
author: githubcopilotchallenge
---

Il y a quelques mois, j’en ai eu assez de payer 6€ par mois pour un VPS de test chez un hébergeur, pour 3 vCPU et 8 Go de RAM. Un laptop que je n’utilisais jamais trainait dans mon placard : un i3-7020U avec 20 Go de RAM et un SSD de 220 Go. Rien d’extraordinaire, mais largement suffisant pour mes besoins de développement local.

Alors je me suis renseigné, j’ai testé, et j’ai monté mon serveur sur cette base. Le résultat ? À la hauteur de mes espérances. Voire même au-delà.

Et maintenant que mon serveur est en place, pourquoi ne pas te faire profiter de l’expérience à travers un petit tuto qui t’accompagnera dans cette démarche d’autonomie numérique ?

Et cerise sur le gâteau, **en utilisant exclusivement des outils open source** qui souvent valent et parfois surpassent leurs équivalents commerciaux.

Que tu sois un développeur cherchant un environnement de test gratuit ou simplement curieux de reprendre le contrôle sur tes données, transformer un PC ou un laptop “obsolète” en serveur personnel peut t’intéresser, et c’est bien plus accessible que tu ne le penses.

On va procéder par étapes, pour ne pas se perdre encours de route. Et la toute première étape, c’est de…

## Choisir et préparer son matériel

Pas besoin d’une machine de guerre pour notre serveur perso. L’avantage majeur des processeurs “serveur” (Threadripper, Epyc, Xeon) ? La stabilité et la puissance nécessaires pour faire tourner des applications en 24/7, sur des systèmes fortement sollicités et qui ne doivent surtout pas tomber en panne.

Mais quand c’est pour toi et toi seul (ou quelques personnes), les contraintes ne sont pas les mêmes. Ton mini serveur tiendra la charge bien plus facilement qu’en production. Et une vieille bécane qui ne servait plus à rien ni à personne, justement, c’est ce dont on a besoin.

Investissement zéro (d’un point de vue financier). ROI exponentiel. Mais “obsolète”, ça veut tout dire, et en même temps, çà ne veut rien dire de précis, il faut donc en tout premier lieu…

### Définir *obsolète* : jusqu’où remonter dans le temps ?

Tous les vieux PC ne sont pas adaptés à un usage serveur moderne. Pour faire tourner Ubuntu Server et Docker, il faut un minimum de compatibilité :

**Matériel compatible (à partir de 2007-2009) :**

- **Processeurs Intel** : Core 2 Duo et générations suivantes, Xeon série 5400 (Harpertown) ou Nehalem (W-3520, séries 35xx)
- **Processeurs AMD** : Athlon 64, Opteron 2ième génération, Phenom et générations suivantes
- **Architecture** : 64 bits obligatoire avec support SSE2

**Matériel trop ancien :** Les machines 32 bits ou très anciennes (486, Pentium III) ne peuvent pas faire tourner les distributions Linux actuelles. Mais elles gardent leur charme pour la nostalgie (jeux DOS, systèmes légers comme FreeDOS) ou des projets spécialisés.

### Spécifications recommandées

**Minimum viable (comme mon i3-7020U) :**

- un processeur 64 bits dual-core (même un i3 fait l’affaire)
- 4 Go de RAM minimum (16 ou 20 Go comme dans mon cas, c’est le luxe)
- un disque dur ou SSD (le SSD change vraiment la donne en réactivité)
- une carte réseau Ethernet ou Wi-Fi (Ethernet est plus stable pour un serveur)

**Configuration idéale :**

- un processeur **quad-core ou dual-core avec hyperthreading**
- **8 Go de RAM** ou plus
- **SSD** pour le système et les applications critiques
- une **connexion Ethernet** stable

### Préparation du matériel

Quelques recommandations avant de commencer l’installation :

1. **Nettoyage physique** : dépoussiére les composants internes pour éviter la surchauffe
2. **Vérification du disque** : utilise `smartctl` pour vérifier l’état du disque dur
3. **Sauvegarde** : récupère toutes les données importantes avant la réinstallation
4. **Test de stabilité** : laisse tourner la machine quelques heures pour détecter d’éventuels problèmes matériels

## Installer un système d’exploitation adapté

### Pourquoi choisir Ubuntu Server ?

Tout simplement parce qu’Ubuntu Server est le choix idéal pour débuter :

- Il offre une installation simple et guidée
- Il bénéficie d’une documentation abondante
- Il profite d’un large support communautaire
- Ses versions LTS (Long Term Support) sont stables et utilisables sur des années
- Il ne propose pas d’interface graphique inutile

Tu peux installer une autre distribution : *Debian*, *AlmaLinux*, *Rocky Linux* sont tout aussi indiquées, mais les instructions dans ce tutorial sont pour Debian / Ubuntu.

Tu as fait ton choix ? Alors passons à…

### L’installation étape par étape

1. **Tout d’abord, prépare le support d’installation** : 
    - Télécharge l’image ISO d’Ubuntu Server LTS depuis le site officiel
    - Crée une clé USB bootable avec [Rufus](https://rufus.ie/fr/) (Windows) ou `dd` (Linux/macOS)
2. **Ensuite, installe le système** : 
    - Démarre sur la clé USB
    - Suis l’assistant d’installation
    - Choisis un partitionnement simple (par défaut, sauf besoins spécifiques)
    - **Important** : n’installe pas d’environnement graphique – inutile et “gourmand” en ressources
3. **Enfin, occupe toi de la configuration réseau**
    - une IP statique est recommandée pour un serveur
    - Ou tu fais une réservation DHCP côté routeur pour simplifier

Tu peux maintenant faire tes premier pas d’administrateur système :

```
# Mise à jour du système
sudo apt update && sudo apt upgrade -y

# Installation d'outils de base
sudo apt install curl wget git htop
```

Ça y est, tu as un serveur, en configuration minimale certes, mais prêt à l’emploi.

Pour autant, tu n’es pas rendu. Dans l’état, tu ne feras pas grand chose de ton serveur, alors je te conseille de continuer par…

## L’installation et la configuration de Docker

### Docker ? Ok. Mais pourquoi Docker ?

Docker a révolutionné la gestion des services en les isolant dans des conteneurs légers. Sur mon petit serveur, je fais tourner simultanément Nextcloud, Grafana, Prometheus, n8n, et plusieurs projets de test sans aucun conflit. Chaque application vit dans son propre univers, avec ses dépendances, tout en partageant les ressources système de façon optimale.

L’autre avantage énorme : **la portabilité**. Si demain je change de serveur, je copie mes fichiers de configuration docker et tout redémarre à l’identique. Fini les installations manuelles interminables et les “ça marchait sur mon ancienne machine”.

Docker est quasi-agnostique : tant que tu ne pars pas sur des configurations ARM (Pi 5 par exemple), tout ce qui est dockerisable est dockerisé et utilisable sans configuration compliquée – tu verras dans la suite du tuto.

Mais avant de profiter de ton serveur, on passe à…

### L’installation de Docker

```
# Suppression des anciennes versions
sudo apt remove docker docker-engine docker.io containerd runc

# Installation des dépendances
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

# Ajout de la clé GPG officielle de Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Ajout du dépôt
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Installation
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Ajout de l'utilisateur au groupe docker
sudo usermod -aG docker $USER
```

### Vérification de l’installation

```
# Test Docker
docker --version
docker run hello-world

# Test Docker Compose (nouvelle syntaxe sans tiret)
docker compose version
```

Tu vois, installer Docker n’est pas vraiment douloureux, il suffit de suivre les étapes une à une. Et la prochaine, c’est le…

## Déploiement d’une stack de services de base

### Objectif de la stack

Nous allons déployer une infrastructure complète uniquement avec des outils open source :

- **Traefik**, le reverse proxy automatique
- **Portainer**, l’interface graphique de Docker
- **Prometheus** pour la collecte de métriques
- **Grafana** pour la visualisation des données récupérées par Prometheus

Cette stack tourne parfaitement sur mon vieux laptop.

Je te propose tout d’abord de créer la structure des fichiers. Tu ne mets pas tout en vrac dans le répertoire principal, tu organises proprement pour mieux t’y retrouver ensuite. Donc un dossier `certs` où tu mettras plus tard les certificats SSL.

```
~/stack-docker/
├── docker-compose.yml
├── traefik.yml
├── prometheus.yml
└── certs/
    ├── cert.pem
    └── cert.key
```

Maintenant, tu sais où on va, et la prochaine étape, c’est la…

### Configuration de Docker

Le premier fichier que tu vas créer, c’est `docker-compose.yml`. La commande ? Toute simple :

```
vim docker-compose.yml
```

C’est tout. Un petit `i` (pour insert) et tu peux coller le contenu suivant dans ton fichier. Tu enregistres utilisant la touche `Échap` puis en entrant la séquence suivante au clavier : `:x`, tu valides, et c’est terminé.

```
services:
  traefik:
    image: traefik:latest
    container_name: traefik
    command:
      - "--configFile=/traefik.yml"
    ports:
      - "80:80"
      - "443:443"
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.traefik.rule=Host(`traefik.localhost`)"
      - "traefik.http.routers.traefik.entrypoints=websecure"
      - "traefik.http.routers.traefik.tls=true"
      - "traefik.http.routers.traefik.service=api@internal"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./traefik.yml:/traefik.yml:ro
      - ./certs:/certs:ro
    networks:
      - web
    restart: unless-stopped

  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.portainer.rule=Host(`portainer.localhost`)"
      - "traefik.http.routers.portainer.entrypoints=websecure"
      - "traefik.http.routers.portainer.tls=true"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
    networks:
      - web
    restart: unless-stopped

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.prometheus.rule=Host(`prometheus.localhost`)"
      - "traefik.http.routers.prometheus.entrypoints=websecure"
      - "traefik.http.routers.prometheus.tls=true"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prometheus_data:/prometheus
    networks:
      - web
    restart: unless-stopped

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=changeme123
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.grafana.rule=Host(`grafana.localhost`)"
      - "traefik.http.routers.grafana.entrypoints=websecure"
      - "traefik.http.routers.grafana.tls=true"
    volumes:
      - grafana_data:/var/lib/grafana
    networks:
      - web
    restart: unless-stopped

volumes:
  portainer_data:
  prometheus_data:
  grafana_data:

networks:
  web:
    external: false
```

Tu remarques que dans ce fichier, il y a toute une section dédiée à Traefik, et qu’ensuite, on le retrouve pour exposer tous les services auxqueks on accède via un navigateur. Mais il faut avant passer par…

### La configuration de Traefik

Avant de passer à la configuration, un mot sur ce qu’est un *reverse proxy* : c’est tout simplement **un aiguilleur intelligent** qui dirige les visiteurs vers le bon service en fonction de l’adresse demandée.

Crée le fichier `~/stack-docker/traefik.yml` :

```
entryPoints:
  web:
    address: ":80"
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https
  websecure:
    address: ":443"
    http:
      tls:
        certificates:
          - certFile: "/certs/cert.pem"
            keyFile: "/certs/cert.key"

providers:
  docker:
    exposedByDefault: false
    network: stack-docker_web

api:
  dashboard: true
  # Pas de insecure: true car on passe par Traefik lui-même
```

Les fichiers `docker-compose.yml` et `traefik.yml` sont les fichiers essentiels de ta configuration. Pour autant, les outils suivants ne sont pas inutiles. Prometheus te parmet de connaître en temps réel l’état de santé de ton serveur. **Un must-have** pour ne pas subir un crash que tu aurais pu anticiper. Pour l’installer, tu n’as rien de plus à faire, c’est dans le fichier `docker-compose.yml` que tu l’as demandé. Mais il faut pour lui aussi par une phase essentielle, la…

### Configuration de Prometheus

Crée le fichier `~/stack-docker/prometheus.yml` :

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['prometheus:9090']
  
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

Voilà, tu en as fini avec les fichiers de configuration. Mais ce n’est pas fini, il reste encore quelques étapes essentielles avant que tu ne profites de ton serveur. Un serveur, même s’il n’hébergeait que les photos de grand-maman à la plage, nécessite un minimum de sécurisation. Retrouver ses photos sur les sites de hackers russes ne lui ferait peut-être pas plaisir… et ce qui ne fais pas plaisir à grand-maman ne te fais pas plus plaisir à toi… CQFD.

## Sécuriser avec des certificats SSL

### HTTPS local avec mkcert

Pour sécuriser tes services locaux, `mkcert` est la solution idéale. Sur mon setup, j’ai longtemps galéré avec les certificats auto-signés et les erreurs de navigateur. Mkcert résout tout ça élégamment :

```
# Installation de mkcert
curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/amd64"
chmod +x mkcert-v*-linux-amd64
sudo mv mkcert-v*-linux-amd64 /usr/local/bin/mkcert

# Installation du CA racine
mkcert -install

# Génération des certificats
mkdir -p ~/stack-docker/certs
cd ~/stack-docker/certs
mkcert localhost portainer.localhost prometheus.localhost grafana.localhost traefik.localhost
mv localhost+4.pem cert.pem
mv localhost+4-key.pem cert.key
```

**Note d’expérience** : Même avec mkcert, Firefox peut parfois faire du zèle. Sur Chrome, ça marche impeccablement. C’est un petit prix à payer pour avoir du HTTPS local sans configuration complexe.

### Configuration des noms locaux

Pour accéder à tes services via des noms plutôt que des IP :

1. **Édite le fichier hosts** sur chaque machine cliente : 
    - Linux/Mac : `/etc/hosts`
    - Windows : `C:\Windows\System32\drivers\etc\hosts`
2. **Ajoute les entrées** :

```
192.168.1.100 traefik.localhost
192.168.1.100 portainer.localhost
192.168.1.100 prometheus.localhost
192.168.1.100 grafana.localhost
```

*(Remplace `192.168.1.100` par l’IP de votre serveur)*

**Astuce pratique** : Sur mon setup, j’ai configuré une réservation DHCP pour que mon serveur ait toujours la même IP (192.168.1.50). Plus simple que de gérer une IP statique et ça évite les conflits.

## Lancement et configuration de la stack

### Démarrage des services

```
cd ~/stack-docker
docker compose up -d
```

### Vérification du déploiement

```
# Vérifier que tous les conteneurs tournent
docker ps

# Consulter les logs en cas de problème
docker compose logs traefik
docker compose logs portainer
```

### Accès aux interfaces

Depuis ta machine de développement, tu peux accéder à :

- **Traefik Dashboard** : https://traefik.localhost
- **Portainer** : https://portainer.localhost
- **Prometheus** : https://prometheus.localhost
- **Grafana** : https://grafana.localhost

**⚠️ Sécurité** : Change immédiatement les mots de passe par défaut !

### Configuration de Grafana

1. Connecte-toi avec admin/changeme123
2. Change le mot de passe
3. Ajoute Prometheus comme source de données : 
    - URL : `http://prometheus:9090`
    - Clique sur “Save &amp; Test”
4. Importe un dashboard préconfiguré : 
    - ID 1860 (Node Exporter Full)
    - ID 179 (Docker monitoring)

## Sécuriser le serveur

### Configuration SSH sécurisée

Édite le fichier `/etc/ssh/sshd_config` :

```
sudo vim /etc/ssh/sshd_config
```

Modifie ou ajoute ces lignes :

```
PermitRootLogin no
PasswordAuthentication no  # Après avoir configuré les clés
Port 2222  # Port non standard (optionnel)
```

Redémarre le service SSH :

```
sudo systemctl restart ssh
```

Assure-toi que la connexion par clé SSH **fonctionne parfaitement**, afin d’éviter de te retrouver bloqué hors de ton propre serveur.

Tu as oublié de vérifier que ta clé SSH fonctionne ? Pas de panique ! Quelques manipulations simples et tout va rentrer dans l’ordre :

- Branche un écran à ton serveur (si c’est une tour, sur un laptop tu l’as déjà).
- Édite le fichier de configuration `sshd_config` et remets `PasswordAuthentication yes` avant de sauvegarder et de relancer le service. Et ne tourne pas en boucle, teste bien cette fois-ci !

### Configuration des clés SSH

```
# Sur votre machine locale
ssh-keygen -t rsa -b 4096 -C "votre@email.com"

# Copie de la clé vers le serveur
ssh-copy-id utilisateur@IP_SERVEUR
```

### Pare-feu avec UFW

```
# Installation et activation
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Autoriser SSH (adapte le port si modifié)
sudo ufw allow 22/tcp

# Autoriser les services web
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 8080/tcp

# Activation
sudo ufw enable
```

## Monitoring et surveillance

### Ajout de Node Exporter pour les métriques système

Ajoute cette section dans ton fichier `~/stack-docker/docker-compose.yml` :

```
  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    command:
      - '--path.rootfs=/host'
    volumes:
      - '/:/host:ro,rslave'
    networks:
      - web
    restart: unless-stopped
```

### Dashboards Grafana recommandés

- **Node Exporter Full (1860)** : monitoring système complet
- **Docker monitoring (179)** : surveillance des conteneurs
- **Traefik 2.0 (4475)** : métriques du reverse proxy

### Surveillance des ressources en ligne de commande

```
# Installation d'outils de monitoring
sudo apt install htop iotop iftop ncdu

# Surveillance temps réel
htop          # Processus et mémoire
iotop         # E/S disque
iftop         # Trafic réseau
df -h         # Espace disque
```

## Maintenance et sauvegarde

### Mises à jour automatiques

```
# Configuration des mises à jour automatiques
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

### Script de sauvegarde

Crée le fichier `~/backup-docker.sh` :

```
#!/bin/bash
# Script de sauvegarde automatique

BACKUP_DIR="/backup"
DATE=$(date +%Y%m%d_%H%M%S)

# Création du dossier de sauvegarde
mkdir -p $BACKUP_DIR

# Sauvegarde des volumes Docker
docker run --rm -v portainer_data:/data -v $BACKUP_DIR:/backup alpine tar czf /backup/portainer_$DATE.tar.gz -C /data .
docker run --rm -v grafana_data:/data -v $BACKUP_DIR:/backup alpine tar czf /backup/grafana_$DATE.tar.gz -C /data .
docker run --rm -v prometheus_data:/data -v $BACKUP_DIR:/backup alpine tar czf /backup/prometheus_$DATE.tar.gz -C /data .

# Sauvegarde de la configuration
tar czf $BACKUP_DIR/config_$DATE.tar.gz ~/stack-docker/

# Nettoyage des sauvegardes anciennes (> 30 jours)
find $BACKUP_DIR -name "*.tar.gz" -mtime +30 -delete

echo "Sauvegarde terminée : $DATE"
```

Rend le script exécutable :

```
chmod +x ~/backup-docker.sh
```

Même si tu as une sauvegarde locale, n’oublie pas l’importance d’une **stratégie de sauvegarde externe** (vers un NAS ou un service cloud, sur ton poste de travail ou sur un SSD externe) pour te prémunir contre une panne matérielle du serveur lui-même.

### Tâches cron pour l’automatisation

```
# Édition de la crontab
crontab -e

# Exemples de tâches
# Sauvegarde quotidienne à 2h du matin
0 2 * * * /home/user/backup-docker.sh

# Mise à jour des conteneurs le dimanche à 3h
0 3 * * 0 cd /home/user/stack-docker && docker compose pull && docker compose up -d

# Nettoyage Docker hebdomadaire
0 4 * * 0 docker system prune -f
```

## Extensions et évolutions possibles

### Services populaires à ajouter

**Cloud personnel :**

- **Nextcloud** : stockage et synchronisation de fichiers
- **Syncthing** : synchronisation P2P

**Media et divertissement :**

- **Plex/Jellyfin** : serveur multimédia
- **Transmission** : client BitTorrent

**Productivité :**

- **n8n** : automatisation de tâches (alternative à Zapier)
- **Ghost** : plateforme de blog
- **WordPress** : site web complet

**Sécurité et réseau :**

- **Pi-hole** : bloqueur de publicités DNS
- **WireGuard** : VPN personnel

### Exemple d’ajout de service : Nextcloud

Pour ajouter Nextcloud à votre stack, ajoute cette section dans `~/stack-docker/docker-compose.yml` :

```
  nextcloud:
    image: nextcloud:latest
    container_name: nextcloud
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.nextcloud.rule=Host(`nextcloud.localhost`)"
      - "traefik.http.routers.nextcloud.entrypoints=websecure"
      - "traefik.http.routers.nextcloud.tls=true"
    volumes:
      - nextcloud_data:/var/www/html
    environment:
      - MYSQL_ROOT_PASSWORD=rootpass
      - MYSQL_PASSWORD=ncpass
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
    networks:
      - web
    restart: unless-stopped
```

Et ajoute le volume correspondant dans la section volumes :

```
volumes:
  portainer_data:
  prometheus_data:
  grafana_data:
  nextcloud_data:  # Nouvelle ligne
```

## Conclusion

Cette expérience m’a ouvert les yeux : pourquoi dépendre de services payants quand on peut créer sa propre infrastructure avec du matériel existant ou avec du matériel d’occasion à bas prix ? Une tour ou un portable avec un i3 et 16Go de RAM se trouve à **moins de 50 €** sur leboncoin.

Aujourd’hui, mon petit serveur fait tourner une dizaine de services en continu, surveille mes applications, héberge mes projets en phase de test, et tout cela sans débourser un centime supplémentaire. Et il tient la charge, avec une stack correcte (Traefik, Portainer, AlloyDB Omni, n8n, Prometheus, Grafana, node importer…) :

[

![Mesures - Grafana](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/grafana-dashboard.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/grafana-dashboard.jpg)Et même par ces temps caniculaires (capture en date du 11 août 2025, avec des pics de température à 42°C et 29/30°C à l’intérieur – je n’ai pas de clim…), il ne surchauffe pas :

[

![Grafana - température des processeurs](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/grafana-temperature-processeurs.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/grafana-temperature-processeurs.jpg)Avec le tableau de bord deTreafik, j’ai un œil sur mon réseau :

[

![Tableau de bord Traefik](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/traefik-1024x451.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/traefik.jpg)Et grâce à Portainer, je peux avoir un accès direct à mes images Docker, je peux en installer, j’ai un accès direct aux logs sans passer par la ligne de commande…

[

![Tableau de bord Portainer](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/portainer-1024x495.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/08/portainer.jpg)Pour les performances, par rapport au [VPS Contabo](https://githubcopilotchallenge.tsw.ovh/vps-low-cost-layer7-vs-contabo/) que j’ai déjà testé, ce serveur sur laptop est une pépite : **processeur, RAM, SSD sont plus performants** (ce n’est pas peu dire) et l’exploitation en mode local m’affranchit de la latence du réseau.

***Un i3-7020U de 2017 qui bat un serveur payant… ça donne à réfléchir sur la qualité de certaines offres “low cost”.***

La preuve par 9 dans un comparatif rapide…

## Laptop vs VPS Contabo : Pourquoi payer quand on a mieux à la maison ?

### Configurations comparées

<table class="has-fixed-layout"><thead><tr><th></th><th class="has-text-align-center" data-align="center">**Mon laptop Lenovo**</th><th class="has-text-align-center" data-align="center">**Mon ancien VPS Contabo**</th></tr></thead><tbody><tr><td>**Processeur**</td><td class="has-text-align-center" data-align="center">Intel i3-7020U @ 2.30GHz</td><td class="has-text-align-center" data-align="center">Intel Broadwell (E5-26xx)</td></tr><tr><td>**RAM**</td><td class="has-text-align-center" data-align="center">20 Go</td><td class="has-text-align-center" data-align="center">8 Go</td></tr><tr><td>**Stockage**</td><td class="has-text-align-center" data-align="center">220 Go SSD</td><td class="has-text-align-center" data-align="center">150 Go SSD</td></tr><tr><td>**Coût mensuel**</td><td class="has-text-align-center" data-align="center">**0€** (déjà payé)</td><td class="has-text-align-center" data-align="center">**5,36€**</td></tr></tbody></table>

### Résultats des tests de performance

#### CPU (sysbench cpu –threads=1)

```
sysbench cpu --threads=1 --time=10 run
```

<table class="has-fixed-layout"><thead><tr><th>Métrique</th><th class="has-text-align-center" data-align="center">Laptop maison</th><th class="has-text-align-center" data-align="center">VPS Contabo</th><th class="has-text-align-center" data-align="center">Différence</th></tr></thead><tbody><tr><td>**Événements/seconde**</td><td class="has-text-align-center" data-align="center">**749,71**</td><td class="has-text-align-center" data-align="center">610,51</td><td class="has-text-align-center" data-align="center">🟢 **23% plus rapide**</td></tr><tr><td>**Latence moyenne**</td><td class="has-text-align-center" data-align="center">**1,33 ms**</td><td class="has-text-align-center" data-align="center">1,63 ms</td><td class="has-text-align-center" data-align="center">🟢 **18% plus réactif**</td></tr><tr><td>**Latence max**</td><td class="has-text-align-center" data-align="center">**2,35 ms**</td><td class="has-text-align-center" data-align="center">20,95 ms</td><td class="has-text-align-center" data-align="center">🟢 **89% plus stable**</td></tr></tbody></table>

#### Mémoire (sysbench memory)

```
sysbench memory --memory-total-size=1G run
```

<table class="has-fixed-layout"><thead><tr><th>Métrique</th><th class="has-text-align-center" data-align="center">Laptop maison</th><th class="has-text-align-center" data-align="center">VPS Contabo</th><th class="has-text-align-center" data-align="center">Différence</th></tr></thead><tbody><tr><td>**Débit**</td><td class="has-text-align-center" data-align="center">**3925,56 MiB/sec**</td><td class="has-text-align-center" data-align="center">2526,36 MiB/sec</td><td class="has-text-align-center" data-align="center">🟢 **55% plus rapide**</td></tr><tr><td>**Temps d’exécution**</td><td class="has-text-align-center" data-align="center">**0,26 sec**</td><td class="has-text-align-center" data-align="center">0,40 sec</td><td class="has-text-align-center" data-align="center">🟢 **35% plus rapide**</td></tr></tbody></table>

#### Stockage (dd tests)

```
dd if=/dev/zero of=testfile bs=1M count=1024 conv=fdatasync
dd if=testfile of=/dev/null bs=1M count=1024
```

<table class="has-fixed-layout"><thead><tr><th>Métrique</th><th class="has-text-align-center" data-align="center">Laptop maison</th><th class="has-text-align-center" data-align="center">VPS Contabo</th><th class="has-text-align-center" data-align="center">Différence</th></tr></thead><tbody><tr><td>**Écriture**</td><td class="has-text-align-center" data-align="center">**342 MB/s**</td><td class="has-text-align-center" data-align="center">131 MB/s</td><td class="has-text-align-center" data-align="center">🟢 **161% plus rapide**</td></tr><tr><td>**Lecture**</td><td class="has-text-align-center" data-align="center">**7,4 GB/s**</td><td class="has-text-align-center" data-align="center">1,0 GB/s</td><td class="has-text-align-center" data-align="center">🟢 **640% plus rapide**</td></tr></tbody></table>

### Le verdict

En utilisant mon laptop au lieu du VPS Contabo, j’économise 5€36 par mois soit **64€32 par an** pour des **performances supérieures**. En déduisant la consommation électrique (environ 50KWh sur un an, soit ~8/10€ TTC) on est toujours dans le vert avec **une économie réelle d’au moins 55€ sur l’année**.

D’autant que mon laptop d’entrée de gamme **surpasse le VPS payant** dans tous les domaines :

- CPU **23% plus performant** par cœur, avec un cœur supplémentaire
- RAM **55% plus rapide** (par Go, et j’en ai 2,5 x plus)
- Stockage **2 à 7 fois plus rapide**
- Latence **9 fois plus stable**

## Le mot de la fin

Avant de sortir la carte bleue pour un VPS de test ou développement, vérifie si votre vieux laptop ne fait pas déjà mieux que ce que tu vas payer !

Transformer un vieux PC en serveur personnel, c’est **bien plus qu’un simple recyclage**. Mon petit i3-7020U avec ses 20 Go de RAM fait aujourd’hui tourner une infrastructure qui coûterait facilement 80 à 100 € par mois en serveur et en services cloud. En quelques mois, j’économise bien plus que le prix d’achat initial du laptop.

Mais au-delà de l’aspect économique, c’est une **véritable leçon d’autonomie numérique**. Avec Ubuntu Server, Docker et cette stack d’outils open source, tu disposes d’une infrastructure moderne qui n’a rien à envier aux solutions commerciales. Grafana rivalise avec Tableau, Prometheus remplace New Relic, Traefik fait mieux que beaucoup de reverse proxy payants.

Ce projet, simple à démarrer, grandit avec tes besoins. Mon serveur héberge maintenant mes projets de développement, mes sauvegardes automatiques, mon cloud personnel avec Nextcloud, et des outils d’automatisation comme n8n. Tout ça tourne 24h/24 sans broncher, sur une machine que je n’utilsais plus. Et je ne regrette pas les heures passées à chercher des solutions, à installer, à tester avant d’avoir mon mini serveur.

### Ce que toi tu gagnes

- **Tu fais des économies** : fini les 6€/mois pour un serveur de test basique, et les dizaines d’euros supplémentaires pour les services que tu héberges
- **Tu montes en compétences :** en administration système, Docker, monitoring
- **Tu gagnes en autonomie** : Tes données t’appartiennent, tes services aussi
- **Et en évolutivité** : ton infrastructure prête pour tous tes futurs projets
- **Tu as la satisfaction** et le plaisir de faire du neuf avec de l’ancien

## Aller plus loin

Ton serveur personnel est maintenant prêt à t’accompagner dans tes projets. N’hésite pas à expérimenter, ajouter des services, optimiser les performances. C’est ton terrain de jeu, sans limite de temps ni restriction de quota.

L’investissement initial (quelques heures de configuration) te fera économiser plusieurs centaines d’euros par an, tout en te donnant une maîtrise complète de ton infrastructure numérique.