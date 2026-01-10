## 💻 Tutoriel : Déploiement de Seatizen Monitoring

**Seatizen Monitoring** est une application web dédiée à la visualisation et au monitoring des données. Elle est basée sur une architecture *Frontend* / *Backend* et gérée via Docker Compose.

* **Dépôt GitHub :** [github.com/SeatizenDOI/seatizen-monitoring](https://www.google.com/search?q=https://github.com/SeatizenDOI/seatizen-monitoring)

### 🌐 Architecture Technique Détaillée

L'application repose sur quatre conteneurs Docker pour assurer la gestion complète des données, de la source à l'interface utilisateur :

#### 1. Flux des Données

1. **Source :** Les données proviennent d'un **Geopackage** (`.gpkg`), un format de base de données SQLite optimisé pour les données géospatiales, téléchargé depuis Zenodo.
2. **Base de Données :** Bien que les données sources soient un Geopackage, elles sont chargées dans une base **PostgreSQL (PostGIS)**. Ce transfert est crucial pour des raisons de **performance** et de rapidité d'exécution des requêtes de l'application web.

#### 2. Les Conteneurs Docker

Le déploiement utilise quatre services conteneurisés :

| Conteneur | Technologie | Rôle |
| --- | --- | --- |
| **`postgis`** | PostgreSQL | Héberge la base de données. |
| **`sm-gdal`** | GDAL | **Outil d'Extraction :** Contient la bibliothèque GDAL, indispensable pour lire et extraire les données du Geopackage afin de les insérer dans la base PostGIS. |
| **`backend`** | FastAPI (Python) | Fournit l'API pour servir les données du PostgreSQL au Frontend. |
| **`frontend`** | Next.js | Interface utilisateur de l'application. |

### 🚀 Instructions de Démarrage

Le déploiement et la gestion de l'environnement se font intégralement via **Docker Compose**.

| Action | Commande | Rôle |
| --- | --- | --- |
| **Lancer** l'application | `docker compose up --build -d` | Démarre les quatre services en mode détaché (`-d`) après avoir reconstruit les images (`--build`). |
| **Arrêter** l'application | `docker compose down -v` | Arrête et supprime les conteneurs, et surtout, supprime les **volumes (`-v`)** associés. |

> **⚠️ Point de Vigilance**
> Pour éviter des problèmes au redémarrage (tels que des volumes orphelins ou des volumes montés non désirés), il est **impératif** d'utiliser la commande `docker compose down -v` lors de l'arrêt. L'option `-v` garantit la suppression propre des volumes créés par Docker.