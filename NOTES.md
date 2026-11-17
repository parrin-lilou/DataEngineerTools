# Mes notes – Data Engineering


# 1. Introduction au cours
## Définitions clés
### Crawler

Un **robot qui parcourt des sites web de façon générique**.

Il fonctionne sur **n'importe quel site**, sans dépendre de sa structure HTML. Il récupère les informations de base : texte, images, liens.

 

→ *Exemple : le robot de Google qui explore tout le web.*

 

### Scraper

Un **programme conçu pour un site précis**, qui récupère des informations **ciblées et de qualité** (prix, titres d'articles…).

Défaut : il est **fragile**. Si le site change son HTML, le scraper ne marche plus.

 

→ *Exemple : un programme qui récupère les prix sur un site de vente précis.*

 

| | Crawler | Scraper |

|---|---|---|

| Fonctionne sur | Tous les sites | Un site précis |

| Données récupérées | Générales (texte, liens, images) | Ciblées et précises |

| Maintenance | Facile | Difficile (casse si le site change) |

 

### Robots.txt

Un **fichier placé à la racine d'un site** (ex. `google.com/robots.txt`) qui indique aux robots **ce qu'ils ont le droit de visiter ou non**.

Il **n'empêche rien techniquement** : c'est une **règle de bonne conduite** à respecter.

 

### Sitemap (plan du site)

Une **page qui liste toute la structure d'un site**. Elle sert surtout au **SEO**, et elle est très pratique pour un robot, qui trouve toutes les pages d'un coup.

 

### SEO (Search Engine Optimization)

L'ensemble des techniques pour qu'un site **apparaisse en haut des résultats** des moteurs de recherche.

 

### Surcharge du serveur

Envoyer trop de requêtes trop vite peut **faire tomber un petit site**.

Bonne pratique : **faire des pauses entre les requêtes**. Certains sites, comme Wikipédia, l'imposent.

 

### API

Une **porte d'accès directe aux données** d'un site. Le site renvoie les données brutes, souvent en **JSON**, au lieu d'une page HTML.

 

### DOM

L'**arbre des balises HTML** d'une page (`<html>` → `<body>` → `<div>` → …).

 

## Comment les sites se protègent du scraping

 

- **Noms de balises HTML générés aléatoirement**, pour qu'on ne puisse pas s'appuyer dessus

- **Blocage d'adresses IP** repérées comme des robots

- **Contenu généré par JavaScript**, absent du HTML de départ (vu avec Google en TP)

- **Détection des comportements non humains** : vitesse, clics, scroll

 

## Les 2 méthodes de scraping

 

1. **Récupérer et parser le HTML** : on télécharge la page et on extrait les données des balises. Il faut comprendre le DOM.

2. **Récupérer les appels API** : on intercepte directement les données au format JSON.

   Plus efficace, mais ces appels sont souvent cachés ou bloqués.



Dans les deux cas, on utilise la librairie Python **`requests`**.

 

### L'inspecteur du navigateur

Clic droit → **Inspecter** (ou `Cmd + Option + I` sur Chrome) :

 

- **Elements** : le code HTML de la page, pour repérer les balises qui contiennent les données

- **Network** : tous les appels réseau, pour trouver les appels d'API (cocher **Keep log**)


## L'architecture d'un projet de données

 

Il vaut mieux concevoir une architecture **solide et scalable** dès le départ : ça coûte plus cher à développer, mais beaucoup moins cher à maintenir.

 

### 1. Récupération des données : les librairies Python

 

| Librairie | Rôle |

|---|---|

| **Requests** | Faire des requêtes HTTP (+ `requests_cache` pour éviter de retélécharger pendant le développement) |

| **BeautifulSoup** | Transformer le HTML en objet Python facile à explorer |

| **Readability** | Extraire le texte principal d'une page |

| **Scrapy** | Framework complet pour crawler et scraper efficacement |

 

### 2. Stockage : les bases de données

 

**SQL (relationnelles)** : données **structurées**, sous forme de tableaux

- **MySQL** : base relationnelle classique

- **PostgreSQL** : relationnelle, optimisée pour les données géographiques



**NoSQL** : données **hétérogènes et flexibles**, très utilisées pour le web

- **MongoDB** : stocke des documents JSON

- **Redis** : paires clé → valeur, ultra-rapide car tout est en mémoire (RAM)

- **Cassandra** : pensée pour de très gros volumes de données

- **ElasticSearch** : moteur de recherche, très efficace pour chercher dans du texte



### 3. Scalabilité

 

**Scalabilité** : la capacité d'un système à **gérer plus de charge** en ajoutant des machines.

 

Une seule machine est souvent trop lente, alors on **répartit le travail** entre plusieurs programmes grâce à une **file de messages** (*message queue*), qui distribue les tâches à plusieurs « travailleurs ».

 

- **RabbitMQ** : créé à l'origine pour les objets connectés

- **Kafka** : créé par LinkedIn pour le streaming de données en continu


##En une phrase

Le cours apprend à **récupérer des données du web** (requests, BeautifulSoup, Scrapy), à les **stocker** (MongoDB, Redis, ElasticSearch…) et à **passer à grande échelle**, tout en **respectant les bonnes pratiques** (robots.txt, pas de surcharge des serveurs).

---


# 2. Mémo TP – Environnement virtuel avec uv

 

| Commande | Rôle |

|---|---|

| `brew install uv` | Installer uv (Mac) |

| `uv python install 3.12` | Installer Python 3.12 |

| `uv sync` | Recréer l'environnement d'un projet **existant** (lit `pyproject.toml` + `uv.lock`) |

| `uv init` | Créer un **nouveau** projet à partir de zéro |

| `source .venv/bin/activate` | Activer l'environnement |

| `deactivate ` | Deactiver l'environnement |

| `uv add <package>` | Ajouter un package au projet |

| `uv run jupyter notebook` | Lancer Jupyter |

 

- `(dataengineertools)` au début du terminal = environnement activé (uv affiche le nom du projet au lieu de `.venv`)

- `which python` → doit finir par `.venv/bin/python`

- Chemin avec parenthèses → mettre des guillemets : `cd "/Users/lilou/Documents(PC)/..."`



---

 

# 3. Mémo Git

 

## Les 4 zones

 

```

Working directory --git add--> Staging area --git commit--> Local repository --git push--> Remote repository

```

 

- **Working directory** : mes fichiers sur l'ordi

- **Staging area** : les modifications choisies pour le prochain commit

- **Local repository** : l'historique des commits, sur mon ordi

- **Remote repository** : le dépôt en ligne (GitHub)



## Les 4 états d'un fichier

 

- **Untracked** : pas suivi par Git (nouveau fichier)

- **Unmodified** : pas modifié depuis le dernier commit

- **Modified** : modifié, mais pas encore ajouté

- **Staged** : ajouté avec `git add`, prêt à être commité



## Commandes

 

| Commande | Rôle |

|---|---|

| `git clone <URL>` | Copier un dépôt sur mon ordi |

| `git status` | Voir l'état des fichiers |

| `git add <fichier>` | Ajouter à la staging area |

| `git commit -m "message"` | Enregistrer un commit en local |

| `git push` | Envoyer mes commits sur GitHub |

| `git fetch` | Télécharger les nouveautés sans les appliquer |

| `git merge <branche>` | Fusionner une branche |

| `git pull` | `fetch` + `merge` en une commande |

 

## Fork vs Clone

 

| | Fork | Clone |

|---|---|---|

| Où va la copie | Sur GitHub, dans mon compte | Sur mon ordi |

| Comment | Bouton **Fork** sur GitHub | `git clone <URL>` |
 

## Remotes

 

- **remote** = raccourci vers l'URL d'un dépôt en ligne

- **origin** = mon dépôt (mon fork) → j'y pousse mon travail

- **upstream** (« en amont ») = le dépôt d'origine (le prof) → j'y récupère les mises à jour



```

git remote -v                       # voir les remotes

git push -u origin master           # 1er push : -u retient que master ↔ origin/master

git pull upstream master            # récupérer les mises à jour du prof

``` 

---

# 4. Mémo Docker

 

## C'est quoi Docker ?

 

**Docker** est un outil qui **met une application et tout ce dont elle a besoin** (librairies, versions, configuration) **dans une boîte isolée** appelée **conteneur**. Cette boîte tourne **de la même façon sur n'importe quelle machine** : mon Mac, l'ordi d'un camarade, un serveur dans le cloud.
 

**Utilité dans ce cours :** installer facilement des bases de données (MongoDB, Redis, ElasticSearch…) sans les installer « en vrai » sur mon Mac.

 

## Conteneurisation ≠ Virtualisation

 

| | Machine virtuelle (VM) | Conteneur Docker |

|---|---|---|

| Contient | Un système d'exploitation complet | Seulement l'application et ses dépendances |

| S'appuie sur | Son propre système | Le système de la machine hôte |

| Poids | Lourd (plusieurs Go) | Léger |

| Démarrage | Lent (minutes) | Rapide (secondes) |

 

- **Machine hôte** = la vraie machine sur laquelle Docker tourne (mon Mac)



## Les 3 concepts clés

 

### Dockerfile

Un **fichier texte qui contient la recette** pour construire une image : quel système de base, quelles dépendances installer, quel code copier, quelle commande lancer.

 

```dockerfile

FROM ubuntu:18.04        # partir d'une image existante

COPY . /app              # copier mes fichiers dans l'image

RUN make /app            # exécuter une commande pendant la construction

CMD python /app/app.py   # la commande lancée au démarrage du conteneur

```

 

| Instruction | Rôle |

|---|---|

| `FROM` | Image de départ (ex. `ubuntu`, `python:3.12`) |

| `COPY` | Copier des fichiers de ma machine vers l'image |

| `RUN` | Exécuter une commande **pendant la construction** de l'image |

| `CMD` | Commande lancée **au démarrage** du conteneur |

 

### Image

Le **résultat de la construction** du Dockerfile : un modèle figé, prêt à l'emploi. On ne la modifie pas, on s'en sert pour lancer des conteneurs.

 

### Conteneur

Une **image en train de tourner**. On peut lancer plusieurs conteneurs à partir d'une même image.

Par défaut, un conteneur est **isolé** : il a ses propres fichiers et on ne peut pas y accéder de l'extérieur. Il faut **ouvrir les ports** (`-p`) et **partager les dossiers** (`-v`) explicitement.

 

### 💡 Pour retenir

 

```

Dockerfile  --docker build-->  Image  --docker run-->  Conteneur

 (recette)                   (plat surgelé)          (plat servi)

```

 

En Python : **image = classe**, **conteneur = objet** (une image peut créer plusieurs conteneurs, comme une classe crée plusieurs objets).

 

## Commandes de base

 

| Commande | Rôle |

|---|---|

| `docker build -t <nom_image> .` | Construire une image à partir du Dockerfile |

| `docker run <nom_image>` | Lancer un conteneur à partir d'une image |

| `docker ps` | Lister les conteneurs **en cours** |

| `docker ps -a` | Lister **tous** les conteneurs (même arrêtés) |

| `docker logs <conteneur>` | Afficher les logs (ce que le conteneur écrit) |

| `docker stop <conteneur>` | Arrêter un conteneur |

| `docker restart <conteneur>` | Relancer un conteneur (depuis le début) |


### Options de `docker run`

- `-p <port_hôte>:<port_conteneur>` : **ouvrir un port**

  → `-p 8888:8888` : `localhost:8888` sur mon Mac mène au port 8888 du conteneur

- `-v <dossier_hôte>:<dossier_conteneur>` : **partager un dossier** entre mon Mac et le conteneur

  → `-v ./data:/app/data`

- On peut combiner plusieurs `-p` et `-v` dans une seule commande



Les options se placent **avant** le nom de l'image :

```

docker run -p 8888:8888 -v ./data:/app/data <nom_image>

```


 

### Options de `docker logs`

- `-f` (*follow*) : suivre les logs **en direct** (`Ctrl + C` pour quitter)

- `--tail=50` : afficher seulement les **50 dernières lignes**

```

docker logs -f --tail=50 <conteneur>

```

 

## Docker Compose

 

**Docker Compose** permet de **lancer plusieurs conteneurs ensemble** (ex. mon application + une base Redis + une base MongoDB) à partir d'un seul fichier de configuration : **`docker-compose.yml`**.

 

```yaml

services:

  web:                  # mon application

    build: .            # construite à partir du Dockerfile du dossier

    ports:

      - "5000:5000"

    volumes:

      - .:/code

    depends_on:

      - redis

  redis:                # une base Redis

    image: redis

  mongodb:              # une base MongoDB

    image: mongo

```

 

- **Service** = une application (un conteneur) du fichier

- `image:` → utiliser une image existante (téléchargée depuis **Docker Hub**, le « GitHub des images »)

- `build:` → construire l'image à partir de mon Dockerfile



| Commande | Rôle |

|---|---|

| `docker compose up` | Lancer tous les services |

| `docker compose up -d` | Lancer en arrière-plan (*detached*) |

| `docker compose down` | Arrêter et supprimer les conteneurs |