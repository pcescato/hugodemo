---
title: "Migrer de WordPress vers Hugo en 2025 : le guide complet"
date: "2025-08-26T11:31:32+00:00"
lastmod: "2025-08-26T11:31:32+00:00"
draft: false
description: "Quitter WordPress pour Hugo, c’est troquer la richesse d’un CMS contre la vitesse et la simplicité d’un site statique. Retour d’expérience sur cette transition."
tags:
  - Hugo
  - performances
  - WordPress
categories:
  - Tutoriels
author: githubcopilotchallenge
---

Ce n’est un secret pour personne, je suis comme tout blogueur et comme tout propriétaire de site obsédé par la rapidité de chargement des pages des mes sites. Et le moins qu’on puisse dire, c’est que WordPress ne facilite pas les choses.

Entre PHP qui génère ses pages à la volée, les appels à la base de données et les plugins… sans cache point de salut, et avec un cache, tout n’est pas gagné pour autant.

Bon, je ne vais pas te mentir, j’ai réussi à optimiser mon site, grâce à OpenLiteSpeed (voir l’article [Configurer un VPS avec OpenLiteSpeed et LiteSpeed Cache](https://tsw.ovh/cyberpanel-openlitespeed-lscache/) sur tsw.ovh, mon blog spécial WordPress) et en optant pour un VPS d’entrée de gamme performant – celui de chez [Layer7](https://githubcopilotchallenge.tsw.ovh/go/layer7) que je teste dans l’article [VPS low cost : Layer7 vs Contabo](https://githubcopilotchallenge.tsw.ovh/vps-low-cost-layer7-vs-contabo/). Alors…

## Pourquoi tenter l’aventure Hugo ?

Mon site a beau être rapide, je sais aussi que chaque dixième de seconde grapillé est essentiel pour le référencement et surtout pour mes lecteurs. Un site lent, c’est l’assurance de perdre 50% des lecteurs avant même leur arrivée sur la page. Alors voila, j’ai entendu parler de Hugo, et je me suis dit : pourquoi pas ?

Pour autant, quand on a longtemps utilisé WordPress, l’idée de tout casser pour repartir sur Hugo peut donner des sueurs froides. Tu tombes sur des articles de blogueurs qui ont tenté l’expérience… et qui en sont revenus dépités : problèmes de migration, images perdues, liens cassés. Bref, de quoi hésiter.

Pourtant, si comme moi tu rêves d’un site **rapide comme l’éclair**, **sans maintenance lourde**, **sécurisé par défaut** (pas de base SQL ni de plugins à patcher tous les 15 jours), Hugo est une évidence. Et je ne suis pas du genre à me laisser arrêter par quelques expériences ratées. Quand j’ai décidé de tester, je fonce – et voici le tuto que j’aurais aimé trouver.

Comme toujours, il est prévu pour être exécuté sur un serveur Ubuntu / Debian, mais est adaptable à toute autre distribution de Linux (on n’aura pas de `apt install` par exemple, mais `yum install` pour les distros `CentOS` / `AlmaLinux`, `Fedora`…). Voila, c’est dit, on peut s’y mettre…

### Prérequis

- **Niveau technique** : Ce guide s’adresse à des utilisateurs ayant des bases en ligne de commande Linux
- **Accès serveur** : Vous devez avoir accès `root/sudo` à un serveur Ubuntu/Debian
- **Connaissances minimales** : Notions de base en édition de fichiers (nano/vim) et navigation système
- **Temps estimé** : Comptez 2 à 3 heures pour une migration complète

### Avant de commencer : Hugo en quelques mots

Avant de plonger dans l’installation et la migration, il est utile de comprendre comment **Hugo** fonctionne :

- Hugo est un **générateur de site statique** : il prend tes contenus au format Markdown (`.md`) et les transforme en pages HTML. Différence clé : WordPress génère les pages à chaque visite (dynamique), Hugo les pré-génère une fois (statique). Résultat : vitesse maximale mais pas de commentaires natifs, de recherche ou d’admin web.
- La logique repose sur deux piliers : 
    - `Content/` : c’est ton répertoire d’articles et de pages, chaque fichier `.md` contient un *front matter* (métadonnées : titre, date, etc.) et le corps en Markdown.
    - `Layouts/` et `Themes/` : ce sont les gabarits (templates). Hugo assemble ton contenu `.md` avec ces templates pour générer le HTML final.
- Les `shortcodes` sont des petits morceaux de code réutilisables (par exemple pour gérer les images, intégrer une vidéo, etc.) – exactement comme dans WordPress. `[‌gallery‌]` devient `{{gallery}}`.
- Enfin, `static/` contient tous les fichiers servis tels quels (images, CSS, JS).

En résumé : tu écris en Markdown, Hugo s’occupe de la mise en forme et génère un site statique prêt à héberger partout.

### Structure type d’un projet Hugo

Quand tu crées un site avec `hugo new site monsite`, tu obtiens quelque chose comme :

```
monsite/
├── archetypes/        # modèles de front matter pour les nouveaux contenus
├── content/           # tes articles et pages en Markdown (.md)
│   └── posts/
│       └── mon-article.md
├── layouts/           # gabarits personnalisés (templates)
├── static/            # fichiers statiques servis tels quels (images, css…)
├── themes/            # thèmes externes (ex : Hextra)
├── config.toml        # configuration du site (ou .yaml / .json)
```

### Rédiger en Markdown

Chaque fichier `.md` commence par un bloc *front matter*. Le *front matter*, c’est l’en-tête de chaque article en `YAML` (entre les `---`) qui contient les métadonnées : titre, date, catégories… Exactement comme les champs personnalisés de WordPress, mais en plus simple :

```
---
title: "Exemple"
slug: exemple
date: 2025-08-20 12:00:00
draft: false
categories:
  - "Intelligence artificielle"
tags:
  - "C#"
  - "Python:tips"
---

# Titre principal

paragraphe avec du **gras** et de *l'italique*, un lien [mon domaine](https://mondomaine.com)

- un élément dans une liste à puces
- un deuxième élément dans la liste

> une citation
```

### Quels éditeurs Markdown utiliser ?

- **[Visual Studio Code](https://code.visualstudio.com/)** avec l’extension *Markdown All in One* (prévisualisation intégrée).
- **[Obsidian](https://obsidian.md/)** : parfait si tu aimes organiser tes notes comme un wiki.
- **[Dillinger](https://dillinger.io/)** : éditeur WYSIWYG en ligne, avec des capacités d’export avancées (Markdown, PDF, HTML).
- Même un simple **éditeur de texte** (nano, vim, notepad++) suffit : Hugo se fiche de l’outil tant que le fichier est en `.md`.

## Installe Hugo Extended

Pourquoi Extended ? Parce que sans cette version, impossible d’utiliser les thèmes modernes (Sass, pipelines d’assets, etc.).

```
apt remove hugo -y   # si une vieille version traîne
cd /usr/local/bin
wget https://github.com/gohugoio/hugo/releases/latest/download/hugo_extended_0.157.0_Linux-64bit.tar.gz
tar -xzf hugo_extended_0.157.0_Linux-64bit.tar.gz
rm hugo_extended_0.157.0_Linux-64bit.tar.gz

hugo version
```

Tu dois voir quelque chose comme :

```
hugo v0.148.2-40c3d8233d4b123eff74725e5766fc6272f0a84d+extended linux/amd64 BuildDate=2025-07-27T12:43:24Z VendorInfo=gohugoio
```

Hugo est installé, ton environnement de travail est prêt…

## Crée ton site

Va dans ton dossier de travail (par exemple `/root/site` ou `~/site`) et tape :

```
cd /root/site
hugo new site hugo-hextra
cd hugo-hextra
```

À ce stade, tu as la structure Hugo de base : `content/`, `themes/`, `config.toml`, etc.
Encore vide, mais prête à accueillir ton futur site.

## Ajoute un thème

Pour ce tuto, j’ai opté pour Hextra, un thème moderne qui rappelle *[Astra Pro](https://githubcopilotchallenge.tsw.ovh/wpastra)* sous WordPress : rapide, clair, complet (c’est d’ailleurs celui que j’utilise le plus souvent avec WordPress). Il y a tout un ensemble de thèmes que tu peux trouver dans les ressources (en fin d’article) ou avec [une simple recherche Google](https://www.google.com/search?q=hugo+themes).

Il n’y en a pas autant que pour WordPress, mais il y a déjà de quoi habiller ton site avec un style unique. Et entre nous, la personnalisation est bien plus simple qu’avec WordPress.

```
git init
git submodule add https://github.com/imfing/hextra themes/hextra
```

Puis copie sa config par défaut :

```
cp themes/hextra/hugo.toml ./hugo.toml
```

## Configure ton site

Édite le fichier `hugo.toml` et ajoute au moins ces lignes en haut :

```
baseURL = "http://192.168.1.100/"
title = "Test Hextra sur serveur"
theme = "hextra"
```

👉 Remplace `192.168.1.100` par l’IP ou domaine réel de ton serveur.

## Lance Hugo sur ton réseau

Pour tester depuis un autre appareil du réseau, tape :

```
hugo server -D --bind=0.0.0.0
```

Tu dois voir :

```
Watching for changes in /root/site/{archetypes,assets,content,data,i18n,layouts,static,themes}
Watching for config changes in /root/site/hugo.toml, /root/site/config/_default, /root/site/themes/hextra/config.toml, /root/site/themes/hextra/config/_default
Start building sites …
hugo v0.148.2-40c3d8233d4b123eff74725e5766fc6272f0a84d+extended linux/amd64 BuildDate=2025-07-27T12:43:24Z VendorInfo=gohugoio

                  │ EN
──────────────────┼─────
 Pages            │  49
 Paginator pages  │   0
 Non-page files   │   0
 Static files     │ 235
 Processed images │   0
 Aliases          │  16
 Cleaned          │   0

Built in 293 ms
Environment: "development"
Serving pages from disk
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 0.0.0.0)
Press Ctrl+C to stop
```

Depuis ton PC ou ton smartphone sur le même réseau, ouvre :

```
http://192.168.1.100:1313/
```

Si tu as une erreur **connection refused**, ouvre le port côté firewall :

```
ufw allow 1313/tcp
```

## Importe ton contenu WordPress (script maison, sans wp2hugo)

Ici on suit **ma** procédure : export WordPress → conversion Markdown via **mon script Python**. Pas d’outil magique qui décide à ta place.

Les outils magiques, je les ai testés, et très honnêtement, ils sont pour beaucoup je pense dans l’insatisfaction des blogueurs qui ont voulu poasser à Hugo : les plugins pour WorPress qui te promettent du Markdown te rendent un Markdown vérolé quasi-inutilisable. Quant à *wp2hugo*, j’ai fait l’impasse dessus pour avoir ce que moi je voulais.

### Exporte depuis WordPress

- Tableau de bord → **Outils → Exporter → Tous les contenus** → tu récupères un fichier `.xml` (WXR).
- Copie ce fichier sur le serveur Hugo (ex. `/root/wp-export.xml`).

### Prépare l’environnement de conversion

```
# Dépendances système
apt update && apt install -y python3 python3-venv python3-pip pandoc

# Dans le dossier du projet Hugo
cd /root/hugo-hextra
python3 -m venv venv-hugo
source venv-hugo/bin/activate
pip install lxml beautifulsoup4 pyyaml
```

### Script de conversion WP → Markdown

Crée `tools/wp_xml_to_md.py` :

```
#!/usr/bin/env python3
import os, subprocess, re
from lxml import etree
from datetime import datetime

INPUT_XML = "/root/wp-export.xml"  # chemin vers l’export WXR
OUTPUT_DIR = "content/posts"       # où écrire les .md
os.makedirs(OUTPUT_DIR, exist_ok=True)

NS = {
    "wp": "http://wordpress.org/export/1.2/",
    "content": "http://purl.org/rss/1.0/modules/content/",
}

def to_slug(s):
    return "".join(c.lower() if c.isalnum() or c in "-" else "-" for c in s).strip("-")

def yaml_safe(s: str) -> str:
    """Ajoute des guillemets si nécessaire pour YAML"""
    if re.search(r"[:#\-\?\[\]\{\},&\*!\|>'\"%@`]|^\s|\s$", s) or " " in s:
        return f"\"{s.replace('\"', '\\\"')}\""
    return s

def safe_decode(data: bytes) -> str:
    """Essaie UTF-8 puis fallback en latin-1 → utf-8"""
    try:
        return data.decode("utf-8")
    except UnicodeDecodeError:
        # fallback en latin-1
        return data.decode("latin-1").encode("utf-8").decode("utf-8")

root = etree.parse(INPUT_XML, parser=etree.XMLParser(encoding="utf-8"))

for item in root.xpath("//item"):
    status = item.findtext("{http://wordpress.org/export/1.2/}status") or ""
    post_type = item.findtext("{http://wordpress.org/export/1.2/}post_type") or "post"
    if status != "publish" or post_type not in {"post", "page"}:
        continue

    title = (item.findtext("title") or "Sans titre").strip()
    slug = (item.findtext("{http://wordpress.org/export/1.2/}post_name") or to_slug(title))

    date = (
        item.findtext("{http://wordpress.org/export/1.0/modules/content/}post_date")
        or item.findtext("{http://wordpress.org/export/1.2/}post_date")
        or datetime.utcnow().isoformat()
    )

    html = item.findtext("{http://purl.org/rss/1.0/modules/content/}encoded") or ""

    # Conversion HTML → Markdown via pandoc
    p = subprocess.run(
        ["pandoc", "-f", "html", "-t", "gfm-smart", "--wrap=none"],
        input=html.encode("utf-8"),
        capture_output=True,
    )
    md = safe_decode(p.stdout).strip()

    # Récupération des catégories et tags
    categories = [
        cat.text.strip()
        for cat in item.findall("category")
        if cat.get("domain") == "category" and cat.text
    ]
    tags = [
        cat.text.strip()
        for cat in item.findall("category")
        if cat.get("domain") == "post_tag" and cat.text
    ]

    # Front matter YAML
    fm = [
        "---",
        f'title: "{title.replace("\\", "\\\\").replace("\"", "\\\"")}"',
        f"slug: {slug}",
        f"date: {date}",
        "draft: false",
    ]
    if categories:
        fm.append("categories:")
        for c in categories:
            fm.append(f"  - {yaml_safe(c)}")
    if tags:
        fm.append("tags:")
        for t in tags:
            fm.append(f"  - {yaml_safe(t)}")
    fm.extend(["---", ""])

    out_name = f"{date[:10]}-{slug}.md" if post_type == "post" else f"{slug}.md"

    with open(os.path.join(OUTPUT_DIR, out_name), "w", encoding="utf-8") as f:
        f.write("\n".join(fm) + md + "\n")

print("Conversion terminée →", OUTPUT_DIR)
```

Exécute :

```
python tools/wp_xml_to_md.py
```

Tu obtiens des fichiers **Markdown propres** dans `content/posts/`, sans bidouille.

## Gére les images (import + conversion AVIF/WebP + shortcode)

Objectif : **récupérer tes images WordPress**, les convertir en **AVIF** + **WebP** (avec fallback), puis les servir via un **shortcode** Hugo.

### Outils requis

Option Python (simple et portable) :

```
source venv-hugo/bin/activate
pip install requests pillow pillow-avif-plugin
# (selon la distro, prévoir libavif/libaom côté système pour l’AVIF)
```

Option CLI (rapide et robuste) :

```
apt install -y libavif-bin webp  # avifenc / cwebp
```

### Télécharge et convertis les images (sans toucher aux .md)

Crée `tools/images_fetch_convert.py` :

```
#!/usr/bin/env python3
import os, re, subprocess, shutil, requests
from pathlib import Path
from io import BytesIO
from PIL import Image
import pillow_avif  # noqa: F401 (active le support AVIF)

MD_DIR = Path("content")
OUT_DIR = Path("static/images")
OUT_DIR.mkdir(parents=True, exist_ok=True)

IMG_MD = re.compile(r'!\[([^\]]*)\]\((https?://[^)]+)\)')
IMG_HTML = re.compile(r'<img[^>]+src="([^"]+)"[^>]*>', re.I)

def save_bytes_as(img_bytes, base_noext):
    # Original (debug)
    with open(base_noext + ".orig", "wb") as f: f.write(img_bytes)
    # Pillow → AVIF/WebP (fallback CLI si besoin)
    try:
        img = Image.open(BytesIO(img_bytes))
        img.save(base_noext + ".avif", "AVIF")
    except Exception:
        if shutil.which("avifenc"):
            tmp = base_noext + ".tmp"
            with open(tmp, "wb") as f: f.write(img_bytes)
            subprocess.run(["avifenc", tmp, base_noext + ".avif"]); os.remove(tmp)
    try:
        img = Image.open(BytesIO(img_bytes))
        img.save(base_noext + ".webp", "WEBP")
    except Exception:
        if shutil.which("cwebp"):
            tmp = base_noext + ".tmp"
            with open(tmp, "wb") as f: f.write(img_bytes)
            subprocess.run(["cwebp", "-q", "80", tmp, "-o", base_noext + ".webp"]); os.remove(tmp)

def process_url(url):
    filename = url.split("?")[0].split("/")[-1]
    base = OUT_DIR / Path(filename).name
    base_noext = str(base.with_suffix(''))
    r = requests.get(url, timeout=30)
    r.raise_for_status()
    save_bytes_as(r.content, base_noext)
    print("✔", filename)

for md in MD_DIR.rglob("*.md"):
    text = md.read_text(encoding="utf-8")
    urls = set([u for _, u in IMG_MD.findall(text)] + IMG_HTML.findall(text))
    for u in urls:
        if u.startswith("http"):
            try: process_url(u)
            except Exception as e: print("✖", u, e)

print("Terminé →", OUT_DIR)
```

Lance :

```
python tools/images_fetch_convert.py
```

Résultat : pour chaque image trouvée, tu obtiens `static/images/mon-image.avif`, `static/images/mon-image.webp` et `static/images/mon-image.jpg`.

### Shortcode `img.html` (balise `<picture>` avec fallback)

Crée `layouts/shortcodes/img.html` :

```
{{ $src := .Get "src" }}
{{ $alt := .Get "alt" | default "" }}
<picture>
  <source type="image/avif" />
  <source type="image/webp" />
  <img src="{{ $src }}.jpg" alt="{{ $alt }}" loading="lazy" />
</picture>
```

Appel dans tes `.md` (**sans extension**) :

```
{{< img src="/images/n8n-add-nodes" alt="n8n – ajouter un nœud" >}}
```

### Réécris automatiquement les `.md` pour remplacer `

![]()

` / `<figure>` par le shortcode

Tu as deux options pour compléter cette étape, tu testes et en fonction du résultat, tu choisis celle qui donne le meilleur résultat sur ton contenu. Ou tu adaptes, si aucune ne te convient parfaitement.

- Version rapide basée sur regex (Markdown `

![]()

`) :

```
# tools/replace_md_images_with_shortcode.py
import os, re
ROOT = "content"
BASE = "/images"
pat = re.compile(r'!\[([^\]]*)\]\((https?://[^)]+)\)')
for root, _, files in os.walk(ROOT):
    for fn in files:
        if not fn.endswith('.md'): continue
        p = os.path.join(root, fn)
        txt = open(p, encoding='utf-8').read()
        def repl(m):
            alt, url = m.group(1), m.group(2)
            name = os.path.basename(url.split('?')[0])
            noext = os.path.splitext(name)[0]
            return f"{{{{< img src=\"{BASE}/{noext}\" alt=\"{alt}\" >}}}}"
        new = pat.sub(repl, txt)
        if new != txt:
            open(p,'w',encoding='utf-8').write(new)
            print('✔', p)
```

- Version HTML avec `<figure>` WordPress :

```
# tools/replace_figure_with_shortcode.py
from bs4 import BeautifulSoup
import os
ROOT = "content"
BASE = "/images"
for root, _, files in os.walk(ROOT):
    for fn in files:
        if not fn.endswith('.md'): continue
        p = os.path.join(root, fn)
        txt = open(p, encoding='utf-8').read()
        soup = BeautifulSoup(txt, 'html.parser')
        changed = False
        for fig in soup.find_all('figure'):
            img = fig.find('img')
            if not img or not img.get('src'): continue
            url = img['src']
            alt = img.get('alt','')
            name = os.path.basename(url.split('?')[0])
            noext = os.path.splitext(name)[0]
            shortcode = f"{{{{< img src=\"{BASE}/{noext}\" alt=\"{alt}\" >}}}}"
            fig.replace_with(shortcode)
            changed = True
        if changed:
            open(p,'w',encoding='utf-8').write(str(soup))
            print('✔', p)
```

## Déploie ton site statique

Une fois satisfait du rendu, il ne te reste plus qu’à publier ton site :

```
hugo
cp -r public/* /var/www/html/
```

✅ Ton site est désormais accessible en clair via `http://192.168.1.16/`. Tu n’as plus besoin de Hugo jusqu’à la prochaine mise à jour, tu pourrais même le désinstaller, ton site resterait visible, tant que tu as un serveur web (Apache, NGINX, Caddy…) bien configuré.

Pour déployer ton site sur internet, n’oublie pas le volet sécurité : c’est du HTML, d’accord, mais celà ne t’affranchis pas de la gestion des certificats SSL, et de l’ajout des en-têtes de sécurité. Il y a moins de risques qu’avec un site dynamique, mais le risque zéro n’existe pas.

## Avant la migration : Checklist indispensable

Avant de te lancer dans la migration, il est primordial de t’assurer que tu es prêt à toute éventualité et que tu n’auras **aucune mauvaise surprise**. Voici une checklist **à respecter inconditionnellement** :

- **Backup intégral du site et de la base de données**
    - Sauvegarde tous les fichiers WP (FTP/tar/zip du dossier), exports de la base MySQL, éventuels dossiers uploads ou custom.
    - Vérifie la validité des sauvegardes sur un autre serveur/test : c’est la seule façon de « revenir en arrière » sans tout perdre en cas de pépin.
- **Établis un plan précis d’action**
    - Quelles redirections mettre en place ? (anciens liens → nouveaux liens Hugo, gestion SEO)
    - Quelles fonctionnalités sont perdues ou changent ? (recherche, formulaires, commentaires : faut-il prévoir Disqus/Remark42, Algolia…)
    - Y a-t-il des phases temporaires d’indisponibilité ? Un message d’info pour les lecteurs ?
- **Batterie de tests multi-navigateurs et devices avant mise en production**
    - N’attends pas d’être en “prod” pour vérifier que le nouveau site s’affiche bien sur Chrome, Firefox, Opera, Safari, Android, iOS.
    - Vérifie aussi la vitesse sur mobile (3G/4G), le responsive, l’accessibilité visuelle et la navigation sans souris (tabulation, lecteurs d’écran).
    - Repère bien ce qui diffère entre les navigateurs : la fidélité n’est pas automatique

## Aller plus loin

*Ce guide t’offre une procédure clé en main pour une migration propre et sereine, mais il y reste de nombreuses optimisations, notamment si tu vises la performance absolue ou la gestion de gros volumes. Voici quelques points que je n’ai pas abordés en détail :*

- **Performance pour les gros sites**
    - Utilisation avancée de `partialCached` pour éviter les recalculs inutiles sur des milliers de pages.
    - Profiter des pipelines de cache (Hugo Pipes) pour compresser / minifier / fingerprinter / vérifier l’empreinte des assets CSS et JS.
    - Utilisation de PostCSS pour automatiser les préfixes CSS ou les transformations avancées.
    - Bundler les fichiers pour limiter le nombre de requêtes HTTP.
- **Organisation optimale des médias**
    - Gérer les images de chaque article via les [Page Bundles](https://gohugo.io/content-management/page-bundles/) (dans le même dossier que le Markdown, plus facile pour du multi-format AVIF/WebP).
    - Mettre les images “globales” (logo, favicons…) plutôt dans `assets/medias` que dans `static/images` pour profiter des transformations Hugo Pipes.
- **Workflow avancé**
    - Automatiser les builds avec CI/CD (Netlify, GitHub Actions, Clever Cloud…) pour déployer sans erreur et profiter du cache incrémental.
    - Optimiser la gestion des menus, catégories/tags, et intégrer un moteur de recherche statique (Lunr, Algolia…).

Autant de points que j’aborderai peut-être dans de prochains articles, si j’en ai le temps…

## L’envers du décor – expérimentation, erreurs et blocages…

Quand on expérimente, on n’a pas forcément le résultat attendu au premier essai. Avant de produire ce tuto, j’ai dû tester, tenter une approche, une autre, parfois beaucoup d’autres…

Alors ne te décourage pas si tu rencontres :

- **Erreur `try not defined`** → ta version de Hugo est trop vieille pour Hextra. Mets à jour.
- **404 en accédant à `http://IP/`** → il faut lancer `hugo` et copier le contenu du dossier `public/` dans `/var/www/html/`.
- **Impossible d’accéder depuis un autre appareil** → ajoute `--bind=0.0.0.0` et ouvre ton firewall.

Chaque blocage est une étape normale. Quand tu bloques, cherche une solution, soit via Google, soit en utilisant un assistant IA (Claude d’Anthropic, ChatGPT, DeepSeek, Kimi… sont des assistants intelligents qui trouveront une solution à ton blocage – et qui te l’expliqueront).

Et si tu es arrivé jusqu’ici, félicitations : tu as ton WordPress transformé en site statique rapide et moderne.

## Le mot de la fin : WordPress ou Hugo pour mon blog ?

Après tous ces tests et cette analyse approfondie, une question demeure : vais-je migrer mon propre blog vers Hugo ? La réponse est non, et voici pourquoi.

Mon site WordPress actuel, avec sa configuration OpenLiteSpeed et LS Cache, me donne entière satisfaction. Les 329ms de temps de chargement sur la page d’accueil restent excellents pour l’expérience utilisateur, et mon trafic actuel (quelques milliers de visiteurs par mois) ne justifie pas l’abandon de l’écosystème WordPress. J’y perdrais l’interface d’administration que j’apprécie, la flexibilité des plugins, et surtout la simplicité de publication depuis n’importe quel appareil.

Hugo représente une solution remarquable pour qui privilégie la performance pure et accepte les contraintes techniques qu’elle impose. Mais pour un blog personnel avec un trafic modéré, optimiser WordPress reste plus pragmatique que de tout reconstruire. Cette expérimentation m’a surtout confirmé que le choix de plateforme dépend moins des performances brutes que de l’adéquation avec ses besoins réels et sa façon de travailler.

Parfois, le mieux est l’ennemi du bien. Et dans mon cas, “assez rapide” avec WordPress vaut mieux que “très rapide” avec Hugo si cela implique de perdre mon confort de publication quotidien. Même si je suis un fan inconditionnel de Hugo.

Par contre, pour chaque nouveau projet, je me poserai forcément la question : *WordPress ou Hugo ?*

## Ressources

- Documentation officielle Hugo : <https://gohugo.io/documentation/>
- Configuration front matter Hugo : <https://gohugo.io/content-management/front-matter/>
- Guide Hugo sur les shortcodes : <https://gohugo.io/templates/shortcode-templates/>
- Thèmes (gratuits) Hugo : 
    - <https://themes.gohugo.io/>
    - <https://htmlrev.com/free-hugo-templates.html>
    - <https://github.com/statichunt>