---
title: "n8n - l'interface ultime pour l'automatisation ?"
date: "2025-07-30T13:57:09+00:00"
lastmod: "2025-07-30T13:57:09+00:00"
draft: false
description: "n8n : trois caractères que je voyais passer régulièrement sans jamais comprendre pourquoi tout le monde en parlait. Il a fallu que je prenne le temps de vraiment m'y intéresser pour réaliser ce que je ratais. Spoiler : ça valait le détour."
tags:
  - automatisation
  - n8n
categories:
  - Tutoriels
author: githubcopilotchallenge
---

Des mois que j’entendais parler de **n8n**. Et je n’en voyais pas l’utilité. En fait, je ne comprenais même pas à quoi ça servait.

Mais à force d’en entendre parler, j’ai fini par me documenter. Histoire de ne pas mourir idiot. Du coup j’ai compris son utilité, et j’ai même appris à m’en servir.

Et dans cet article, je vais te montrer, à toi aussi, comment tirer parti de cet outil.

Mais avant tout, tu te demandes peut-être…

## n8n, c’est quoi exactement ?

**n8n** est une **plateforme d’automatisation open source** qui permet de connecter différentes applications, services et API entre elles pour automatiser des tâches, des workflows ou des processus métiers.

Autrement dit :

- C’est un **Zapier open source et auto-hébergeable**, souvent **plus puissant et flexible**, surtout si tu veux tout contrôler toi-même.
- C’est un outil qui **relie automatiquement** tes apps entre elles, comme une **connexion intelligente** entre tes outils du quotidien. Parfait pour **gagner du temps, sans coder**. Idéal si tu n’es pas développeur !

Pour te donner un exemple concret de ce que tu peux faire avec n8n, quand un client s’inscrit sur ton site, tu peux créer un nouveau contact dans ton CRM, lui envoyer un courriel de bienvenue, envoyer un message à tes collaborateurs sur un canal Slack dédié, l’ajouter à une feuille Google Sheet… tout ça sans avoir à rien faire, automatiquement, dès que tu as défini les actions à mener.

Si tu n’as qu’un nouveau client par mois, tu peux te permettre de gérer manuellement, en moins de 10 minutes tu as terminé. Sur un an, tu y auras passé 2 heures peut-être, l’intérêt n’est pas flagrant.

Mais imagine que tu aies ne serait-ce que 3 clients par jour à enregistrer.

À raison de 5 minutes par client, tu y passes 1/4 d’heure. Tous les jours. Sur une semaine, 75 minutes. Sur un mois, 5 heures. Sur un an, en comptant 11 mois à travailler (tu es supposé prendre des vacances), 55 heures.

En automatisant, tu auras peut être passé quelques heures à tout mettre en place, mais ensuite, c’est un **gain net** de 5 heures par mois que tu peux consacrer à des tâches à plus forte valeur ajoutée, comme l’acquisition de nouveaux clients. Et si vous êtes ne serait-ce que 3 à ce poste dans l’entreprise, le gain est exponentiel. C’est **plus d’un mois** de travail gagné en automatisant quelques tâches simples.

Et le truc génial, c’et que tout le monde peut se servir de n8n : les développeurs pour automatiser des tâches complexes, les entreprises pour intégrer leurs outils sans coder, les indépendants pour gagner du temps, mais aussi les data engineers pour orchestrer des pipelines simples. **Une sorte d’outil universel**, puissant et facile à manipuler.

Et n8n n’est pas seulement un Zapier gratuit mais une vraie plateforme d’automatisation métier, ses usages en entreprise le prouvent :

**Gestion commerciale**

- Lead nurturing : nouveau prospect → ajout CRM → séquence d’emails personnalisés → assignation d’un commercial
- Suivi commandes : commande WooCommerce → création facture → envoi transporteur → notification client + équipe

**RH et onboarding**

- Nouveau salarié → création de comptes (Slack, Google Workspace, etc.) → envoi d’un kit de bienvenue → planification de formations
- Absence déclarée → validation manager → mise à jour du planning → notification de l’équipe

**Support client**

- Ticket support → classification automatique → assignation expert → escalade si délai dépassé
- Avis client négatif détecté → alerte équipe → création s’une tâche de suivi

**Marketing/Communication**

- Nouveau contenu sur le blog → publication réseaux sociaux programmée → ajout dans la newsletter → analyse de l’engagement
- Webinar → emails relance → synchronisation CRM → rapport de participation

**Finance/Comptabilité**

- Facture reçue → extraction des données via OCR → validation → comptabilisation → archivage
- Seuil de stock atteint → commande fournisseur → validation achat → suivi livraison

**IT/Monitoring**

- Alerte serveur → diagnostic automatique → création ticket → notification équipe si critique
- Nouvel employé → provisioning automatique de comptes → politique de sécurité → audit accès

Et l’automatisation avec n8n peut s’appliquer à pratiquement tous les secteurs :

- **Santé** : Prise RDV → rappels patients → synchronisation planning → facturation assurance
- **Éducation** : Inscription étudiants → création comptes → planning cours → suivi assiduité
- **Immobilier** : Nouveau bien → publication multi-plateformes → planification visites → suivi prospects
- **BTP** : Devis validé → commande matériaux → planning équipes → facturation étapes
- **Associations** : Adhésion → envoi carte → newsletter → relance cotisation
- **Restauration** : Réservation → confirmation → rappel → enquête satisfaction
- **Transport/Logistique** : Commande → optimisation tournées → suivi livraison → facturation
- **Industrie** : Commande → planification production → suivi qualité → expédition

En fait, dès qu’il y a des **processus répétitifs** et des **données qui circulent entre systèmes**, n8n peut apporter de la valeur. Peu importe le secteur.

C’est l’équivalent de Zapier, mais c’est aussi son contraire. Mêmes outils, même finalité, mais un modèle financier différent. Un tableau te permettra de visualiser plus facilement les différences entre les deux :

<table><thead><tr><th>Outil</th><th class="has-text-align-center" data-align="center">Open source ?</th><th class="has-text-align-center" data-align="center">Auto-hébergement ?</th><th class="has-text-align-center" data-align="center">Gratuit ?</th><th class="has-text-align-center" data-align="center">Notes</th></tr></thead><tbody><tr><td>**n8n**</td><td class="has-text-align-center" data-align="center">✅<span style="color: var(--ast-global-color-3); font-family: inherit; font-size: inherit; font-weight: inherit;">Oui</span></td><td class="has-text-align-center" data-align="center">✅ Oui</td><td class="has-text-align-center" data-align="center">✅ (self-hosted)</td><td class="has-text-align-center" data-align="center">Puissant, technique, flexible</td></tr><tr><td>**Zapier**</td><td class="has-text-align-center" data-align="center">❌ Non</td><td class="has-text-align-center" data-align="center">❌ Non</td><td class="has-text-align-center" data-align="center">❌ (freemium)</td><td class="has-text-align-center" data-align="center">Très simple, mais payant et fermé</td></tr></tbody></table>

Et même si Zapier reste une référence incontestable, n8n a lui aussi des arguments pour séduire :

- il est **open source**, **auto-hébergeable** et **gratuit**
- son **interface graphique** (drag &amp; drop) te permet de construire des workflows **visuellement**, sans écrire une ligne de code – elle est plus avancée, plus libre, plus technique et plus flexible que celle de Zapier
- chaque étape du workflow est un nœud (“node”), avec des connecteurs pour Gmail, Outlook, Slack, Discord, Microsoft Teams, Google Sheets, Airtable, Notion, Trello, Shopify, WooCommerce, GitHub, Jira, et des centaines d’autres services
- il est **personnalisable avec JavaScript** ou **Python** (tu insères une fonction dans un nœud) et **extensible** (tu peux **créer tes propres nœuds** en Typescript)

Mais n8n n’est pas sans inconvénients :

- **Courbe d’apprentissage plus raide**
    Même si l’interface est bien faite, n8n demande de comprendre la logique des nœuds, des inputs/outputs, des variables… Ce n’est pas du drag &amp; drop simpliste.
- **Tu gères l’hébergement (et donc les pannes)**
    Si tu veux du gratuit, tu dois avoir un VPS ou un conteneur Docker, gérer les backups, installer les mises à jour manuellement et prévoir une montée en charge si ton usage devient massif. Avec Zapier, tu ne te poses pas la question : tu crées ton compte, tu connectes les apps, tu dors tranquille.
- **Moins de connecteurs “plug and play”**
    Même s’il y en a beaucoup, certains connecteurs dans n8n nécessitent une configuration manuelle d’API et des étapes d’authentification que Zapier gère automatiquement. Tu as plus de liberté, mais aussi plus de friction.
- **Pas (encore) de gestion fine des utilisateurs**
    Si tu veux gérer une équipe dans une interface unique, avec des droits par projet ou workflow, n8n self-hosted ne te le permet pas facilement (sauf avec la version “Enterprise” ou des contournements). Mais rien ne t’empêche de créer plusieurs instances — une par équipe, par projet ou par client — chacune isolée dans son propre environnement. C’est simple à faire avec Docker, et ça permet une séparation claire des usages.
- **Pas de dashboard de suivi complet intégré**
    Tu n’as pas de console native pour suivre les exécutions ratées, les quotas, les logs centralisés propres, ni les alertes en cas d’échec.

Voici un tableau comparatif plus complet des deux outils :

<table><thead><tr><th class="has-text-align-left" data-align="left">Critère</th><th class="has-text-align-center" data-align="center">**n8n**</th><th class="has-text-align-center" data-align="center">**Zapier**</th></tr></thead><tbody><tr><td class="has-text-align-left" data-align="left">**Prise en main**</td><td class="has-text-align-center" data-align="center">Moins intuitive au départ</td><td class="has-text-align-center" data-align="center">Interface ultra-simplifiée</td></tr><tr><td class="has-text-align-left" data-align="left">**Courbe d’apprentissage**</td><td class="has-text-align-center" data-align="center">Moyenne à élevée selon les cas</td><td class="has-text-align-center" data-align="center">Très faible, tout est guidé</td></tr><tr><td class="has-text-align-left" data-align="left">**Maintenance / Hébergement**</td><td class="has-text-align-center" data-align="center">À ta charge si self-hosted</td><td class="has-text-align-center" data-align="center">Aucune gestion côté utilisateur</td></tr><tr><td class="has-text-align-left" data-align="left">**Fiabilité / monitoring**</td><td class="has-text-align-center" data-align="center">Tu dois mettre en place toi-même</td><td class="has-text-align-center" data-align="center">Suivi natif avec alertes mail</td></tr><tr><td class="has-text-align-left" data-align="left">**Multi-utilisateur / droits**</td><td class="has-text-align-center" data-align="center">Basique (1 utilisateur par défaut)</td><td class="has-text-align-center" data-align="center">Complet (équipe, rôles, accès limités)</td></tr><tr><td class="has-text-align-left" data-align="left">**Connecteurs métiers**</td><td class="has-text-align-center" data-align="center">Beaucoup, mais parfois moins poussés</td><td class="has-text-align-center" data-align="center">Très nombreux, bien intégrés</td></tr><tr><td class="has-text-align-left" data-align="left">**Webhooks / Polling**</td><td class="has-text-align-center" data-align="center">Plus souple, mais plus “brut”</td><td class="has-text-align-center" data-align="center">Intégré, simple à activer</td></tr><tr><td class="has-text-align-left" data-align="left">**Scalabilité / haute disponibilité**</td><td class="has-text-align-center" data-align="center">Possible, mais à construire</td><td class="has-text-align-center" data-align="center">Géré nativement par Zapier</td></tr><tr><td class="has-text-align-left" data-align="left">**Support / assistance**</td><td class="has-text-align-center" data-align="center">Communautaire ou plan entreprise</td><td class="has-text-align-center" data-align="center">Support client intégré, livechat</td></tr><tr><td class="has-text-align-left" data-align="left">**Prix**</td><td class="has-text-align-center" data-align="center">Gratuit si hébergé soi-même</td><td class="has-text-align-center" data-align="center">Freemium, mais vite payant</td></tr></tbody></table>

**Zapier peut sembler gratuit… mais seulement en apparence.**
Dès que tu veux un peu de logique conditionnelle, plus de 2 – 3 connexions, ou une exécution toutes les 5 minutes, **tu passes très vite aux plans payants**.
Et si tu montes en charge (workflows complexes, volume important), **tu peux dépasser les 100€/mois sans t’en rendre compte**.

Dès que tu dépasses le stade du hobby ou que tu veux de l’automatisation sérieuse (multi-étapes, filtres, branchements conditionnels, accès à la logique métier…), la facture monte très vite.

Zapier pratique en fait une stratégie de pricing purement commerciale (c’est une entreprise) :

- **Plan gratuit** : 100 tâches/mois seulement, workflows basiques
- **Pro à partir de 17€/mois** : là ça devient intéressant avec 750 tâches par mois et l’accès à toutes les fonctionnalités
- **Team à 60€/mois** pour le multi-utilisateur (jusqu’à 25) et 2000 tâches mensuelles
- **Enterprise** – pas de prix affiché, vous avez un négociateur au bout du fil. Prévoyez un défibrilateur – juste au cas où…

Et le piège : une tâche = une action. Un workflow simple comme celui que je prends en exemple (webhook → récup users → filtre → IA → email) = **5 tâches par article publié**. Avec 3 articles/semaine, tu exploses vite le quota gratuit, tu es déjà à 60 tâches sur un mois. Pour 1 abonné. Plus 1 tâche par abonné supplémentaire. Et en plus, vu que j’utilise une logique conditionnelle, tu as besoin au minimum du plan Pro à **17€42/mois** (si paiement annuel, 26€13 sinon). Hors taxes, cela va sans dire. 😉 Mettre en production un tel workflow, gratuit avec n8n, te coûterait a minima 209€04 HT par an chez Zapier (soit 250€85 TTC).

À l’inverse, **n8n est gratuit si tu l’auto-héberges**, et ses limitations ne sont pas commerciales : ce sont celles que tu choisis de t’imposer (ressources serveur, quota API des services connectés, etc.). Mais la gratuité a un coût : celui du temps et des compétences nécessaires pour l’hébergement et la maintenance.

Ok. Tout ça c’est bien, mais un exemple concret d’automatisation, tout simple, juste pour illustrer le propos, c’est mieux encore.

Pour reproduire l’exemple, tu auras besoin d’un compte [n8n](https://n8n.io/) (un essai gratuit de 15 jours te permet de tester l’application et son intérêt pour toi, avant de passer à un plan payant : à partir de 20€ HT/mois en version Starter, de 50 en version Pro – ou de l’auto-héberger), et d’un site WordPress, sur lequel tu installeras l’extension [WP Webhooks](https://wordpress.org/plugins/wp-webhooks/). Comme toujours, je te recommande d’effectuer tes essais sur un site de test ou mieux, [une machine virtuelle](https://www.virtualbox.org/) ou un VPS de test, histoire de ne pas casser un site ou un serveur en production…

Si tu n’as jamais installé d’extension WordPress, je te renvoie à un article sur mon blog *Tout sur WordPress* : [Installer, activer, mettre à jour et supprimer une extension WordPress](https://tsw.ovh/guide-extension-wordpress/). Tu verras, ce n’est pas sorcier. L’article date de 2017, mais il est toujours d’actualité, le modus operandi pour installer une extension n’a pas changé.

## Héberger n8n en local ou sur un VPS via Docker Compose

Tu veux tester n8n sans t’inscrire à un service cloud ? Tu peux l’auto-héberger très facilement avec Docker. Voici un exemple de configuration de base :

```
services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    ports:
      - "5678:5678"
    volumes:
      - ./n8n_data:/home/node/.n8n
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=mon-Mot~2PasseFort
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
    restart: unless-stopped
```

À savoir :

- Par défaut, n8n ne met pas de mot de passe. Ici, on active une authentification basique.
- Les données sont stockées dans un volume local (./n8n\_data).
- Pour la prod, prévois un reverse proxy (Traefik ou Nginx), un nom de domaine et un certificat SSL.

Une fois le fichier `docker-compose.yml` créé, tu lances depuis ta console SSH :

```
docker compose up -d
```

Et tu accèdes à ton interface n8n sur `http://localhost:5678`.

## Prise en main de n8n

Avant de passer à l’exemple lui même, je t’explique rapidement l’interface de n8n.

Chez n8n, tout est nœud, et chaque nœud est une action, un traitement, un appel d’API ou un bloc logique (if, loop, etc.).

Dans ton interface vierge n8n, tu as un bouton + en haut à droite de l’écran, et un tout au centre. C’est avec l’un de ces boutons que tu vas ajouter le premier nœud. Ensuite, tu auras un bouton plus à droite de chaque nœud, si tu cliques dessus, tu obtiendras un écran de ce type :

[

![n8n – ajouter un nœud](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-add-nodes.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-add-nodes.jpg)Vu le nombre de nœuds disponibles (plusieurs centaines), tu peux faire une recherche pour trouver ce que tu veux – tu saisis *AI* et tous les nœuds incluant *AI* vont apparaître – dont *Airtable*, *AI agent* et même *Wait*.

À chaque fois, tu auras **3 blocs principaux** dans ta vue (comme tu le verras plus bas sur le capture d’écran *n8n* : *n8n webhook*) :

1. le bloc central, où tu effectues les réglages
2. à gauche, les données reçues, que tu vas traiter dans ton bloc
3. à droite, les données en sortie, une fois le traitement appliqué

J’ai rajouté un 4ième bloc, qui n’en est pas vraiment un, mais qui est important lui aussi en phase de test : c’est un “menu” du bloc 3 qui te permet d’effectuer une recherche dans la sortie obtenue, de garder la sortie pour pouvoir l’utiliser dans l’ensemble des opérations qui suivent (pin/unpin), de visualiser tes données au format json, sous forme de tableau ou encore de schéma.

Maintenant que tu sais tout ce qu’il y a à savoir sur n/n, sur WordPress, sur l’environnement de test, on peut passer à…

## Un exemple pour prendre en main n8n

On va prendre un exemple simple, le but ici c’est de comprendre le concept. Un workflow simple, mais pas inutile non plus… comme par exemple l’envoi d’un email à vos abonnés après publication d’un article de blog. Avant de nous attaquer à la réalisation, nous allons passer par une phase dite de…

### Visualisation conceptuelle

Mieux vaut réfléchir en amont, ce qui nous évitera d’oublier une étape lors de la construction dans n8n. Voici le détail du workfow :

```
graph TD
    A[Enregistrement de l'article WordPress] --> B(Envoi du webhook à n8n);
    B --> C(Récupération des données de l'article);
    C --> D(Récupération de la liste des abonnés);
    D --> E{Y a-t-il des abonnés?};
    E -- Oui --> F[Génération d'un résumé par un LLM];
    F --> G[Envoi du mail via SMTP];
    G --> H[Workflow terminé];
    E -- Non --> H[Workflow terminé];
```

Maintenant que tu sais où tu va, tu peux te lancer dans l’aventure, avec…

### La construction de ton workflow

n8n n’est pas un outil magique, il faut lui donner la matière première, d’une manière ou d’une autre. Dans notre exemple, c’est une action dans WordPress qui va tout déterminer. Mais pour que le workflow fonctionne comme prévu, il faut en bousculer l’ordre quand on le construit, les étapes de construction ne seront donc pas forcément dans l’ordre déterminé lors de la visualisation conceptuelle. La toute première étape, pour nous, ce sera la…

#### Récupération des données de l’article

Dès qu’un article est mis à jour, WordPress envoie un flux de données à n8n, qui le traite. Ce qui déclenche les actions suivantes, toutes conditionnées par la première (forcément). Pour que n8n écoute, il lui faut un Webhook, c’est le point d’entrée du Workflow.

Double clique sur le bloc pour ouvrir une fenêtre modale et gérer le webhook (c’est le comportement habituel de n8n, tu déclares un élément, tu double cliques pour les réglages (insertion de code, ajout d’identifiants, ajout d’actions et d’options…). Tu as maintenant ta fenêtre modale :

[

![n8n webhook](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-webhook.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-webhook.jpg)En phase de création / test, tu cliques sur *Listen for test event* (tu l’as dans les blocs 1 et 2).

Tu retournes dans l’admin WordPress, tu vas dans *Réglages &gt; WP Webhooks*, en haut de ta page tu cliques sur *Send data*, dans le champ de recherche *Search triggers* tu commences ta saisie avec `post`, tu sélectionnes *Post created*. Tu cliques sur *Add Webhook URL*(1), tu nommes ton webhook (2), tu renseignes l’URL de test, pour le moment (3). Tu vas dans le menu à trois points verticaux à droite de ton nouveau hook, tu cliques sur *Send demo*. Si tout es bien réglé, tu vas avoir le bloc 3 qui ressemble étrangement à celui de la capture d’écran (normal, ce sont les données de démo).

[

![](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/wp-webhooks-add-webhook.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/wp-webhooks-add-webhook.jpg)Une fois que tu as réglé le webhook, on peut passer à…

#### Enregistrement de l’article WordPress

Là, sérieusement, tu veux que je détaille ? 😉 On peut passer au point suivant…

#### Envoi du webhook à 8n8

Difficile de faire plus simple ! Le workflow n8n démarre après l’envoi du webhook… déclenché par le changement d’état de l’article : quand tu cliques sur *Publier*, l’article passe automatiquement de *Brouillon* à *Publié*, et c’est précisément ce qui déclenche l’envoi du webhook à n8n. Simple, via un plugin, celui que je t’ai demandé d’installer et d’activer précédemment (WP Webhooks). Donc, tu n’as rien à faire de spécial. Juste à enregistrer ton article. Le plugin fait le reste.

#### Récupération de la liste des abonnés

[

![n8n - WordPress Get many users](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-get-many-users.png)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-get-many-users.png)Pour récupérer les utilisateurs, il faut d’abord configurer un compte WordPress. Tu cliques sur le crayon à côté de *Credential to connect with*. La fenêtre ci-dessous apparaît :

[

![n8n - configuration d'un compte WordPress](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-configure-wordpress.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-configure-wordpress.jpg)Tu vas avoir besoin d’un mot de passe d’application. Pour le créer, va dans l’administration de ton site WordPress, à *Comptes &gt; Administrateur/administratrice* et sélectionne ton compte ou un compte avec les mêmes droits. S’il ne s’agit pas d’un site perso, tu devrais même créer un compte administrateur pour ce type d’actions (nomme le *automatisation* par exemple), ça évitera de casser l’appli si ton compte à toi est supprimé. Rends toi à la section suivante :

[

![WordPress - Mot de passe applicatif](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/wordpress-mot-de-passe-application.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/wordpress-mot-de-passe-application.jpg)Une fois que tu as ton mot de passe d’application, retourne dans n8n.

1. Commence par nommer le compte, surtout si tu n’en as pas qu’un. *Compte WordPress 1*, ça ne te dit pas de quel compte il s’agit.
2. Renseigne ton identifiant.
3. Puis ton mot de passe d’application.
4. Enfin l’URL.

Une fois la connexion WordPress enregistrée, tu peux retourner à ton nœud *Get Many Users*. Le bloc 1 ne te sert pas, par contre tu as tout à voir dans le bloc 3. C’est là que se trouve ce qui te servira plus tard dès que tu as cliqué sur *Execute step* : les *first\_name* et *email*.

Et on passe au troisième nœud, un nœud *Code*, parce qu’il faut parfois en passer par là. n8n supporte le *javascript* et maintenant, *Python* à titre expérimental. Pour notre exemple, on va utiliser javascript.

[

![n8n - nœud code](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-code.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-code.jpg)Insère le code suivant (dans le bloc 2) :

```
// Filtrer les utilisateurs qui ont le rôle "subscriber"
const subscribers = items.filter(item => {
  const roles = item.json.roles;
  return Array.isArray(roles) && roles.includes('subscriber');
});

// Créer un tableau d'objets { email, display_name }
const emails = subscribers.map(item => ({
  email: item.json.email,
  display_name: item.json.name || item.json.display_name || 'Abonné'
}));

// Vérifier s'il y en a au moins un
const hasSubscriber = emails.length > 0;

// Sortie
return [{
  json: {
    hasSubscriber,
    emails // <-- maintenant un tableau d'objets [{email, display_name}, ...]
  }
}];
```

Et tu obtiens un tableau `json` des abonnés et de leurs noms. Utile pour envoyer un email personnalisé. Et pour vérifier qu’il y a au moins un abonné. Ce qui nous emmène au point suivant…

#### Y a-t-il des abonnés?

Ce nœud ne permet qu’une chose : une suite conditionnelle, gérée via un nœud *if*. Si la condition est vraie, on poursuit le flux. Si elle est fausse, dans notre cas, on s’arrête. Inutile de déclarer une fin d’application, sauf pour informer d’une absence d’abonnés que normalement tu n’ignores pas. 😐

[

![n8n - nœud if](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-if.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-if.jpg)Tu cliques dans le bloc 1 sur une donnée, tu le glisses dans le champ *Conditions* du bloc 2. En fonction du type de donnée, le bloc 2 va se modifier. Tu vas glisser sur la donnée `hasSubscriber`, et ton choix va se porter sur `is true` ou `is false` dans une liste déroulante. Dans notre cas, il y a un abonné (il valait mieux pour l’exemple), on continue avec la…

#### Génération d’un résumé par un LLM

C’est sans aucun doute la partie la plus touffue de cet exemple.

Tu commences par ajouter un nœud *Edit Fields*. C’est un des nœuds essentiels pour nous, c’est ici qu’on va agencer les données reçues pour les traiter plus tard.

[

![n8n - nœud Edit Field](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-noeud-edit-field.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-noeud-edit-field.jpg)Dans le bloc 1, tu peux choisir le nœud dans lequel tu vas piocher les données. Comme tu le vois dans la capture d’écran, il y a plusieurs origines. Et pour chaque donnée, tu as l’index du tableau `json` que tu obtiendras en sortie, le type de donnée, la donnée et un exemple de la sortie.

Tu cliques sur *Add Field* (en bas du boc 2) – tu as un bloc qui apparaît, pour nommer la donnée, (l’index du `json` généré), définir le type de donnée et enfin la donnée que tu récupères dans le bloc 1.

Dans le bloc 3, tu as la sortie `json`. C’est la donnée que tu utiliseras dans les nœuds suivants. Et le nœud suivant, c’est un nœud *AI Agent*. Je ne te fais pas l’affront de t’expliquer à quoi il sert… 😀

[

![n8n - AI Agent](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-ai-agent.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-ai-agent.jpg)Prend soin de bien rédiger ton prompt, dans lequel que peux insérer des données issues du bloc 1.

Dans un premier temps, tu as à définir le modèle à utiliser, dans le nœud dérivé (le rond en-dessous d’*AI Agent*). C’est un nœud à part entière, que tu choisis en cliquant sur le `+ [model]` du nœud *AI Agent*. En cliquant tu as directement les *Language Models*. Tu choisis ton API, puis tu procèdes comme pour la gestion des identifiants WorPress, avec le crayon en haut à gauche dans le bloc 2, à côté de *Credentials to connect with* – ici j’utilise OpenRouter pour me connecter au LLM.

[

![n8n - AI Agent Chat Model](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-chat-model.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-chat-model.jpg)Tu définis le LLM à utiliser, les options que tu veux gérer (la température par exemple), et c’est tout.

Pour autant, jamais la connexion à un LLM n’aura été aussi simple : tu prends ton nœud *AI Agent*, tu définis l’API à utiliser, tu rédiges ton prompt et c’est terminé. Bon, ça prend un peu plus de temps à faire qu’à lire, mais en moins de 5 minutes, tu as géré.

Une fois les données traitées par l’AI Agent, on passe à la dernière étape…

#### Envoi du mail via SMTP

Tu as à nouveau besoin d’un nœud *Code*, pour formater les données avant l’envoi. Insère le javascript suivant :

```
const emails = $('Edit Fields').item.json.emails;
const postTitle = $('Edit Fields').item.json.post_title;
const aiAgent = $('AI Agent').item.json.output;

return emails.map(e => ({
  json: {
    email: e.email,
    display_name: e.display_name,
    post_title: postTitle,
    ai_agent: aiAgent
  }
}));
```

Et tu peux passer au nœud suivant – le dernier de cet exemple, *Send Email*.

[

![n8n - Send Email](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-send-email.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-send-email.jpg)Encore une fois, il te faut déclarer ton compte email (les données SMTP), exactement comme lorsqe tu utilises Outlook ou Thunderbird – sauf que n8n n’a pas de fonction `autodiscover`.

Tu prends les données du bloc 1, tu les insères là où tu en as besoin dans le bloc 2 (et c’est là que tu retrouve le nom de l’utilisateur, qui te permet de personnaliser ton envoi). Cliques sur Execute step, et miracle, tu vas recevoir ton premier email, qui t’informe de la parution d’un nouvel article.

Ça y est, c’est terminé, tout est fait. Voici le résultat obtenu dans n8n :

[

![workflow n8n](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/workflow-n8n.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/workflow-n8n.jpg)Tu remarqueras de suite que tu as plus d’étapes dans le workflow n8n que dans le workflow conceptuel : c’est normal, il y a des étapes qui nécessitent un post-traitement. Quand tu as récupéré les utilisateurs dans WordPress par exemple, c’était de la récupération brute, il a fallu récupérer le nom et l’adresse email, le reste ne te servait à rien dans le flux.

#### Activation du webhook

Dernière étape : tu as tout géré avec l’url de test, il te faut maintenant retourner au Webhook (le point d’entrée de ton workflow, copier l’url de production et créer un nouveau webhook dans *WP Webhooks* avant de supprimer l’ancien – une fois créé, il n’est pas modifiable. Pas top, mais vu que l’opération ne prend que quelques secondes, ce n’est pas très grave.

Et surtout, **tu actives ton workflow** :

[

![n8n-  activation du workflow](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-activation.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/n8n-activation.jpg)Voilà, tu as un outil prêt à informer tes lecteurs de la publication d’un nouvel article. Mais attention ! Pour l’envoi d’une notification ou d’une lettre d’information, **autant passer par un service externe** tel que JetMail, SendGrid ou Mailchimp. Ton email aura **un autre rendu** et tu ne surchargeras pas ton serveur SMTP.

Mais en changeant quelques paramètres tu pourras envoyer une alerte à ton équipe quand l’article passe en relecture, quand il est publié, quand il est supprimé… Ce ne sera plus un mail pour informer tes lecteurs, mais tes collaborateurs.

## Aller plus loin

Je t’ai détaillé un exemple simple, qui peut te servir sur un blog perso par exemple.

Le code est disponible sur Github : [Alerte Nouvel Article – Workflow n8n](https://github.com/pcescato/Alerte-Nouvel-Article-Workflow-n8n/) sous licence Apache 2. Sens toi libre de le récupérer, de l’adapter, d’en faire ce que tu veux.

Mais en production, en entreprise, il te faudra également prévoir la gestion des erreurs, et des fallbacks en cas d’indisponibilité de ton modèle IA préféré par exemple… tu ne peux pas te permettre de lancer un workflow et de t’en remettre au ciel pour sa bonne exécution.

Si tu veux informer tes lecteurs, tu pourras aussi passer par un `hook` WordPress, ça t’évitera de prendre une extension (plug’n play mais limitée dans ses actions). Tu pourras par exemple décider d’envoyer l’article via `curl`, et prévoir dans n8n de ne pas réinformer si il y a déjà eu un envoi pour cet article…

## Bilan

 n8n fait le job et honnêtement, il le fait bien. Je mets le temps passé à mettre en place ce workflow sur le compte de mon inexpérience avec ce type d’outils – une fois qu’on a compris comment s’en servir, il peut vite devenir addictif pour effectuer des tâches répétitives en entreprise, ou même pour soi.

Cet outil mérite-t-il pour autant le label *ultime* ? Pas vraiment. **n8n est l’un des meilleurs compromis actuels** entre puissance, liberté et coût, mais il n’est pas (encore) l’interface « ultime » pour tout le monde. Il excelle dans certains cas d’usage et reste perfectible dans d’autres.

Il est open source. Il a plus de 350 connecteurs. Et on peut en construire en Typescript. Son coût d’utilisation est quasi-nul. Il a une communauté active, des templates prêts à l’emploi, un forum réactif, des mises à jour mensuelles.

Pour autant, sa courbe d’apprentissage est assez raide, le support n’est disponible qu’en abonnement Pro, tu n’as pas une gestion intuitive des erreurs, son UX mobile n’est pas extraordinaire, il n’est pas franchement scalable…

Alors il est plus qu’intéressant, **presque l’interface ultime** pour un freelance ou petite équipe technique qui veut du gratuit et de la liberté, pour une entreprise avec une culture DevOps ou un service interne capable de maintenir l’infrastructure, pour des data-engineers qui ont besoin d’orchestrer des pipelines ETL/ELT légers.

Par contre les DSI d’une PME sans ressources DevOps préféreront Make ou Zapier. Les grands comptes avec exigences de gouvernance (SSO, RBAC fin, audit logs) opteront soit pour la version Enterprise (payante), soit pour une alternative (Zapier ou Make à nouveau).

C’est **l’interface la plus prometteuse** du marché open-source, mais « ultime » dépend du profil.

Dans l’état actuel, il est :

- **ultime pour les tech-savvy** qui veulent contrôler leur stack
- **très bon mais perfectible** pour les profils métier
- **encore trop DIY** pour les grandes structures non-tech

D’ici 12-18 mois, avec une UX simplifiée et un mode SaaS mature, il pourrait effectivement devenir l’interface de référence.