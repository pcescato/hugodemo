---
title: "Écrire dans WordPress, publier en statique : retour sur un workflow automatisé"
date: "2026-02-07T12:40:15+00:00"
lastmod: "2026-02-07T12:40:15+00:00"
draft: false
description: "Il y a une tension que beaucoup de personnes qui écrivent en ligne connaissent bien. Les générateurs de sites statiques sont rapides, élégants, légers et faciles à déployer presque partout...."
categories:
  - Uncategorized
author: githubcopilotchallenge
---

Il y a une tension que beaucoup de personnes qui écrivent en ligne connaissent bien.

Les générateurs de sites statiques sont rapides, élégants, légers et faciles à déployer presque partout.

Mais publier un blog avec ces générateurs n’est pas à la portée du premier venu. Entre l’interface d’écriture (inexistante) et la configuration, la mise en place, les thèmes, le déploiement… autant d’étapes hors de portée du blogueur lambda.

Et au fil du temps, on passe souvent plus de temps à ajuster son outil de publication qu’à écrire réellement.

À l’inverse, WordPress reste probablement l’un des meilleurs environnements pour rédiger, structurer et organiser des contenus.

L’interface est mature, stable, efficace. Elle permet de se concentrer sur l’essentiel : écrire.

De là vient sans doute son succès et son adoption massive.

Le problème, c’est tout le reste.

Exposer un site WordPress complet côté public implique un hébergement convenable, une forte dépendance aux plugins — au minimum pour la sécurité et le SEO — et une maintenance sérieuse.

Et de base, les performances sont variables, pour ne pas dire à peine passables.

## La recherche d’une alternative

C’est ce constat qui m’a progressivement amené à chercher une solution différente.

Depuis quelque temps déjà, j’ai commencé à déplacer la publication de mes contenus vers des sites statiques.

Mais ce que je voulais, au fond, était plus simple : conserver WordPress comme environnement d’écriture, tout en supprimant complètement sa présence côté public.

À l’heure où des sites statiques peuvent être déployés en quelques secondes sur presque n’importe quelle infrastructure, garder WordPress en frontal n’a plus toujours beaucoup de sens — pour peu que l’on dispose d’une solution de déploiement robuste.

Écrire normalement. Publier automatiquement.

Sans export manuel, sans script à lancer, sans friction.

## Les outils existants : efficaces mais lourds

Ayant des dizaines d’articles sur mes blogs WordPress, j’ai développé une suite d’outils en Python capables d’exporter un site complet vers Hugo ou Astro.

Fonctionnels, fiables, mais basés sur une logique d’export global : génération complète du site, transformation, puis déploiement.

Un processus efficace, mais lourd.

Et surtout peu naturel dans un flux d’écriture quotidien.

C’est cette recherche d’un workflow éditorial plus fluide qui a donné naissance au projet.

## Une construction rapide

Le plus surprenant n’est pas tant le résultat que la vitesse à laquelle il a pris forme.

Entre les premières spécifications et un plugin réellement fonctionnel, moins de quarante-huit heures se sont écoulées.

Non pas parce que tout a été généré automatiquement, mais parce qu’une grande partie du travail consistait à définir clairement ce qui devait être construit, puis à piloter son exécution.

Pour mettre en place ce workflow, j’ai utilisé GitHub Copilot CLI.

**Note** : Il ne s’agit pas de l’autocomplétion dans l’éditeur, mais d’un outil en ligne de commande capable de générer des fichiers complets à partir de prompts structurés.

Pas comme un générateur de code chargé de “faire le travail” avec un prompt magique, mais comme un partenaire d’exécution capable d’accélérer la mise en place d’un système déjà pensé, réfléchi et structuré.

## Mon expérience avec GitHub Copilot CLI

Lorsque j’ai commencé ce projet, je ne cherchais pas un outil capable de coder à ma place.

Je cherchais un moyen d’accélérer l’exécution d’un projet dont l’architecture était déjà claire.

Ayant déjà utilisé GitHub Copilot CLI, Gemini CLI et divers LLM sur d’autres projets, je savais que ces outils pouvaient produire du code rapidement.

Mais je savais aussi que sans cadre précis, ils produisent surtout… du code.

Pas nécessairement un système cohérent.

La première étape n’a donc pas été de coder. La première étape a été de rédiger les spécifications. De définir le périmètre. De découper le projet en blocs fonctionnels.

D’identifier les contraintes non négociables : WordPress natif uniquement, pas d’exécution shell, traitement asynchrone fiable, commits GitHub atomiques, conformité WordPress.org pour pouvoir publier le plugin dans le dépôt officiel WordPress et en faire profiter la communauté.

Puis d’organiser le développement en étapes successives.

Ce travail ressemble beaucoup à ce que ferait un chef de projet technique avant de confier l’implémentation à une équipe. La différence, ici, est que l'”équipe” était constituée d’un outil capable de produire du code très vite — mais uniquement si les instructions étaient claires et précises.

Je n’ai donc pas écrit de code au sens traditionnel. J’ai rédigé un cahier des charges fonctionnel et technique, des prompts, affiné des instructions, corrigé des trajectoires.

Chaque étape consistait à décrire ce qui devait être construit, à vérifier ce qui était produit, puis à ajuster.

Parfois, Copilot proposait une structure pertinente dès la première tentative. Parfois, il fallait reprendre, préciser, contraindre davantage.

### Le schéma de travail

Très vite, un schéma de travail s’est installé : spécification → génération → vérification → correction → itération.

Dans ce processus, Copilot se comporte moins comme un générateur magique que comme un exécutant rapide.

Il peut structurer une classe entière en quelques secondes, proposer une implémentation cohérente ou refactoriser un bloc complet.

Mais il peut aussi oublier un hook essentiel, écraser une méthode existante ou produire du code fonctionnel mais non conforme aux contraintes initiales.

À plusieurs reprises, le plugin a cessé de fonctionner après une modification pourtant “correcte” en apparence.

Une méthode remplacée par un stub incomplet. Un hook non enregistré. Un fichier généré sans être réellement écrit sur le disque. Ou encore une erreur fatale à l’activation, typique d’un environnement WordPress strict.

Chaque incident obligeait à revenir aux fondamentaux : vérifier, comprendre, corriger, reformuler.

### Ce que cela implique

C’est probablement l’aspect le plus intéressant de l’expérience.

Utiliser Copilot efficacement ne consiste pas à écrire un prompt unique et attendre un résultat.

Cela ressemble beaucoup plus à un pilotage continu, où la qualité des instructions conditionne directement la qualité de ce qui est produit.

Dans ce cadre, l’outil devient particulièrement efficace pour accélérer tout ce qui est structuré : création de classes, organisation des fichiers, implémentation de fonctions répétitives, refactoring, documentation.

Dès que l’objectif est clairement défini, l’exécution peut devenir très rapide.

Mais la responsabilité de l’architecture, des choix techniques et de la cohérence globale reste entièrement humaine.

Au final, l’expérience ressemble moins à du “développement assisté par IA” qu’à une forme de direction technique assistée.

Le code est produit rapidement, mais il doit être pensé, encadré et validé en permanence.

Ce projet a été construit en moins de deux jours. Pas parce que l’outil remplace le travail de conception, mais parce qu’une fois ce travail effectué, l’exécution peut être considérablement accélérée.

C’est probablement là que GitHub Copilot CLI devient le plus intéressant : il n’est pas un substitut au développement, mais l’accélérateur d’un projet déjà pensé et structuré.

## Le workflow en pratique

Le workflow est désormais opérationnel.

Les articles sont rédigés dans WordPress, comme auparavant.

Au moment de la publication ou de la mise à jour, un plugin dédié déclenche automatiquement la synchronisation vers un dépôt GitHub.

Chaque contenu est converti en Markdown avec un front matter spécifique à Hugo, accompagné de ses images optimisées (WebP et AVIF).

L’ensemble est envoyé dans un commit unique via l’API GitHub.

Un workflow GitHub Actions prend ensuite le relais : génération du site statique, puis déploiement sur GitHub Pages.

Concrètement, publier dans WordPress suffit désormais à mettre en ligne une version statique complète du site, sans export manuel ni intervention supplémentaire.

### Architecture technique

Le dépôt contient :

- Le plugin WordPress (conformité WordPress.org)
- L’intégration avec l’API GitHub (commits atomiques)
- La gestion asynchrone des synchronisations (Action Scheduler)
- La génération Markdown compatible Hugo
- Le workflow GitHub Actions utilisé pour le déploiement

### Captures du fonctionnement

Le processus en images :

1. **Rédaction** : Interface WordPress habituelle, aucun changement dans l’expérience d’écriture
2. **Commit automatique** : Le dépôt GitHub reçoit le Markdown, les images optimisées et le front matter
3. **Arborescence Hugo** : Structure `content/posts/` générée automatiquement avec la nomenclature correcte
4. **Site déployé** : Version statique en ligne via GitHub Pages, performances optimales

L’ensemble forme une chaîne de publication simple : écrire dans WordPress, publier, et laisser le reste s’exécuter.

Le code source complet est disponible sur GitHub : \[lien à ajouter\]

## Réflexions

Au départ, l’objectif n’était pas de tester une IA ni de construire un plugin pour construire un plugin. Tester, je l’ai déjà fait. Et construire pour construire, où est l’intérêt ?

Non, il s’agissait de retrouver un flux d’écriture naturel, sans renoncer à la performance et à la légèreté d’un site statique. Et un plugin était la méthode la plus pertinente pour y parvenir.

Au fond, ce projet ne raconte pas comment l’IA écrit du code.

Il montre surtout ce qui devient possible lorsque la conception reste humaine et que l’exécution s’accélère.

GitHub Copilot CLI n’a pas remplacé le développement.

Il n’a pas non plus supprimé le besoin de réfléchir, d’architecturer ou de décider.

Mais utilisé comme un partenaire d’exécution plutôt que comme un générateur automatique, il a permis de transformer rapidement une idée claire en un système fonctionnel.

## Conclusion

C’est peut-être là que ces outils prennent réellement leur sens : ils ne modifient pas la manière dont on construit, mais réduisent la distance entre ce que l’on imagine et ce que l’on met en production.

Dans ce cas précis, ils ont permis de résoudre une tension réelle : écrire confortablement dans WordPress tout en publiant sur un site statique performant.

Sans friction. Sans compromis.

Juste un workflow qui fonctionne.