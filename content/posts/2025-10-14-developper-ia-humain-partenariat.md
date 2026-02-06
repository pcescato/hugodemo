---
title: "Développer avec l'IA sans perdre le contrôle : Claude, Mistral et moi"
date: "2025-10-14T14:39:07+00:00"
lastmod: "2026-02-06T17:04:28+00:00"
draft: false
description: "Comment collaborer efficacement avec l'IA en développement ? J'ai utilisé Claude et Mistral en validation croisée pour transformer un plugin WordPress utilisé sur plus de 6000 sites. Résultat : 1h au lieu de 10 à 15h, 4 bugs critiques détectés, 40 erreurs corrigées en une passe. La méthode human-in-the-loop expliquée pas à pas."
tags:
  - IA
  - open source
  - performances
  - WordPress
categories:
  - Tutoriels
author: githubcopilotchallenge
---

## Et si l’IA n’était ni un remplaçant, ni un simple assistant ?

Cet article n’est pas vraiment sur WordPress. Ni même sur un plugin de sécurité. **C’est un retour d’expérience sur une question qui obsède tous les développeurs :** comment collaborer efficacement avec l’IA sans devenir un simple subalterne qui copie-colle le code généré ?

**Mon laboratoire :** La refonte d’un plugin WordPress utilisé sur plus de 6000 sites.

**Mes partenaires IA :** [Claude (Anthropic)](https://claude.ai/chat/) et [Mistral AI](https://chat.mistral.ai/chat).

**Mon rôle :** chef d’orchestre, valideur, décideur final.

<figure class="wp-block-image size-large">![](/images/1283/sqlite-to-streamlit-1024x683.webp)</figure>**Le résultat :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_ead58a-f1 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">1 heure de développement au lieu de 10 à 15 heures</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">code conforme aux standards WordPress à 100%</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">zéro régression sur les sites existants</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">détection de bugs que j’aurais pu manquer seul</span>

</div>**Le secret ?** Pas l’IA. Pas l’humain. **La méthode de collaboration.**

Ce que vous allez découvrir :

- comment utiliser deux IA en validation croisée
- pourquoi Mistral a proposé du code buggé (et comment Claude l’a détecté)
- le workflow **human-in-the-loop** qui change tout
- les 40 erreurs que l’IA a corrigées en une passe
- ce que seul l’humain peut (et doit) faire

**Spoiler :** Le plugin WordPress n’est qu’un prétexte. La vraie histoire, c’est comment orchestrer plusieurs IA pour multiplier sa productivité par 20 sans sacrifier la qualité.

Prêt ? Plongeons dans cette collaboration humain-machine. Mais avant, je voudrais rappeler que…

## Ce n’est pas ma première expérience avec l’IA

Avant ce projet, j’avais déjà exploré la collaboration IA-développeur :

**Première expérience : [Générer une application complète avec Gemini CLI](https://githubcopilotchallenge.tsw.ovh/generer-appli-complete-gemini-cli/)**

- découverte de l’approche CLI/terminal
- test de Gemini pour la génération automatisée
- premières intuitions sur le workflow

**Deuxième expérience : [Coder un plugin WordPress avec l’IA](https://githubcopilotchallenge.tsw.ovh/coder-plugin-avec-ai/)**

- développement plus ciblé sur WordPress
- apprentissage des limites spécifiques
- compréhension de la validation nécessaire

**Cette troisième expérience apporte quelque chose de nouveau : la validation croisée.**

C’est la première fois que j’utilise **deux IA différentes** pour se valider mutuellement. Et c’est ce qui a tout changé.

## Le prétexte : un plugin WordPress en production

**[Stop XML-RPC Attacks](https://wordpress.org/plugins/stop-xml-rpc-attacks/)** : Un plugin WordPress minimaliste, actif sur plus de 6000 sites, conçu pour bloquer les attaques via XML-RPC tout en préservant la compatibilité avec Jetpack et WooCommerce, développé il y a 6 ans déjà [pour contrer les attaques de type DDoS sur mon blog](https://tsw.ovh/securite-attaques-xml-rpc/).

Pendant des années, la version 1.0.1 a parfaitement rempli son rôle avec une approche minimaliste : moins de 20 lignes de code, zéro configuration, efficace dès l’activation.

```
add_filter('xmlrpc_methods', function ($methods) {
    unset($methods['system.multicall']);
    unset($methods['system.listMethods']);
    unset($methods['system.getCapabilities']);
    unset($methods['pingback.extensions.getPingbacks']);
    unset($methods['pingback.ping']);
    return $methods;
});
add_action('wp', function () {
    header_remove('X-Pingback');
}, 9999);
```

**12 lignes de code. Zéro configuration. + de 6000 installations actives.**

Mais. Parce qu’il y a toujours un mais. Le plugin était efficace mais rigide. Pas d’options, pas de souplesse, pas de logs. Une solution “one-size-fits-all” qui ne convenait probablement pas à tous.

Le défi était de faire évoluer cette solution éprouvée sans casser ce qui fonctionne, tout en apportant la flexibilité nécessaire. En utilisant l’IA pour accélérer ce développement. Mais ce n’était pas au programme…

## Acte 1 : Mistral analyse (sans qu’on lui demande)

Un jour, pour montrer qu’on peut résoudre simplement un problème, j’ai présenté mon plugin à Mistral Le Chat :

### Mon message (sans rien demander) :

```
J'ai développé un plugin WordPress pour moi - fonctionalité : bloquer les attaques via XML RPC tout en permettant l'utilisation de Jetpack par exemple - actif sur + de 6000 sites (stats WP) - actif dès l'activation, script de 12 lignes, zéro réglages<br></br><br></br>```
[Code de la v1.0.1]
```
```

**Je n’avais rien demandé. Juste présenté mon code.**

### La réponse de Mistral : une analyse complète non sollicitée

Mistral m’a répondu avec une analyse technique approfondie que je n’avais pas demandée :

```
Voici une analyse technique et stratégique de votre plugin, ainsi que des améliorations possibles pour le rendre encore plus robuste, tout en gardant sa simplicité.
```

**Points forts identifiés par Mistral :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_9eadcd-08 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la simplicité et efficacité du code</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">le ciblage précis des méthodes dangereuses</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la compatibilité Jetpack/WooCommerce préservée</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la performance optimale (pas de requêtes DB)</span>

</div>**Limites détectées par Mistral :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_5a54f1-99 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">une approche « liste noire » vs « liste blanche »</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">une en-tête X-Pingback peut être réajouté par certains thèmes</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">aucun log des tentatives d’attaque</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">une compatibilité avec les plugins de cache incertaine</span>

</div>**Et surtout : Mistral m’a proposé une version “optimisée” complète**, avec :

- une architecture orientée objet avec singleton
- une interface d’administration avec dashboard
- trois modes de sécurité configurables
- un système de logging optionnel
- des options pour les utilisateurs avancés

### Ma première réaction

“Intéressant… mais est-ce vraiment mieux ?”

Avec **6000 sites en production**, je ne pouvais pas simplement copier-coller du code proposé par une IA, aussi convaincant soit-il.

**C’est là que commence la vraie histoire : la validation croisée.** Une partie de ping-pong à trois – Mistral et Claude face à moi qui renvoyait la balle à l’un ou à l’autre en fonction du contexte.

## Acte 2 : Claude détecte ce que Mistral a manqué

Face à cette proposition intéressante mais non sollicitée, j’ai eu un réflexe de développeur prudent : **demander un second avis**. J’ai pris le code proposé par Mistral et l’ai soumis à **Claude (Anthropic)** :

**Ma question à Claude :**

```
Peux-tu me comparer ces 2 approches pour un plugin WP ?<br></br><br></br>```
[Code original v1.0.1]
``` vs ```
[Code proposé par Mistral]
```
```

Au-dela de l’interface d’administration, je voulais comprendre les différences réelles entre les deux versions, identifier les problèmes éventuels et ne pas céder aux sirènes de fausses bonnes idées qui pourraient, de plus, venir avec leur lot de bugs éventuels.

Et des bugs, il y en avait…

### Ce que Claude a immédiatement détecté

#### Bug critique #1 : conflits de filtres

```
// Ligne 13 : Option pour désactiver XML-RPC
add_filter('xmlrpc_enabled', '__return_false');
// Ligne 57 : Filtre qui s'exécute TOUJOURS (CONFLIT!)
add_filter('xmlrpc_enabled', function ($enabled) {
    if (!is_user_logged_in()) return false;
    return $enabled;
});
```

**Problème détecté par Claude :** les deux filtres se contredisent. Le second rend le premier totalement inutile. L’utilisateur qui choisit “Full Disable” se retrouverait avec un comportement différent de celui attendu.

**Mistral n’avait pas repéré ce conflit logique.**

#### Bug critique #2 : logging cassé

```
$blocked = array_diff(array_keys($methods), array_keys($methods));
if (!empty($blocked)) {
    error_log('Blocked methods: ' . implode(', ', $blocked));
}
```

**Problème détecté par Claude :** `array_diff()` d’un tableau avec lui-même retourne toujours un tableau vide ! Le logging ne fonctionnera jamais.

**Mistral avait proposé cette fonctionnalité… qui ne fonctionnait pas.**

#### Bug critique #3 : dégradation prévisible des performances

```
add_filter('rewrite_rules_array', function ($rules) {
    foreach ($rules as $rule => $rewrite) {
        if (strpos($rewrite, 'xmlrpc.php') !== false) {
            unset($rules[$rule]);
        }
    }
    return $rules;
});
```

**Problème détecté par Claude :** ce filtre est appelé à chaque requête et parcourt TOUTES les règles de réécriture WordPress. C’est coûteux, inutile, et peut dégrader les performances sur les 6000 sites.

**Mistral voulait améliorer la sécurité, mais sans se soucier des performances.**

#### Bug critique #4 : hook dans un hook

```
add_filter('xmlrpc_methods', function ($methods) {
    if (defined('SXRA_LOG_BLOCKED') && SXRA_LOG_BLOCKED) {
        add_filter('xmlrpc_call', function ($method) use ($blocked) {
            // Logging ici
        });
    }
    return $methods;
});
```

**Problème détecté par Claude :** ajouter un filtre à l’intérieur d’un autre filtre crée des hooks en cascade et des comportements imprévisibles. WordPress n’est pas conçu pour ça.

### Le constat : Mistral avait raison sur le fond, tort sur la forme

**Ce que Mistral a bien fait :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_50256d-78 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">identifier le besoin de flexibilité</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">proposer une architecture moderne</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">suggérer des fonctionnalités pertinentes</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">apporter une vision stratégique correcte</span>

</div>**Ce que Mistral a manqué :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_3a173a-28 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">des bugs d’implémentation critiques</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">des conflits logiques dans le code</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">les standards WordPress n’étaient pas respectés</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">l’impact sur les performances n’avait pas été anticipé</span>

</div>**Ce que Claude a apporté :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_c47ba5-29 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">une détection précise des bugs</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la compréhension globale des standards WordPress</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">des solutions d’implémentation correctes</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">des explications pédagogiques pour chaque problème</span>

</div>## Acte 3 : L’humain orchestre et décide

À partir de là, le développement est devenu un véritable ping-pong entre les deux IA et moi :

### Le workflow en action

<div class="wp-block-merpress-mermaidjs diagram-source-mermaid">```
flowchart TD
    A["Itération 1<br/>Architecture POO corrigée"] --> B["Tests locaux<br/>Problèmes de performance détectés"]
    B --> C["Itération 2<br/>Optimisation des hooks"]
    C --> D[Tests<br/>Conflits de constantes détectés]
    D --> E["Itération 3<br/>Gestion des constantes améliorée"]
    E --> F["Tests<br/>Besoin d'interface utilisateur"]
    F --> G["Itération 4<br/>Ajout de l'interface admin"]
    G --> H["Tests<br/>Problèmes i18n détectés"]
    H --> I["Itération 5<br/>Internationalisation ajoutée"]
```

</div>### Les itérations successives

**Itération 1 :** Architecture POO corrigée

- suppression des bugs de filtres
- singleton implémenté correctement
- structure de classe propre

**Itération 2 :** Optimisation des hooks

- suppression du filtre `rewrite_rules_array`
- hooks placés au bon moment du cycle WordPress
- performance préservée

**Itération 3 :** Gestion des options

- interface de configuration ajoutée
- trois modes de sécurité implémentés
- options stockées en base de données

**Itération 4 :** Interface d’administration

- tableau de bord avec statistiques
- design responsive et accessible
- descriptions claires pour chaque option

**Itération 5 :** Internationalisation

- Toutes les chaînes traduisibles (support multi-langues)
- Conformité aux standards i18n WordPress

### Mon rôle dans cette orchestration

**Ce que je n’ai pas fait :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_41aca2-73 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">écrire l’architecture *from scratch*</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">implémenter manuellement l’interface d’administration</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">chercher dans la documentation WordPress pendant des heures</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">tester différentes combinaisons de hooks et priorités jusquà trouver la bonne combinaison</span>

</div>**Ce que j’ai fait (et que seul l’humain peut faire) :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_534656-f7 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">décider quelle proposition de Mistral accepter ou refuser</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">arbitrer entre les suggestions de Claude et mes contraintes</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">tester à chaque étape avec WP\_DEBUG</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">comprendre l’impact sur 6000 sites en production</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">maintenir la philosophie « zéro configuration par défaut »</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">valider que la complexité ajoutée apporte vraiment de la valeur</span>

</div>**L’IA propose. L’humain dispose.**

## Le moment de vérité : 40 erreurs invisibles

Après plusieurs itérations et des tests locaux tous réussis, il est temps de passer la main à **Plugin Check (PCP)**, l’outil officiel de validation.

### Premier essai

```
❌ 40+ ERREURS ET WARNINGS<br></br>
```

Le plugin fonctionne parfaitement en local :

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_2d12b0-f4 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">WP\_DEBUG : 0 erreur</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">tests fonctionnels : OK</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">compatibilité Jetpack : OK</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">performance : excellente</span>

</div>**Mais Plugin Checker ne valide pas (au contraire).**

### Les erreurs que ni Mistral, ni Claude, ni moi n’avions vues

#### Erreur #1 : Text Domain incorrect

```
// ❌ Le code généré par les IA - fonctionnel
define('SXRA_TEXT_DOMAIN', 'stop-xmlrpc-attacks');
__('Security Mode', SXRA_TEXT_DOMAIN);
// ✅ Ce que WordPress.org exige
__('Security Mode', 'stop-xmlrpc-attacks');
```

**Raison :** le *text domain* doit être une chaîne littérale, pas une constante. Sinon les outils d’analyse de traduction de WordPress.org ne peuvent pas le détecter.

**Ni Mistral ni Claude ne connaissaient cette règle stricte.**

#### Erreur #2 : les échappements manquants

```
// ❌ Dangereux
_e('XML-RPC Security', 'stop-xmlrpc-attacks');
// ✅ Sécurisé
esc_html_e('XML-RPC Security', 'stop-xmlrpc-attacks');
```

**Raison :** toutes les sorties doivent être échappées, même celles qui semblent “sûres”. C’est une règle de sécurité stricte.

**Les IA avaient utilisé les fonctions “classiques” sans échappement.**

#### Erreur #3 : fonction dépréciée

```
// ❌ Dépréciée depuis WordPress 4.6
add_action('plugins_loaded', function() {
    load_plugin_textdomain('stop-xmlrpc-attacks', ...);
});
// ✅ Correct (ne rien faire!)
// WordPress.org charge automatiquement les traductions
```

**Raison :** depuis WordPress 4.6, les traductions sont chargées automatiquement pour les plugins hébergés sur wordpress.org. Appeler `load_plugin_textdomain()` est devenu inutile et non recommandé.

#### Erreur #4 : commentaires traducteurs manquants

```
// ❌ Refusé par Plugin Check
echo sprintf(__('Always blocked (%d):', 'stop-xmlrpc-attacks'), $count);
// ✅ Accepté
/* translators: %d: number of blocked methods */
echo sprintf(__('Always blocked (%d):', 'stop-xmlrpc-attacks'), $count);
```

**Raison :** les traducteurs ont besoin de contexte pour comprendre ce que signifie le placeholder `%d`. C’est obligatoire pour toute chaîne avec placeholders.

**Aucune IA n’avait intégré cette règle.**

#### Erreur #5 : sanitization manquante

```
// ❌ Dangereux
$ip = $_SERVER['REMOTE_ADDR'];
// ✅ Sécurisé
$ip = isset($_SERVER['REMOTE_ADDR']) 
    ? sanitize_text_field(wp_unslash($_SERVER['REMOTE_ADDR'])) 
    : 'unknown';
```

**Raison :** même `$_SERVER['REMOTE_ADDR']` doit être sanitizé selon les standards WordPress. C’est contre-intuitif (une IP fournie par le serveur), mais c’est la règle.

### Retour à Claude avec le rapport complet

**Ma demande à Claude :**

```
Voici les 40 erreurs du Plugin Check. Peux-tu corriger tout ça ?
```

**Claude** me propose une version complètement corrigée, avec :

- une explication pour chaque erreur
- la correction appropriée
- le code complet mis à jour

### Deuxième essai : essai transformé

```
✅ 0 ERREUR<br></br>✅ 0 WARNING<br></br>✅ 100% CONFORME<br></br>
```

**Impressionnant.** Claude a corrigé les 40 problèmes **en une seule passe**, en comprenant non seulement ce qui était cassé, mais aussi **pourquoi et comment** WordPress voulait que ce soit fait.

**Temps écoulé pour cette correction : moins de 5 minutes avec Claude. Sans IA : probablement 1 à 2 heures de corrections et de tests.**

## Les leçons de cette collaboration

### Comment ma méthode a évolué au fil des expérimentations

#### Expérience 1 : Automatisation avec Gemini CLI

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_d3566b-f4 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">génération rapide d’applications</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">workflow automatisé</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="fas_exclamation-triangle" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">contrôle limité sur les détails</span>

</div>#### Expérience 2 : Plugin WordPress avec Gemini CLI, Claude et ChatGPT

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_066656-97 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">plus de contrôle sur le code</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">adapté au contexte WordPress</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="fas_exclamation-triangle" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">validation manuelle extensive nécessaire via les IA</span>

</div>#### Expérience 3 (celle-ci) : Validation croisée avec deux IA

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_bcc320-03 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">détection automatique des bugs</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">qualité supérieure</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">confiance renforcée</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">**la méthode qui change tout**</span>

</div>### Leçon 1 : Les IA ont des forces complémentaires

#### Mistral : le visionnaire stratégique

**Ses points forts :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_77aa04-8b kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">une vision macro excellente</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la détection des faiblesses conceptuelles</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">propose des architectures ambitieuses</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">identification des besoins utilisateurs</span>

</div>**Ses points faibles :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_73369f-3a kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">peut proposer du code avec des bugs</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">ne connaît pas toujours les standards spécifiques</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">détails d’implémentation parfois incorrects</span>

</div>**Mistral est parfait pour du brainstorming, pour l’architecture initiale, pour l’identification des besoins.**

#### Claude : le technicien rigoureux

**Ses points forts :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_46aab9-0e kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">détection de bugs très précise</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">connaissance approfondie des standards</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">explications pédagogiques détaillées</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">code production-ready</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">corrections en une passe</span>

</div>**Ses points faibles :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_854e06-02 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">parfois plus conservateur</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">peut manquer de vision “out of the box”</span>

</div>**Claude est parfait pour le debugging, la conformité, l’optimisation, la production**

### Leçon 2 : La validation croisée est indispensable

**Ce qui aurait pu mal tourner si j’avais fait confiance aveuglément en l’IA :**

#### Scénario A : Copier-coller du code Mistral

**Conséquences potentielles :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_661c77-31 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">bugs dus aux conflits de filtres en production</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">logging complètement non-fonctionnel</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">une expérience utilisateur dégradée</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">**impact négatif sur des milliers de sites**</span>

</div>#### Scénario B : Ne consulter qu’une seule IA

**Conséquences potentielles :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_1a0ffc-b8 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">risque de manquer des optimisations importantes</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">ou au contraire, sur-optimiser sans raison valable</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">biais de l’IA non détecté</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">bugs subtils non repérés</span>

</div>#### Scénario C : Faire confiance à 100% aux IA

**Ce qui s’est réellement passé :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_912c2d-3f kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">40 erreurs que les IA n’avaient pas anticipées</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">standards WordPress que les IA ne connaissaient pas tous</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">code déprécié (`load_plugin_textdomain` deprecated par exemple )</span>

</div>### Leçon 3 : L’humain reste le chef d’orchestre

**Ce que les IA ont fait pour moi :**

✅ **Mistral :**

- m’a proposé une architecture complète
- a identifié les limitations de la v1.0.1
- a suggéré des fonctionnalités pertinentes
- m’a présenté une vision stratégique du besoin utilisateur

✅ **Claude :**

- a détecté les bugs dans le code Mistral
- a corrigé les non-conformités aux standards
- m’a expliqué le “pourquoi” de chaque modification
- a généré du code production-ready

**Ce que seul l’humain peut (et doit) faire :**

- **Décider** : Accepter ou refuser chaque proposition
- **Arbitrer** : Entre complexité et simplicité
- **Comprendre** : L’impact sur plus de 6000 utilisateurs
- **Tester** : En conditions réelles, pas juste en théorie
- **Maintenir** : La philosophie du projet (zéro config par défaut)
- **Assumer** : La responsabilité du code déployé

**L’IA accélère. L’humain dirige.**

### Leçon 4 : Les tests automatisés ne suffisent pas

Un rappel important : le code parfait (en théorie) peut être refusé s’il ne suit pas les règles édictées par la plateforme.

**Avant Plugin Check :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_2efd1e-ef kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">WP\_DEBUG : 0 erreur</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">tests fonctionnels : Tous OK</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">compatibilité : Jetpack fonctionne</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">performance : Excellente</span>

</div>**Après Plugin Check :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_354c96-40 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">plus de 40 erreurs de conformité</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">standards non respectés</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">règles de sécurité violées</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">bonnes pratiques ignorées</span>

</div>**Pourquoi ?** WordPress a des standards très stricts que les tests locaux ne peuvent pas détecter. Même les IA les plus avancées ne connaissent pas toutes ces règles spécifiques.

**La leçon :** Toujours valider avec les outils officiels, même quand tout fonctionne parfaitement.

## Le workflow human-in-the-loop idéal

Voici la méthode qui a émergé de cette expérience :

<div class="wp-block-merpress-mermaidjs diagram-source-mermaid">```
flowchart TD
    A[Mistral propose des idées] --> B[Claude valide et corrige]
    B --> C["Je teste en local<br/>(WP_DEBUG activé)"]
    C --> D[Plugin Check valide la conformité]
    D --> E[Tests sur sites réels]
    E --> F[Mise en production]
```

</div>### Étape 1 : Brainstorming avec une IA “visionnaire”

**Action :** présenter le problème à Mistral (ou similaire)

**But :** obtenir des idées, une vision macro, des propositions architecturales

**Attitude :** écouter, noter, mais ne pas implémenter directement

### Étape 2 : Validation croisée avec une IA “rigoureuse”

**Action :** soumettre les propositions à Claude (ou similaire)

**But :** détecter les bugs, valider l’implémentation, corriger les erreurs

**Attitude :** cles deux avis, identifier les divergences

### Étape 3 : Décision humaine éclairée

**Action :** choisir ce qui a du sens pour le projet

**But :** préserver la philosophie, anticiper l’impact utilisateur

**Attitude :** l’IA propose, l’humain dispose

### Étape 4 : Implémentation itérative

**Action :** développer par petites itérations testables

**But :** valider à chaque étape, corriger rapidement

**Attitude :** tests locaux systématiques (WP\_DEBUG activé)

### Étape 5 : Validation avec outils officiels

**Action :** Plugin Check, tests de conformité

**But :** détecter ce que les IA et les tests locaux ont manqué

**Attitude :** accepter que même l’IA puisse ignorer certaines règles

### Étape 6 : Correction finale avec IA

**Action :** soumettre les erreurs détectées à Claude

**But :** corriger en une passe si possible tous les problèmes de conformité

**Attitude :** l’IA excelle dans ce type de tâche répétitive

### Étape 7 : Tests en conditions réelles

**Action :** déployer sur des sites de test variés

**But :** valider la compatibilité réelle

**Attitude :** rien ne remplace le monde réel

## Les chiffres de cette collaboration

<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Métrique</th><th>Valeur</th></tr></thead><tbody><tr><td>**Version initiale**</td><td>v1.0.1 (12 lignes de code)</td></tr><tr><td>**Version finale**</td><td>v2.0.0 (375 lignes de code)</td></tr><tr><td>**Message initial à Mistral**</td><td>1 (présentation, pas de demande)</td></tr><tr><td>**Analyse Mistral**</td><td>Complète et détaillée (non sollicitée)</td></tr><tr><td>**Échanges avec Claude**</td><td>~20 allers-retours</td></tr><tr><td>**Bugs détectés par Claude**</td><td>4 bugs critiques dans le code Mistral</td></tr><tr><td>**Versions intermédiaires**</td><td>5 itérations</td></tr><tr><td>**Erreurs Plugin Check**</td><td>de 40 à 0 en 1 passe</td></tr><tr><td>**Tests locaux**</td><td>8 sites WordPress différents</td></tr><tr><td>**Temps total de dev**</td><td>environ 1 heure</td></tr><tr><td>**Temps estimé sans IA**</td><td>entre 10 et 15 heures</td></tr><tr><td>**Gain de temps**</td><td>~90%</td></tr><tr><td>**Installations actives**</td><td>6000+</td></tr></tbody></table>

</figure>## Le résultat : une solution flexible sans sacrifier la robustesse

### Ce qui a changé entre v1.0.1 et v2.0.0

**Architecture :**

- 12 lignes → 375 lignes (mais organisées en classe POO)
- approche procédurale → architecture orientée objet
- code inline → structure maintenable et extensible

**Fonctionnalités :**

- 1 mode → 3 modes de sécurité configurables
- 0 option → interface d’administration complète
- pas de logs → logging optionnel avec IP tracking
- 5 méthodes bloquées → 5 à 8 selon configuration

**Expérience utilisateur :**

- zéro configuration → toujours zéro configuration par défaut
- aucune souplesse → 3 niveaux de protection + options avancées
- pas de visibilité → tableau de bord avec statistiques en temps réel

### Ce qui n’a pas changé

**La robustesse :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_e78e4a-47 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">toujours actif dès l’activation</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">zéro configuration obligatoire</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">compatibilité Jetpack / WooCommerce préservée</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">performance optimale maintenue</span>

</div>**La philosophie :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_f9d916-6f kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">“Ça marche out-of-the-box” reste la priorité</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">les options sont pour ceux qui veulent plus</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la simplicité n’est pas sacrifiée à la flexibilité</span>

</div>**L’expérience par défaut :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_c86092-92 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">un utilisateur qui active le plugin ou qui le met à jour sans rien toucher a exactement le même comportement qu’avec la v1.0.1</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la complexité est optionnelle, pas imposée</span>

</div>### Les trois modes de sécurité

#### Mode 1 : Full disable (sécurité maximale)

- désactive complètement XML-RPC
- pour les sites qui n’utilisent ni Jetpack, ni apps mobiles
- protection absolue contre toute attaque XML-RPC

#### Mode 2 : Guest disable (équilibré)

- XML-RPC désactivé pour les visiteurs non connectés
- autorisé pour les administrateurs authentifiés
- bloque les méthodes dangereuses même pour les admins

#### Mode 3 : Selective blocking (meilleure compatibilité) Par défaut

- bloque uniquement les 5 méthodes les plus dangereuses
- compatibilité totale avec Jetpack et WooCommerce
- le comportement exact de la v1.0.1, mais configurable

### L’interface d’administration

Accessible via **Réglages → XML-RPC Security** :

**Tableau de bord visuel avec :**

- le nombre de méthodes bloquées en temps réel
- le mode de sécurité actif
- l’état du logging

**Options claires :**

- une description détaillée pour chaque mode
- des codes couleur intuitifs (vert = sécurité max, jaune = équilibré)
- un avertissement sur l’impact de chaque option

**Options avancées :**

- blocage de l’énumération utilisateur (casse les apps mobiles)
- logging des attaques (uniquement si WP\_DEBUG actif)

## Conclusion : l’IA multiplie les forces, l’humain dirige

Ce projet m’a montré que le futur du développement logiciel n’est ni :

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_bb77d6-a8 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">l’IA remplace complètement le développeur</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">le développeur fait tout manuellement sans assistance</span>

</div>Mais plutôt :

✅ **Le développeur orchestre plusieurs IA spécialisées**

- chaque IA apporte ses forces complémentaires
- la validation croisée détecte les faiblesses de chacune
- l’humain prend les décisions finales éclairées

✅ **Validation systématique et rigoureuse**

- des tests locaux avec WP\_DEBUG
- la validation avec des outils officiels (Plugin Check)
- des tests en conditions réelles
- accepter que même l’IA puisse se tromper

✅ **Responsabilité humaine inéliminable**

- comprendre l’impact sur les utilisateurs
- choisir entre propositions contradictoires
- maintenir la philosophie du projet
- assumer le code en production

### Le gain réel : 90% de temps, 100% de qualité

**Sans IA :** de 10 à 15 heures

- recherche d’architecture
- implémentation manuelle de l’interface
- debugging des problèmes de conformité
- lecture de documentation WordPress
- développement, tests et debug

**Avec IA :** environ 1 heure

- architecture proposée instantanément par Mistral
- bugs détectés en temps réel par Claude
- corrections des standards automatiques
- explications pédagogiques incluses
- conformité validée en 2 passes

La qualité finale est identique, voire supérieure, car :

- la validation croisée est plus rigoureuse que celle d’un développeur seul
- l’IA a détecté des bugs que j’aurais peut-être manqués
- la conformité aux standards est garantie
- l’architecture est aussi propre, voire plus propre que ce que j’aurais fait sans IA

### Ce que j’ai appris sur moi-même

Cette expérience m’a aussi appris quelque chose sur mon propre processus de développement :

**Avant l’IA, j’aurais probablement :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_694c05-6c kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">gardé le code minimaliste par flemme d’architecture</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">évité l’interface admin “parce que c’est long”</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">ignoré certains besoins utilisateurs par manque de temps</span>

</div>**Avec l’IA, j’ai pu :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_0359eb-43 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">oser une architecture ambitieuse</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">créer une vraie interface utilisateur</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">répondre aux besoins de différents types d’utilisateurs</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">maintenir un code de qualité professionnelle sans effort surhumain</span>

</div>**L’IA ne m’a pas remplacé. Elle m’a rendu meilleur.**

## Les erreurs à éviter absolument

Fort de cette expérience, voici les pièges que j’ai identifiés :

### Erreur 1 : faire confiance aveuglément à une seule IA

**Pourquoi c’est dangereux :**

- chaque IA a ses biais et ses angles morts
- le code peut “sembler” correct sans l’être
- les bugs subtils passent inaperçus

**La solution :** toujours valider avec une seconde IA ou un second regard

### Erreur 2 : copier-coller sans comprendre

**Pourquoi c’est dangereux :**

- vous ne pourrez pas maintenir le code
- les bugs seront impossibles ou difficiles à corriger
- vous ne saurez pas expliquer vos choix techniques

**La solution :** demander des explications, comprendre avant d’implémenter

### Erreur 3 : ignorer les tests locaux

**Pourquoi c’est dangereux :**

- les IA peuvent proposer du code qui ne fonctionne pas
- les effets de bord ne sont pas anticipés
- la compatibilité réelle n’est pas testée

**La solution :** Tester chaque itération avec `WP_DEBUG` activé

### Erreur 4 : négliger les outils de validation officiels

**Pourquoi c’est dangereux :**

- les IA ne connaissent pas toutes les règles spécifiques
- les standards évoluent plus vite que les modèles d’IA
- Plugin Check détecte ce que les tests locaux manquent

**La solution :** Toujours valider avec Plugin Check (PCP) avant de déployer

### Erreur 5 : perdre de vue la philosophie du projet

**Pourquoi c’est dangereux :**

- l’IA peut proposer des features inutiles
- la complexité s’accumule sans valeur ajoutée
- on oublie ce qui fait la force du projet initial

**La solution :** Rester le gardien de la vision, arbitrer chaque ajout

**Un exemple ?** Claude m’a proposé d’utiliser l’opérateur `??=` de PHP 8.0 pour simplifier le singleton :

```
// PHP 7.4
public static function init() {
    if (self::$instance === null) {
        self::$instance = new self();
    }
    return self::$instance;
}
// PHP 8.0+
public static function init() {
    return self::$instance ??= new self();
}
```

**La proposition de Claude était techniquement correcte :** Code plus concis, plus lisible, plus “moderne”.

**J’ai refusé. Voici pourquoi :**

**Les statistiques WordPress.org au moment du développement :**

- PHP 7.4 : **24,6%** des installations WordPress
- PHP 8.0+ : ~70% des installations
- PHP &lt;7.4 : ~5% des installations

En exigeant PHP 8.0+, je privais **près d’un quart des utilisateurs WordPress** (24,6%) d’un outil qui sécurise leur site.

Certes, passer à PHP 8.0 voire 8.1 est indispensable pour des raisons de sécurité. Le CMS préféré du web est forcément la cible privilégiée d’attaques de tout genre… autant ne prendre aucun risque. Mais ce n’est pas à mon plugin de forcer cette migration – surtout pour un gain esthétique de quelques lignes de code.

**Mon choix :** Privilégier la sécurité immédiate de plus de 6000 sites (dont potentiellement 1500 en PHP 7.4) à un code plus élégant.

**Claude ne pouvait pas faire ce choix.** Il ne connaît pas :

- la distribution réelle des versions PHP
- l’urgence du besoin de sécurité XML-RPC
- le contexte socio-technique (hébergeurs mutualisés, sites legacy, budget limité)
- ma philosophie “accessibilité maximale”

Pour un plugin de sécurité, **exclure 1 site sur 4 n’est pas acceptable**.

C’est exactement ça, le rôle de l’humain : **faire des choix contextuels basés sur des données réelles** que l’IA ne peut pas connaître.

L’IA propose le code **le plus moderne**. L’humain choisit le code **le plus utile**.

## Et si je devais recommencer ?

Avec le recul, voici ce que je changerais (ou pas) :

### Ce que je referais à l’identique

✅ **Utiliser deux IA en validation croisée**

- Mistral pour la vision stratégique
- Claude pour la rigueur technique
- et toujours l’humain pour orchestrer

✅ **Tester à chaque itération**

- `WP_DEBUG` activé en permanence
- tests sur plusieurs configurations
- validation continue

✅ **Ne jamais copier-coller aveuglément**

- youjours demander des explications
- comprendre avant d’implémenter
- rester le décideur final

## Les questions que vous vous posez probablement

### “Est-ce que ça marche pour tous les types de projets ?”

**Ma réponse :** Oui, mais avec des nuances.

**Ce flux de travail fonctionne bien pour :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_642d32-02 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la refonte de code existant</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">l’ajout de fonctionnalités à un projet mature</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">le debugging de code complexela conformité aux standards</span>

</div>**Fonctionne moins bien pour :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_4c2988-52 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">l’innovation algorithmique pure (l’IA reproduit ce qui existe)</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">les problèmes nécessitant une compréhension métier profonde</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">l’architecture de systèmes distribués complexes</span>

</div>### “Quelle IA choisir ?”

**Ma recommandation :**

**Pour la vision stratégique :**

- Mistral AI (excellent pour l’analyse et les propositions)
- ChatGPT-5 (très créatif sur les architectures)

**Pour la rigueur technique :**

- Claude (Anthropic) – mon préféré pour le debugging
- ChatGPT-5 (très bon aussi, mais parfois moins précis)

**L’important :** En utiliser **au moins deux** en validation croisée.

### “Combien ça coûte ?”

**Pour ce projet :**

- Mistral AI : Gratuit (version Le Chat)
- Claude : Gratuit (version Claude.ai)
- **Total : 0€**

**Si vous utilisez les API payantes :**

- Mistral API : ~0,50€ pour un projet comme celui-ci
- Claude API : ~1-2€ pour un projet comme celui-ci
- **Total : ~2€ maximum**

**Comparé à 10/15 heures de développement :** ROI absolument démentiel.

### “Et la sécurité du code généré par l’IA ?”

**Mon expérience :**

**Les IA sont bonnes pour :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_70ff41-44 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">l’échappement de sortie (si on le leur demande)</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la sanitization des entrées (généralement)</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">la validation des données (la plupart du temps correcte)</span>

</div>**Mais peuvent manquer :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_ec95d7-60 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">les règles spécifiques au framework (d’où les 40 erreurs Plugin Check)</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">les failles subtiles liées au contexte</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">l’impact sur la performance</span>

</div>**La solution : toujours faire auditer par les outils officiels.**

### “L’IA va-t-elle remplacer les développeurs ?”

**Ma conviction après cette expérience : Non.**

**Ce qui va changer :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_8a2406-83 kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">les développeurs vont devenir des “orchestrateurs d’IA”</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">le code “boilerplate” sera généré instantanément</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">le debugging sera énormément accéléré</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="USE_PARENT_DEFAULT_ICON" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">les standards et la conformité seront plus faciles à respecter</span>

</div>**Ce qui ne changera pas :**

- le besoin de comprendre le contexte métier
- la nécessité d’arbitrer entre solutions contradictoires
- la responsabilité du code en production
- la vision architecturale à long terme
- la compréhension des besoins utilisateurs réels

**L’IA rend les bons développeurs meilleurs. Elle ne transforme pas les non-développeurs en développeurs.**

## Ressources et liens

### Le plugin

- **Dans le dépôt WordPress :** [Stop XML-RPC Attacks](https://wordpress.org/plugins/stop-xml-rpc-attacks/)
- **Repository SVN :** <https://plugins.svn.wordpress.org/stop-xml-rpc-attacks/tags/> (versions 1.0.1 et 2.0.0)
- **Statistiques :** 6000+ installations actives

### Les IA utilisées

- **Mistral AI :** [Le Chat](https://chat.mistral.ai/)
- **Claude (Anthropic) :** [Claude.ai](https://claude.ai/)

### Outils de validation

- **Plugin Check (PCP) :** [Plugin officiel](https://fr.wordpress.org/plugins/plugin-check/)
- **WP\_DEBUG :** Documentation sur [developer.wordpress.org](https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/)

### Pour aller plus loin

- **Standards WordPress :** [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/)

## Conclusion finale : une collaboration qui redéfinit le développement

Ce projet a affiné ma perception de l’IA en développement. Ce n’est ni un assistant magique, ni un simple outil. **C’est un partenaire de collaboration avec ses forces et ses faiblesses.**

**Les chiffres :**

- 1 heure au lieu de 10 à 15 heures
- 0 régression sur plus de 6000 sites
- 4 bugs critiques détectés par validation croisée
- 40 erreurs de conformité corrigées en une passe
- 375 lignes de code production-ready

**Mais au-delà des chiffres :**

- une architecture que je n’aurais pas osé tenter seul
- une qualité de code supérieure à ce que j’aurais fait manuellement
- des standards respectés que j’aurais probablement ignorés
- la flexibilité sans perdre la simplicité d’utilisation

**Le secret ?** Pas l’IA. Pas l’humain. **La méthode de collaboration.**

**Human-in-the-loop** n’est pas un buzzword. **C’est une nécessité**. L’IA **propose, détecte, corrige**. L’humain **décide, arbitre, assume**. Ensemble, ils créent quelque chose qu’aucun des deux n’aurait pu faire seul.

**Mon analyse finale :**

<div class="wp-block-kadence-iconlist kt-svg-icon-list-items kt-svg-icon-list-items1283_b2a845-6b kt-svg-icon-list-columns-1 alignnone">- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="fas_exclamation-triangle" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">Si vous développez en 2025 sans utiliser l’IA, vous perdez un temps précieux.</span>
- <span class="kadence-dynamic-icon" data-class="kt-svg-icon-list-single" data-name="fas_lightbulb" data-stroke="USE_PARENT_DEFAULT_WIDTH"></span><span class="kt-svg-icon-list-text">Si vous développez en 2025 en faisant confiance aveuglément à l’IA, **vous prenez des risques énormes**.</span>

</div>**La bonne approche :**

- utilisez plusieurs IA en validation croisée
- testez systématiquement à chaque étape
- validez avec les outils officiels
- comprenez avant d’implémenter
- restez le chef d’orchestre

**L’IA est un accélérateur. Pas un pilote automatique.**

## Votre expérience avec l’IA en développement ?

Avez-vous déjà utilisé plusieurs IA en validation croisée pour un projet ? Quelles ont été vos découvertes et vos pièges ? Êtes-vous d’accord avec l’approche *“human-in-the-loop”* ?

**Partagez votre expérience en commentaire !**