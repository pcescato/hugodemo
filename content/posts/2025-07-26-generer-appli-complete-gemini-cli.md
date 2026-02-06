---
title: "Générer une application complète avec Gemini CLI : fantasme ou réalité ?"
date: "2025-07-26T17:21:22+00:00"
lastmod: "2025-07-26T17:21:22+00:00"
draft: false
description: "Peut-on vraiment générer une application web complète avec FastAPI, React et base de données en moins d'une heure grâce à l'IA ? Gemini CLI le prétend. Test en conditions réelles avec un cahier des charges précis et... quelques surprises à la clé. Retour d'expérience sans filtre."
tags:
  - Gemini CLI
  - performances
  - PoC
  - prototype
  - Python FastAPI
categories:
  - PoC
author: githubcopilotchallenge
---

J’ai posé une question toute simple à Gemini 2.5 Flash : *À quoi sert Gemini CLI ?* Voici sa réponse, très… promotionnelle :

```
Gemini CLI (Command Line Interface) est un utilitaire en ligne de commande open-source développé par Google qui intègre la puissance des modèles d'IA Gemini directement dans votre terminal. Il est conçu pour simplifier et automatiser les tâches courantes des développeurs et améliorer leur productivité.<br></br><br></br>Voici les principales fonctions et utilités de Gemini CLI :<br></br><br></br>Compréhension et analyse de code :<br></br>Il peut explorer et comprendre votre base de code, même les projets très volumineux (jusqu'à 1 million de tokens de contexte).<br></br>Il peut répondre à des questions sur le code en langage naturel (par exemple, "Explique comment fonctionne la fonction de connexion").<br></br>Il aide à visualiser le code en générant des diagrammes de flux ou d'autres représentations.<br></br><br></br>Assistance au développement et débogage :<br></br>Il peut vous aider à corriger des bogues et des problèmes directement depuis votre terminal.<br></br>Il est capable de refactoriser le code pour l'améliorer.<br></br>Il génère des tests unitaires pour votre code.<br></br>Il peut insérer du code ou modifier des fichiers selon vos instructions.<br></br>Il aide à trouver les variables inutilisées ou les types incorrects.<br></br><br></br>Automatisation et gestion de projet :<br></br>Il peut automatiser la génération de projets.<br></br>Il facilite la gestion des versions et les déploiements.<br></br>Il peut créer des rapports Markdown des modifications apportées.<br></br>Il peut interagir avec des systèmes d'entreprise comme SAP et Celonis.<br></br>Il peut être invoqué de manière non interactive dans des scripts pour automatiser des tâches.<br></br><br></br>Interaction en langage naturel :<br></br>Vous pouvez interagir avec Gemini CLI en utilisant des invites en langage naturel, sans avoir à apprendre de commandes spécifiques.<br></br>Il maintient une "mémoire" de votre session, ce qui permet de poser des questions de suivi sans répéter le contexte.<br></br><br></br>Fonctionnalités avancées et extensibilité :<br></br>Il prend en charge le "Model Context Protocol (MCP)" et d'autres extensions.<br></br><br></br>Vous pouvez personnaliser les invites et les instructions pour adapter Gemini à vos besoins spécifiques.<br></br>Il offre des outils intégrés comme la lecture/écriture de fichiers, l'exécution de commandes shell, la recherche Google, etc.<br></br>Il permet de sauvegarder et de reprendre des conversations.<br></br><br></br>Des commandes spéciales (/stats, /memory, /tools, etc.) fournissent des informations sur l'utilisation et le contexte.<br></br><br></br>En résumé, Gemini CLI est un assistant IA puissant et polyvalent qui apporte la capacité de comprendre, générer et modifier du code directement dans votre environnement de terminal, améliorant considérablement l'efficacité des développeurs. Il est même disponible gratuitement avec des limites d'utilisation généreuses via un compte Google personnel.
```

Et bien sûr, la question suivante était : *Est-il possible de générer une application complète avec ?* Question à laquelle j’ai eu une réponse claire :

```
Oui, il est tout à fait possible de générer une application complète avec Gemini CLI. C'est même l'une de ses capacités les plus impressionnantes et un des objectifs de sa conception.<br></br><br></br>Voici comment Gemini CLI y parvient et pourquoi il est un "game-changer" pour la création d'applications :<br></br><br></br>Compréhension du contexte étendu (1 million de tokens) : Contrairement à d'autres outils qui traitent le code par petits morceaux, Gemini CLI peut ingérer et comprendre des bases de code entières, même très volumineuses. Cela lui permet d'avoir une vision globale du projet et de générer du code cohérent et intégré.<br></br><br></br>Interaction en langage naturel : Vous n'avez pas besoin d'écrire des commandes complexes. Vous pouvez simplement lui dire ce que vous voulez : "Crée-moi une application de gestion de tâches simple avec HTML, CSS et JavaScript", ou "Développe une API backend en Python pour gérer des utilisateurs avec FastAPI". Gemini CLI interprétera vos requêtes et agira en conséquence.<br></br><br></br>Scaffolding de projet (génération de structure) : Gemini CLI peut initialiser la structure d'un nouveau projet, créer les dossiers, les fichiers de base et les configurations nécessaires pour le framework ou la technologie que vous spécifiez. Par exemple, il peut créer un projet Flask, React, Node.js, etc.<br></br><br></br>Génération de code incrémentale et itérative : Vous pouvez commencer par une demande générale, puis affiner l'application par étapes. Par exemple :<br></br>"Crée une application web simple pour afficher des données."<br></br>"Ajoute une fonctionnalité pour filtrer les données."<br></br>"Améliore l'interface utilisateur pour qu'elle soit plus moderne."<br></br>"Ajoute une base de données pour stocker les informations."Gemini CLI maintient le contexte de la conversation, ce qui lui permet d'itérer sur le code existant et d'apporter des modifications intelligentes.<br></br><br></br>Utilisation des outils intégrés et des extensions (MCP) : Gemini CLI peut utiliser des outils comme la lecture/écriture de fichiers, l'exécution de commandes shell, la recherche web, etc., pour accomplir ses tâches. De plus, avec le "Model Context Protocol (MCP)", il peut s'intégrer à des serveurs externes pour étendre encore plus ses capacités, par exemple en se connectant à des bases de données ou à d'autres services.<br></br><br></br>Découpage des tâches complexes : Pour générer une application complète, Gemini CLI ne se contente pas de "jeter" du code. Il peut décomposer la tâche en étapes plus petites, comme analyser les exigences, concevoir l'architecture, créer la structure des fichiers, écrire les composants principaux, ajouter le style et les interactions, tester l'application et documenter le code.<br></br><br></br>Génération de tests et débogage : Non seulement il peut générer du code, mais il peut aussi générer des tests unitaires pour le code qu'il produit, et même vous aider à identifier et corriger les bogues.<br></br><br></br>Automatisation du déploiement (avec les bons outils) : Bien que Gemini CLI lui-même ne soit pas un outil de déploiement, il peut vous aider à générer les configurations nécessaires (comme des Dockerfiles) et même interagir avec des services cloud (comme Google Cloud Run) pour déployer l'application une fois qu'elle est prête.<br></br><br></br>En somme, Gemini CLI agit comme un assistant de développement intelligent qui peut prendre en charge l'ensemble du cycle de vie de la création d'une application, de la conception initiale à la génération du code et même au débogage, le tout depuis votre terminal et en utilisant le langage naturel. C'est un outil puissant pour accélérer le développement et prototyper rapidement des idées.
```

Alors bien sûr, j’ai voulu tester. Pas question d’un *Hello World*. Je voulais un vrai test, avec une app déjà en production comme référence.

Ayant récemment développé une appli qui récupère des documents, les envoie à un LLM pour générer un résumé et en extraire des notions, j’ai voulu voir si cette IA faisait mieux que moi. En même temps, je ne suis pas un expert en Python, j’ai donc pensé que si je pouvais le faire, lui pouvait aussi.

## Le cahier des charges

J’avais l’appli, j’en voulais une version simplifiée. J’ai donc gardé les éléments essentiels, avec une petite spécificité : je voulais utiliser AlloyDB Omni, la base de données compatible PostgreSQL de Google.

### Le prompt pour Gemini CLI

L’un des points essentiels quand on utilise l’IA, est de bien définir son prompt. Ce n’est d’ailleurs pas un trait distinctif de l’IA, quel que soit le contexte, poser une question claire et précise augmente forcément les chances d’obtenir une réponse claire et précise. J’ai donc repris mon cahier des charges et je l’ai transposé en instructions claires pour Gemini Cli. Je l’ai d’abord rédigé en anglais, mais au final, je l’ai traduit, pour être certain de bien définir chaque point :

```
#Fonctionnalités :<br></br><br></br>Les utilisateurs peuvent télécharger un fichier (.txt, .rtf, .docx, .odt, .md)<br></br>Le backend extrait le contenu texte brut en utilisant des bibliothèques appropriées aux formats (python-docx, odfpy, etc.)<br></br>Le texte extrait est envoyé à un LLM (via une API compatible OpenAI sur DeepInfra, Fireworks ou OpenRouter) pour :<br></br>Résumé (environ 5 à 10 lignes)<br></br>Extraction de mots clés (10 à 20 concepts pertinents, en minuscules, retournés en JSON)<br></br><br></br>#Gestion des fichiers :<br></br>Le fichier téléchargé d'origine doit être stocké sur le serveur avec des métadonnées<br></br>L'utilisateur doit pouvoir télécharger ou consulter le fichier d'origine depuis l'interface utilisateur<br></br><br></br>#Pile technologique :<br></br>Frontend : React avec TailwindCSS (propre et minimaliste)<br></br>Backend : FastAPI (Python), asynchrone lorsque cela est approprié<br></br>Base de données : AlloyDB (compatible PostgreSQL)<br></br>Déploiement : Docker (configuration multi-service si nécessaire)<br></br>Clé API : configurable via un fichier .env<br></br><br></br>#Contraintes :<br></br>Pas d'OCR, pas de prise en charge des PDF ni des images<br></br>Pas de cartographie des relations entre les mots clés<br></br>Code modulaire, bien commenté<br></br>Inclure une recherche basée sur les mots clés dans le frontend<br></br><br></br>#Configuration de Git et de l'environnement :<br></br>Initialiser un dépôt Git et pousser sur GitHub<br></br>Ajouter un fichier .gitignore qui exclut les artefacts de construction, les environnements virtuels et les fichiers sensibles (par exemple, .env, __pycache__, node_modules, etc.)<br></br>Créer un fichier .env.sample qui inclut toutes les variables d'environnement attendues (par exemple, LLM_API_KEY, LLM_API_BASE)<br></br>Ne pas valider le fichier .env réel
```

La demande était à mon avis suffisamment claire pour que l’IA me génère l’appli demandée.

Et après une vingtaine de minutes, et après avoir répondu à quelques questions, et validé un certain nombre d’actions, j’avais effectivement une application complète à disposition. Complète, oui, mais utilisable, non.

Et Gemini CLI n’avait pas respecté mon cahier des charges :

- il n’avait pas initialisé d’environnement `Git`.
- Il n’avait pas (et c’est logique) créé de fichier `.gitignore`, ni de fichier `.env.sample`.
- Mais il n’avait pas non plus répondu à ma demande concernant le SGBD à utiliser : il a sans sourciller remplacé AlloyDB Omni par PostgreSQL.

Alors, ce n’est pas une hérésie, vu que AlloyDB Omni est 100% compatible PostreSQL. Mais quand je demande un ristretto, je ne veux pas un espresso. Ni d’un café allongé. Les deux sont compatibles, oui. Mais les performances, sur de gros datasets, ne sont pas les mêmes. Et PostgreSQL nécessite `pgvector` pour la gestion d’une base vectorielle, quand AlloyDB la gère nativement.

Il ne faut pas oublier non plus qu’AlloyDB Omni est une base de données Google, je pensais donc que ce point ne lui poserait pas problème. Comme quoi…

Je lui ai fourni un second prompt pour qu’il corrige ces points :

```
# Requête de correction Gemini CLI — Projet "Readwise Light"<br></br><br></br>## Problèmes constatés<br></br>Le projet généré ne respecte pas plusieurs consignes pourtant explicites. Merci de corriger les points suivants de manière prioritaire et rigoureuse.<br></br><br></br>## 1. Remplacer PostgreSQL par **AlloyDB**<br></br>> L’image utilisée est `postgres:15-alpine`, ce qui ne respecte pas la consigne.<br></br><br></br>### Ce qui est attendu :<br></br>```yaml<br></br>services:<br></br>  db:<br></br>    image: google/alloydbomni:latest<br></br>    container_name: alloydb<br></br>    environment:<br></br>      - POSTGRES_USER=user<br></br>      - POSTGRES_PASSWORD=password<br></br>      - POSTGRES_DB=readwise<br></br>    ports:<br></br>      - "5432:5432"<br></br>    volumes:<br></br>      - postgres_data:/var/lib/postgresql/data<br></br>```<br></br><br></br>**Référence DockerHub** :<br></br>[https://hub.docker.com/r/google/alloydbomni/tags]<br></br><br></br>## 2. Ajouter un fichier `.env` et un `.env.sample`<br></br>> Aucun fichier `.env` n’a été généré malgré la mention explicite de cette exigence.<br></br><br></br>### Exemple de `.env` attendu :<br></br><br></br>```env<br></br>DB_HOST=db<br></br>DB_PORT=5432<br></br>DB_NAME=readwise<br></br>DB_USER=xxxxx<br></br>DB_PASSWORD=**********<br></br>SECRET_KEY=xxxxxxx<br></br>API_URL=https://xxxxx.ai/api/v1/xxx/xxx<br></br>API_KEY=***************<br></br>MODEL=qwen/qwen3-30b-a3b:free<br></br>```<br></br><br></br>### Et un `.env.sample` (sans clés privées) :<br></br><br></br>```env<br></br>DB_HOST=db<br></br>DB_PORT=5432<br></br>DB_NAME=readwise<br></br>DB_USER=xxxxx<br></br>DB_PASSWORD=**********<br></br>SECRET_KEY=xxxxxxx<br></br>API_URL=https://xxxxxxx.ai/api/v1/xxx/xxx<br></br>API_KEY=***************<br></br>MODEL=xxxxxxxxxxxx```<br></br><br></br>### Pense à ajouter `.env` dans `.gitignore`<br></br>
```

Alors oui, Gemini CLI a corrigé une partie des points demandés. Sans pour autant créer de référentiel Git. Je n’ai pas insisté, j’ai créé mon dépôt, j’ai committé et basta.

Et au delà de ses points de détail (oui, il faut savoir être indulgent), son appli n’était pas fonctionnelle. J’ai dû repasser derrière lui pour obtenir ce que je voulais :

Je lui ai fourni un ensemble clé / url dans le cahier des charges initial pour pour l’accès au LLM, il s’est empressé de créer une clé fictive et de changer l’url d’accès à l’API.

Par contre rien à redire au niveau de la structure elle-même :

```
readwise_light/<br></br>├──  README.md                    # Documentation principale<br></br>├──  docker-compose.yml           # Configuration Docker Compose<br></br>├──  Dockerfile.backend           # Image Docker backend<br></br>├──  Dockerfile.frontend          # Image Docker frontend<br></br>├──  .gitignore                   # Fichiers ignorés par Git<br></br>│<br></br>├──  backend/                     # Application backend<br></br>│   ├──  requirements.txt         # Dépendances Python<br></br>│   ├──  app/                     # Code source backend<br></br>│   │   ├──  main.py              # Point d'entrée FastAPI<br></br>│   │   ├──  models.py            # Modèles de données SQLModel<br></br>│   │   ├──  crud.py              # Opérations base de données<br></br>│   │   ├──  db.py                # Configuration base de données<br></br>│   │   ├──  text_extractor.py    # Extraction de texte<br></br>│   │   └──  llm.py               # Intégration LLM<br></br>│   └──  uploads/                 # Stockage des fichiers uploadés<br></br>│       └──  README.md<br></br>│<br></br>└──  frontend/                    # Application frontend<br></br>    ├──  package.json             # Dépendances Node.js<br></br>    ├──  vite.config.js           # Configuration Vite<br></br>    ├──  tailwind.config.js       # Configuration TailwindCSS<br></br>    ├──  postcss.config.js        # Configuration PostCSS<br></br>    ├──  eslint.config.js         # Configuration ESLint<br></br>    ├──  index.html               # Template HTML principal<br></br>    ├──  src/                     # Code source frontend<br></br>    │   ├──  main.jsx             # Point d'entrée React<br></br>    │   ├──  App.jsx              # Composant principal<br></br>    │   ├──  App.css              # Styles spécifiques<br></br>    │   ├──  index.css            # Styles globaux<br></br>    │   └──  assets/              # Ressources statiques<br></br>    └──  public/                  # Fichiers publics<br></br>        └──  vite.svg
```

Et son code est relativement bien écrit et documenté, comme on peut le voir ci-dessous (fichier `backend/app/main.py`) :

```
from fastapi import FastAPI, UploadFile, File, Depends, HTTPException
from fastapi.responses import FileResponse
from sqlmodel import Session
from . import crud, models, text_extractor, llm
from .db import engine, create_db_and_tables
import shutil
import os
import json
from typing import List

app = FastAPI()

def get_db():
    with Session(engine) as session:
        yield session

@app.on_event("startup")
def on_startup():
    create_db_and_tables()

@app.post("/upload/")
async def upload_file(file: UploadFile = File(...), db: Session = Depends(get_db)):
    # Create a directory to store uploaded files
    upload_dir = "uploads"
    os.makedirs(upload_dir, exist_ok=True)

    # Save the uploaded file
    file_path = os.path.join(upload_dir, file.filename)
    with open(file_path, "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)

    # Extract text from the file
    try:
        content = text_extractor.extract_text(file_path)
    except ValueError as e:
        raise HTTPException(status_code=400, detail=str(e))

    # Get summary and keywords from LLM
    llm_response = llm.get_summary_and_keywords(content)

    # Parse the llm_response to get summary and keywords
    try:
        # Find the start of the JSON block
        json_start_index = llm_response.find("```json")
        if json_start_index == -1:
            raise ValueError("JSON block not found in the LLM response")

        summary = llm_response[:json_start_index].strip()
        json_str = llm_response[json_start_index + 7:].strip().strip("`")
        keywords = json.loads(json_str)
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Error parsing LLM response: {e}")

    # Create document object
    doc = models.Document(
        filename=file.filename,
        content=content,
        summary=summary,
    )
    doc.set_keywords(keywords)
    db_doc = crud.create_document(db, doc)

    return db_doc

@app.get("/documents/", response_model=List[models.Document])
def get_documents(db: Session = Depends(get_db)):
    return crud.get_all_documents(db)

@app.get("/documents/{doc_id}", response_model=models.Document)
def get_document(doc_id: int, db: Session = Depends(get_db)):
    doc = crud.get_document(db, doc_id)
    if doc is None:
        raise HTTPException(status_code=404, detail="Document not found")
    return doc

@app.get("/download/{doc_id}")
def download_file(doc_id: int, db: Session = Depends(get_db)):
    doc = crud.get_document(db, doc_id)
    if doc is None:
        raise HTTPException(status_code=404, detail="Document not found")
    
    file_path = os.path.join("uploads", doc.filename)
    if not os.path.exists(file_path):
        raise HTTPException(status_code=404, detail="File not found")

    return FileResponse(file_path, filename=doc.filename)

@app.get("/search/")
def search_documents(query: str, db: Session = Depends(get_db)):
    # A simple search implementation
    docs = crud.get_all_documents(db)
    results = []
    for doc in docs:
        if query.lower() in doc.content.lower() or any(query.lower() in k.lower() for k in doc.get_keywords()):
            results.append(doc)
    return results

@app.get("/")
def read_root():
    return {"Hello": "World"}
```

Le code est plutôt propre et lisible dans l’ensemble, avec une architecture modulaire (séparation en modules `crud`, `models`, `text_extractor`, `llm`, etc.).

### Les points positifs

- **Séparation des responsabilités :** les fonctions de traitement (LLM, extraction de texte, base de données) sont bien isolées.
- **Utilisation correcte de** `Depends()` et de `Session()` avec SQLModel : le pattern de dépendance est propre.
- **Vérifications robustes** : 
    - Existence du fichier
    - Retour du LLM parsé avec fallback si le JSON est mal formé
- **Upload de fichiers et persistance** : tout est fonctionnel, logique, clair.
- **Conforme aux bonnes pratiques FastAPI** (par exemple l’usage de `@app.on_event("startup")` pour initier la base de données).

### Les points à améliorer

Quelques failles de robustesse (fichiers uploadés, parsing LLM, validation des entrées), mais rien de bloquant.

Pour une application “production ready”, il faudrait :

- isoler le parsing LLM dans une fonction solide avec tests unitaires,
- ajouter un logger propre,
- mettre en place une vérification MIME et un hash de fichier pour éviter les doublons.

Mais dans l’état, ce n’était pas demandé, on n’est plus dans le contexte initialement défini.

Par contre, j’avais hâte de voir le résultat, on peut avoir un code sublime, mais qui ne fait pas ce qu’on lui demande, j’ai donc procédé au…

## Test de l’application en ligne de commande

C’est là que tout a commencé à se gâter : rien n’était fonctionnel. Et j’ai passé près de deux heures à mettre tout en ordre, en faisant appel à la console Gemini CLI mais surtout à d’autres LLM (chatGPT et Claude d’Anthropic) en situation de blocage.

Voici une liste non exhaustive des points sur lesquels j’ai dû intervenir :

- L’appel au LLM – bien que je lui ai indiqué qu’il fallait passer par `openrouter.ai` pour appeler le LLM, Gemini CLI appelait directement l’API d’OpenAI : `platform.openai.com` – j’ai réécrit la fonction incriminée, après avoir tenté de le lui faire faire
- J’ai dû ajouter `python-multipart` dans `requirements.txt` et `curl` dans le `Dockerfile`
- Forcer la sortie en `json` pour obtenir un résultat interprétable. A nouveau, réécriture partielle d’un script.

Une fois ces point réglés, le texte a pu être uploadé et traité en lançant `curl -F "file=@README.md" http://localhost:8000/upload/`.

Sortie `json` correcte, et enregistrement en base de données effectué.

Mais pas encore de…

## Test depuis un navigateur

L’affaire se corse : impossible d’accéder à l’appli via le navigateur. En remplaçant `"dev": "vite"` par `"dev": "vite --host 0.0.0.0"` dans packages.json, le problème initial est réglé.

J’accède enfin à l’application. Tout n’est pas rose pour autant. Tout ce que j’obtiens, c’est un sublime message d’erreur :

```
[plugin:vite:css] [postcss] It looks like you're trying to use ```
tailwindcss
``` directly as a PostCSS plugin. The PostCSS plugin has moved to a separate package, so to continue using Tailwind CSS with PostCSS you'll need to install ```
@tailwindcss/postcss
``` and update your PostCSS configuration. /app/src/index.css:undefined:null at We (/app/node_modules/tailwindcss/dist/lib.js:35:2121) at LazyResult.runOnRoot (/app/node_modules/postcss/lib/lazy-result.js:361:16) at LazyResult.runAsync (/app/node_modules/postcss/lib/lazy-result.js:290:26) at LazyResult.async (/app/node_modules/postcss/lib/lazy-result.js:192:30) at LazyResult.then (/app/node_modules/postcss/lib/lazy-result.js:436:17) Click outside, press Esc key, or fix the code to dismiss. You can also disable this overlay by setting server.hmr.overlay to false in vite.config.js.
```

Et là, Gemini CLI échoue lamentablement quand je lui demande de corriger en lui indiquant l’erreur. Il rame, tourne à vide pendant près de 10 minutes, je le désactive et je tente avec un autre outil : [blackbox.ai](https://www.blackbox.ai/). Qui va me réécrire une bonne partie du code frontend. Sans pour autant régler le problème. `npm` aura eu raison de Gemini CLI et de Blackbox.

Je finis par demander une réécriture complète du frontend à Gemini CLI :

```
Tu es un générateur d'application web. Tu dois produire un frontend statique en HTML/CSS/JavaScript (sans framework, sans bundler) qui remplace entièrement un frontend existant dans une application FastAPI.<br></br><br></br>Contraintes :<br></br>- Le backend FastAPI existe déjà et expose une route POST /extract qui accepte un fichier (.txt, .md, .docx, .rtf, .odt) et retourne un JSON avec :<br></br>  - summary : une chaîne (résumé)<br></br>  - keywords : une liste de chaînes<br></br>- Le frontend doit proposer :<br></br>  1. un input file,<br></br>  2. envoyer ce fichier via fetch à /extract en POST,<br></br>  3. afficher le résumé et les mots-clés reçus, dans des conteneurs séparés.<br></br>- Tu dois générer uniquement des fichiers purs :<br></br>  - frontend/index.html<br></br>  - frontend/style.css<br></br>  - frontend/script.js<br></br>- Aucune dépendance externe ni outil ("npm", "webpack", etc.).<br></br>- L’ensemble doit être responsive, clair visuellement et ergonomique.<br></br>- Ajoute un feedback utilisateur (chargement, erreurs).<br></br>- Présente les trois fichiers complets de façon structurée (avec balises appropriées).<br></br><br></br>Donne également les instructions pour intégrer ces fichiers dans le projet FastAPI existant (montage `StaticFiles`).<br></br>
```

Et là, rien à dire, à part un petit bug visuel, Gemini CLI fait bien ce qu’on lui demande, mais en deux temps et sans pouvoir compléter certaines tâches. Il ne parvient pas par exemple à réécrire un fichier `Dockerfile.frontend` correct, je dois prendre la main. Mais j’obtiens l’interface demandée, et je réussis à analyser un fichier. Pour les besoins du test, j’ai pris une version texte de l’article sur [le grand gâchis numérique](https://githubcopilotchallenge.tsw.ovh/analyse-des-donnees-le-grand-gachis-numerique/), voici le résumé servi par `Qwen 3 30B A3B` :

```
L'article explore la sous-utilisation de l'intelligence artificielle (IA) par les PME et TPE malgré l'accessibilité des outils technologiques. Il souligne les barrières culturelles, financières et techniques qui empêchent leur adoption, comparant la situation à une 'fracture numérique' où les grandes entreprises dominent les ressources et les compétences. Les solutions open source comme spaCy ou Hugging Face sont citées comme des alternatives abordables, mais leur mise en œuvre reste complexe. L'auteur appelle à une démocratisation réelle via des formations, un accompagnement et des outils simplifiés, avant que les PME ne soient marginalisées dans un futur proche.
```

Il me ressort également les mots / expressions clé :

```
intelligence artificielle, automatisation, PME, TPE, fracture numérique, GAFAM, BATX, outils open source, analyse des avis clients, analyse de sentiment, formation, coût, complexité, démocratisation, écart technologique, innovation, exploitation des données, transformation numérique, barrières d’adoption, ghetto numérique
```

Le rendu est correct – il n’y a que “Analyse en cours…” qui tourne en permanence, rien de désastreux :

[

![Sortie frontend générée par Gemini Cli](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/frontend-readwise-light.jpg)

](https://githubcopilotchallenge.tsw.ovh/wp-content/uploads/2025/07/frontend-readwise-light.jpg)## Bilan de l’expérience

Alors, Gemini CLI est-il capable de créer une application à partir d’un cahier de charges relativement précis ?

On va dire que créer une application complète avec cet outil est possible – mais pas sans effort.

Voici les principaux enseignements que j’en retire :

### Les points positifs :

- L’outil comprend rapidement les intentions exprimées en langage naturel.
- Il génère une structure de projet cohérente et bien modulaire.
- Le code est propre, bien commenté, et suit généralement les bonnes pratiques.
- Il gère les échanges itératifs (prompt → code → correction) de manière fluide.

### Les limites et frustrations :

- Il ne respecte pas toujours scrupuleusement le cahier des charges, même clair et bien formaté.
- Des oublis réguliers sur des éléments clés comme `.gitignore`, `.env.sample`, init Git, ou même le choix du SGBD.
- Il prend parfois des décisions “raisonnables” à ma place (comme remplacer AlloyDB par PostgreSQL sans prévenir).
- Il ne rend pas une application prête à l’emploi : des ajustements manuels restent indispensables.

### Test de reprise de contexte et modifications a posteriori

L’un des aspects les plus critiques d’un assistant IA de développement est sa capacité à reprendre le travail sur du code existant après une déconnexion. Pour tester cette fonctionnalité, j’ai volontairement fermé la console Gemini CLI et attendu le lendemain pour demander des modifications sur l’application générée.

#### Reconnaissance du projet existant

**Temps de reprise de contexte : moins de 3 minutes**

En relançant Gemini CLI dans le répertoire du projet, l’outil a immédiatement analysé la structure de fichiers et le code existant. Sans que j’aie besoin de lui rappeler le contexte ou les spécifications initiales, il m’a proposé un plan d’action détaillé pour les améliorations demandées.

**Points positifs de cette reprise :**

- Identification correcte de l’architecture existante (FastAPI + React + AlloyDB)
- Reconnaissance des points problématiques que j’avais corrigés manuellement la veille
- Proposition de modifications cohérentes avec la structure modulaire mise en place

#### Un cas concret : la réécriture du frontend

Face aux problèmes insurmontables avec le frontend React/Vite initial, j’ai demandé à Gemini CLI de réécrire complètement cette partie en HTML/CSS/JavaScript pur. L’outil a parfaitement compris :

- Les routes backend existantes (`POST /extract`)
- Le format de données attendu (JSON avec `summary` et `keywords`)
- Les contraintes techniques (pas de framework, pas de bundler)

**Résultat :** Une interface fonctionnelle générée rapidement, avec seulement quelques ajustements mineurs nécessaires (comme la correction du fichier `Dockerfile.frontend`).

#### Bilan de la reprise de contexte

**Verdict : très convaincant**

La capacité de Gemini CLI à reprendre le travail sur du code existant est l’un de ses points forts les plus marquants. Le **contexte étendu de 1 million de tokens** se traduit par une **vraie efficacité** pratique pour la maintenance et l’évolution de code, positionnant l’outil comme un véritable “partenaire de développement” capable d’assurer une continuité dans le temps.

### Comparaison avec d’autres outils

J’ai déjà utilisé Claude ainsi que chatGPT pour développer des applications en ligne. Le code produit par Gemini Cli n’est ni plus ni moins valable que celui produit par ses concurrent, même si – en version gratuite, Claude ne permet pas d’aller très loin, et si chatGPT se perd souvent en cours de route. Il est évident que la limite de contexte de Gemini CLI n’y est pas pour rien : 1 million de tokens, ce n’est pas rien, surtout pour une petit projet. Mais les reproches faits aux deux autres outils sont à relativiser :

- si chatGPT se perd souvent, son approche est plus solide, et son code tout aussi bien structuré
- si Claude n’est pas envisageable dans sa version gratuite, le peu que j’ai pu voir de et outil me donne envie de tester la version payante

Concernant Blackbox, je n’ai pas réussi à me faire une opinion. Il a un intérêt, certes, mais il ne m’a pas permis d’aller au-delà de ce que Gemini Cli a pu faire.

### Analyse du coût/bénéfice temporel

Il a fallu moins d’une demi heure à Gemini CLI pour générer une application complète. Imparfaite, mais complète. En comptant les 2 à 3 heures de réglages supplémentaires, et la rédaction du cahier des charges, on n’atteint pas les 4 heures. Combien de temps m’aurait-il fallu pour arriver au même résultat, en partant de zéro, si j’avais dû tout coder par moi-même ?

### En résumé :

Gemini CLI est un **assistant IA puissant** pour accélérer le prototypage et la mise en route d’un projet, mais il ne remplace pas un développeur humain. Il offre un bon gain de temps pour **générer la base** d’une application, mais nécessite toujours un œil critique, des retouches, et une phase de test rigoureuse.

C’est un outil prometteur, mais encore imparfait pour du **“Code as a Service”** automatisé de bout en bout.

Et moi, j’ai appris de cette expérience. Pour l’essentiel…

## Les leçons que j’en ai tiré

### La précision du cahier des charges ne garantit pas le respect à 100%

Même avec des spécifications détaillées et explicites (AlloyDB, fichiers .env, init Git), l’IA prend des “libertés” qu’elle juge raisonnables. Il faut s’attendre à devoir recentrer l’outil sur les exigences non négociables.

### Gemini CLI est excellent pour le scaffolding, moins pour les finitions

Gemini CLI excelle pour générer une structure cohérente et du code propre, mais les détails d’intégration (configuration Docker, dépendances spécifiques, URLs d’API) nécessitent systématiquement une intervention humaine.

### Le contexte étendu est un vrai game-changer

La capacité à reprendre le travail le lendemain en moins de 3 minutes et à comprendre l’architecture existante différencie nettement Gemini CLI des autres outils IA.

### Prévoir du temps de debug même sur du code “propre”

Il ne faut pas confondre code bien structuré et code fonctionnel. Les 2 heures de corrections ne sont pas du temps perdu mais un **investissement prévisible** dans tout projet généré par IA.

### L’IA a ses propres “convictions techniques”

PostgreSQL vs AlloyDB, choix d’URLs d’API… L’outil fait des substitutions qu’il considère comme équivalentes. Il faut explicitement insister sur les choix non négociables.

### Adopter une approche itérative payante

Partir d’une demande globale puis affiner par étapes fonctionne mieux que d’essayer de tout spécifier d’un coup. L’exemple du frontend HTML/CSS/JS pur en est la preuve.

### Garder des alternatives sous la main

ChatGPT et Claude ont été nécessaires pour débloquer certaines situations. Gemini CLI n’est pas omnipotent, avoir un plan B évite les blocages prolongés.

### Le ROI est positif malgré les corrections

Même avec les corrections nécessaires, le gain de temps reste significatif par rapport à un développement from scratch, surtout pour le prototypage rapide.

Gemini CLI est donc **un excellent accélérateur de développement** à condition d’accepter son rôle d’assistant perfectible plutôt que de développeur autonome.

Voici le workflow que je recommande pour exploiter Gemini CLI de façon fiable :

```
graph TD
A[Spécifications techniques] --> AA[Prompt clair]
AA --> B{Génération initiale}
B --> C[Scaffolding]
C --> D[Revue manuelle critique]
D --> E{Corrections IA}
E --> F[Tests intensifs]
F --> G[Déploiement]
G --> H[Documentation]
```

## Le mot de la fin

En résumé, Gemini CLI impressionne par la puissance de son moteur de génération, mais reste perfectible dans le respect strict d’un cahier des charges complexe. L’outil est prometteur pour des prototypes ou MVPs, mais nécessite encore une supervision humaine pour les projets sérieux.

**Le verdict** : Ni fantasme ni réalité absolue, mais un **outil de transition** vers une nouvelle façon de développer où l’humain reste indispensable pour la vision, la cohérence et la qualité finale.

## Aller plus loin

Cette application constitue une base solide pour de nombreuses évolutions. Voici quelques pistes d’amélioration que vous pourriez explorer :

### Fonctionnalités avancées

**Analyse sémantique poussée** : Remplacer la recherche par mots-clés par une recherche vectorielle basée sur des embeddings. Cela permettrait de retrouver des documents par similarité sémantique plutôt que par correspondance exacte.

**Multi-LLM et comparaison** : Intégrer plusieurs modèles (Claude, GPT, Llama) pour générer différents résumés du même document et laisser l’utilisateur choisir le plus pertinent, ou même fusionner les analyses.

**Classification automatique** : Ajouter un système de catégorisation automatique des documents par domaine (technique, juridique, marketing, etc.) avec un modèle de classification pré-entraîné.

**Historique et versioning** : Conserver l’historique des analyses d’un même document pour suivre l’évolution des résumés selon les différentes versions du fichier.

### Améliorations techniques

**Traitement asynchrone** : Implémenter une queue Redis + Celery pour traiter les gros fichiers en arrière-plan avec notification en temps réel du statut.

**Cache intelligent** : Éviter de retraiter des documents identiques en utilisant un système de hash MD5 et de mise en cache des résultats.

**API robuste** : Ajouter le versioning (`/v1/`, `/v2/`), la limitation de taux, la validation avancée des fichiers et un système de monitoring des performances.

### Visualisation et analytics

**Dashboard de visualisation** : Créer des graphiques sur les mots-clés les plus fréquents, l’évolution temporelle des concepts, ou encore un nuage de mots interactif.

**Export enrichi** : Proposer des exports en PDF avec résumés formatés, CSV des métadonnées, ou encore des formats compatibles avec Obsidian/Notion pour l’intégration dans des systèmes de gestion de connaissances.

### Vers la production

**Authentification et rôles** : Système d’utilisateurs avec JWT, gestion des permissions et espaces de travail collaboratifs.

**Sécurité renforcée** : Scan antivirus des fichiers uploadés, validation stricte des types MIME, et système de backup automatique.

### Expérimentations IA avancées

**RAG conversationnel** : Transformer l’application en assistant capable de répondre à des questions sur l’ensemble des documents uploadés en utilisant le contexte des fichiers analysés.

**Résumés adaptatifs** : Ajuster automatiquement la longueur et le style du résumé selon le type de document détecté (rapport technique vs article de blog).

**Extraction d’entités structurées** : Identifier et extraire automatiquement les personnes, lieux, dates, organisations mentionnées avec des outils comme spaCy ou des modèles Transformers spécialisés.

---

**Le défi ultime ?** Demander à Gemini CLI d’implémenter ces améliorations une par une. Ce serait un excellent moyen de tester ses capacités d’évolution sur du code existant tout en enrichissant progressivement l’application !

Le dépôt est accessible sur Github : [https://github.com/pcescato/readwise\_light](https://github.com/pcescato/readwise_light)

Sentez vous libre de le récupérer, de contribuer, d’en faire un fork… et de commenter cet article.