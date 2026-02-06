---
title: "AgentKit : du flux n8n à l'orchestration d'agents déclaratifs"
date: "2026-02-06T19:40:39+00:00"
lastmod: "2026-02-06T19:40:39+00:00"
draft: false
description: "Introduction : une mutation silencieuse Depuis quelques années, la construction de pipelines d'automatisation s'appuie sur des outils visuels comme n8n, Make ou Zapier. Ces plateformes ont simplifié l'orchestration de flux..."
tags:
  - automatisation
  - IA
  - performances
categories:
  - Tutoriels
author: githubcopilotchallenge
image: /images/1220/featured.webp
image_formats:
  - /images/1220/featured.webp
  - /images/1220/featured.avif
---

### Introduction : une mutation silencieuse

Depuis quelques années, la construction de pipelines d’automatisation s’appuie sur des outils visuels comme **n8n**, **Make** ou **Zapier**. Ces plateformes ont simplifié l’orchestration de flux métier : un événement déclenche une série d’actions, qui font circuler la donnée d’un service à un autre. Cette logique a évolué avec l’arrivée des modèles de langage (LLM) : les flux ne se contentent plus d’exécuter, ils interprètent.

![](/images/1220/cover.webp)

Mais cette puissance nouvelle s’est accompagnée d’une difficulté croissante : **gérer la complexité de l’intelligence** elle-même. Dans n8n, par exemple, tu peux chaîner des appels à OpenAI, vérifier la structure JSON, relancer des requêtes si la sortie est mal formée, etc. Ça fonctionne, mais à mesure que le flux s’enrichit, il devient lourd, peu lisible et difficile à maintenir.

C’est dans ce contexte qu’OpenAI a annoncé, lors du **DevDay du 6 octobre 2025**, [le lancement d’**AgentKit**](https://openai.com/index/introducing-agentkit/), un environnement destiné à simplifier la conception et l’orchestration d’agents autonomes. Le projet est actuellement en **bêta ouverte** pour certains développeurs. Il n’est pas open source mais repose sur des standards ouverts (YAML, JSON, API REST). Certaines briques, comme la **Connector Registry**, restent encore en déploiement progressif.

AgentKit ne se veut pas un nouvel outil d’automatisation, mais plutôt une **plateforme de conception d’agents intelligents**. En d’autres termes : une tentative d’industrialiser ce qu’on faisait jusque-là à la main.

### De n8n à l’agent autonome : repenser la chaîne de traitement

Prenons un cas concret. Pendant quelques mois, j’utilise **n8n** [pour automatiser ma veille hebdomadaire](https://githubcopilotchallenge.tsw.ovh/pocket-migration-wallabag/) sur les technologies open source :

- Récupération d’articles via **Wallabag**.
- Nettoyage HTML → Markdown.
- Analyse IA (résumé, scoring, mots-clés, catégorisation).
- Insertion en base PostgreSQL.
- Génération HTML pour la newsletter.
- Diffusion automatique.

Un flux efficace, mais où chaque bloc IA demande une vigilance particulière : un **JSON mal formé**, une **valeur absente**, ou une **catégorie incohérente** peuvent bloquer la chaîne. Il faut multiplier les nœuds de vérification et les boucles de secours. En somme : une orchestration fonctionnelle mais fragile.

C’est précisément ce maillon qu’AgentKit cherche à renforcer : non pas la collecte ou la diffusion, mais **la couche cognitive** du flux. Là où n8n orchestre des appels, AgentKit **décrit des intentions**.

### AgentKit : une logique déclarative

AgentKit repose sur un principe simple : décrire un agent dans un format déclaratif. Tu ne programmes plus son comportement pas à pas, tu le **définis** dans un fichier YAML. Celui-ci contient :

- Les **entrées** (inputs).
- Le **modèle** utilisé (OpenAI, OpenRouter, Mistral, Gemini…).
- Le **prompt** principal.
- Les **contraintes de sortie** (schéma JSON attendu).
- Les **outils** ou fonctions accessibles à l’agent.

Cette structure rend l’agent **portable** et lisible : le même fichier YAML peut être exécuté localement ou déployé demain dans l’environnement natif AgentKit. Et si AgentKit venait à évoluer radicalement ou à disparaître ? Pas de panique : le format YAML et ton runner local restent fonctionnels. C’est tout l’enjeu d’un standard ouvert.

### Exemple : la portion de flux n8n remplacée

Dans le flux n8n d’origine, le cœur du traitement IA ressemblait à ceci :

```
[HTTP Request → OpenAI API]
→ Résumé + mots-clés + score JSON
→ Vérification JSON
→ Enregistrement SQL
```

Un schéma simple mais vulnérable à la moindre erreur de format. Avec AgentKit, cette partie devient un agent à part entière, décrit ainsi :

```
agent:
  name: resume_keywords_agent
  description: >
    Analyse un contenu web pour générer un résumé orienté business, 
    avec notation et classification normalisée.
  inputs:
    - markdown_content: string
  model:
    provider: variable
    model_name: variable
    temperature: 0.1
    response_format: json
  prompt: |
    Tu es un analyste technologique spécialisé dans les solutions open-source et automatisables.
    Analyse le contenu suivant et produis une évaluation orientée business pratique, en français.
    Critères :
      - Applicabilité PME/freelances (0 à 3)
      - Potentiel d'automatisation (0 à 2)
      - Aspect économique concret (0 à 2)
      - Open-source / self-hosted (0 à 2)
      - Innovation vs établi (0 à 1)
    Format attendu :
    {
      "applicabilite_pme": 0,
      "potentiel_automatisation": 0,
      "aspect_economique": 0,
      "open_source": 0,
      "innovation": 0,
      "score_global": 0,
      "summary": "...",
      "keywords": ["..."],
      "category": "..."
    }
    Contenu : {{markdown_content}}
```

Ce fichier, versionné et auditable, remplace plusieurs nœuds dans n8n.

### Le runner universel : un interpréteur maison

AgentKit n’étant pas encore totalement disponible, j’ai développé un **runner Python** pour exécuter ces fichiers YAML localement. Ce script :

- lit le YAML,
- remplace les variables dans le prompt,
- envoie la requête au modèle LLM (OpenRouter, Mistral, DeepInfra…),
- valide la sortie via **Pydantic**,
- relance automatiquement en cas de JSON invalide.

Voici un extrait du modèle de validation :

```
from pydantic import BaseModel, conint, Field
from typing import List
class ResumeKeywordsOutput(BaseModel):
    applicabilite_pme: conint(ge=0, le=3)
    potentiel_automatisation: conint(ge=0, le=2)
    aspect_economique: conint(ge=0, le=2)
    open_source: conint(ge=0, le=2)
    innovation: conint(ge=0, le=1)
    score_global: conint(ge=0, le=10)
    summary: str
    keywords: List[str] = Field(..., min_items=3)
    category: str
```

Et la boucle principale :

```
def run_agent(agent_config, variables, max_retries=2):
    template = Template(agent_config["agent"]["prompt"])
    rendered = template.render(**variables)
    for attempt in range(1, max_retries + 1):
        try:
            raw = call_llm(rendered)
            parsed = json.loads(raw)
            validated = ResumeKeywordsOutput(**parsed)
            return {"success": True, "output": validated.dict(), "attempt": attempt}
        except json.JSONDecodeError:
            print(f"JSON invalide (tentative {attempt}) → relance")
            rendered = f"Corrige ce JSON invalide et renvoie uniquement le JSON corrigé :\n{raw}"
        except Exception as e:
            if attempt == max_retries:
                return {"success": False, "error": str(e)}
```

**📦 Note sur la composition**

En production, avec des dizaines d’agents et des prompts de plusieurs milliers de tokens, un YAML monolithique devient rapidement ingérable. L’approche recommandée consiste à **découper en composants réutilisables** :

- **Prompts modulaires** : extraire les instructions communes (`prompts/base/analyst_role.md`, `prompts/base/output_format.md`)
- **Schémas partagés** : centraliser les formats JSON de sortie (`schemas/scoring.json`)
- **Composition par référence** : utiliser des includes dans le YAML ou des variables d’environnement

Exemple de structure composée :

```
agent:
  name: resume_keywords_agent
  prompt_parts:
    - role: "{{include('prompts/base/analyst_role.md')}}"
    - criteria: "{{include('prompts/scoring/criteria_v2.md')}}"
    - input: "Contenu : {{markdown_content}}"
  output_schema: "{{load('schemas/scoring.json')}}"
```

Le runner peut alors résoudre ces références avant exécution, ce qui permet de versionner chaque brique indépendamment et de propager rapidement les mises à jour communes à tous tes agents.

Une fois ce runner opérationnel, **l’exposer via FastAPI devient trivial**.

### API FastAPI : rendre l’agent accessible

Avec FastAPI, l’agent devient un service REST local. n8n, un front Vue3 ou tout autre système peut le consommer sans se soucier de la logique interne :

```
@app.post("/analyze")
def analyze(req: AgentRequest):
    agent_config = load_agent(f"agents/{req.agent_name}.yaml")
    result = run_agent(agent_config, {"markdown_content": req.markdown_content})
    return result
```

Un simple `POST /analyze` retourne une réponse propre, validée et exploitable :

```
{
  "success": true,
  "output": {
    "applicabilite_pme": 3,
    "potentiel_automatisation": 2,
    "aspect_economique": 2,
    "open_source": 2,
    "innovation": 1,
    "score_global": 9,
    "summary": "Nouvelle évolution de Milvus, simplifiant la gestion vectorielle locale...",
    "keywords": ["Milvus", "vector store", "LLM", "self-hosted"],
    "category": "IA & stockage vectoriel"
  },
  "attempt": 1
}
```

### Ce que ça change

#### Lisibilité et portabilité

Le YAML devient un contrat clair entre développeur et agent : versionnable, diffable, documenté.

#### Robustesse et validation

Pydantic garantit que les données retournées sont cohérentes avant toute utilisation.

#### Découplage fort

L’agent peut être exécuté depuis n8n, FastAPI ou un orchestrateur tiers, sans dépendance technique.

#### Traçabilité et audit

Chaque exécution peut être journalisée : modèle utilisé, tentative, durée, sortie. Ça ouvre la voie à une **observabilité IA** digne de ce nom.

#### Une approche durable

AgentKit ne remplace pas les orchestrateurs : il les complète. On passe d’un enchaînement de fonctions à un **design d’agents autonomes**.

### Limites et perspectives

AgentKit reste en phase de généralisation progressive. Les connecteurs API, la sécurité (rate limiting, validation des entrées, sandboxing) et le monitoring sont encore en cours d’intégration. L’exécution d’agents YAML nécessite également une attention particulière aux vecteurs d’injection : validation stricte des variables, sandboxing des appels externes, et signature des fichiers agents en environnement sensible.

Les coûts liés aux retries peuvent aussi peser dans des contextes à grand volume : **un agent qui relance 2 fois sur 30% des requêtes augmente la facture API de 60%**. Un prompt engineering soigné et des stratégies de backoff peuvent atténuer cet impact.

Mais l’approche est déjà **réplicable localement**. En d’autres termes : tu peux tester le concept dès maintenant, sans attendre l’infrastructure complète d’OpenAI. L’idée n’est pas de remplacer les frameworks existants (LangChain, CrewAI, etc.), mais de proposer un **format pivot**, plus universel, où les agents deviennent des entités déclarées, réutilisables et évaluables.

### Conclusion

L’arrivée d’AgentKit marque une étape vers une IA plus structurée et plus prévisible. L’enjeu n’est plus d’appeler un modèle, mais de **concevoir des agents robustes**, traçables et interopérables.

L’orchestration ne sera plus un assemblage de blocs, mais une **composition d’intentions**.

> En somme, ce que YAML a fait pour l’infrastructure, AgentKit s’apprête à le faire pour les intelligences.

C’est encore jeune, mais la direction est claire : **moins de flux, plus d’agents.**