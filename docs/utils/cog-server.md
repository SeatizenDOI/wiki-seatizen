
## 🗺️ Tutoriel : Intégration des Rasters (Orthophotos, Bathy, Prédictions)

L'ajout de données raster (Orthophotos, Bathy, Prédictions, etc.) à Seatizen Monitoring nécessite un traitement préalable des fichiers, car ils sont trop volumineux pour être servis directement. Ces rasters sont stockés sur un serveur dédié, séparé du site web principal.

### 📦 Utilisation du dépôt `cog-server`

Tout le processus de préparation et de service est géré par le dépôt **`cog-server`** : [https://github.com/SeatizenDOI/cog-server](https://github.com/SeatizenDOI/cog-server).

Ce dépôt contient deux éléments principaux :

1. **Le Serveur :** L'application qui sert les rasters optimisés.
2. **Le Dossier `tools` :** Contient les scripts nécessaires à la préparation des données.

### ⚙️ Préparation des Rasters avec `tools`

Pour chaque catégorie de raster, un traitement est appliqué pour les rendre légers et optimisés pour le web.

#### 1. Organisation des Données Sources

Avant d'exécuter les scripts, vous devez organiser vos données brutes dans une structure de dossiers spécifique.

> **Structure du Dossier `data` :**
> 1. Créez un dossier **`data`**.
> 2. À l'intérieur de `data`, créez six sous-dossiers, un pour chaque catégorie de données :

> * `bathy`
> * `ortho`
> * `pred_asv`
> * `pred_drone`
> * `pred_ign`


> 3. À l'intérieur de chacun de ces sous-dossiers (sauf `pred_asv`), vous devez créer des dossiers par **année** pour organiser les fichiers (Exemple : `data/ortho/2024`).
> 
> 

#### 2. Le Traitement COG (Cloud Optimized Geotiff)

Pour les rasters volumineux comme les Orthophotos, le traitement consiste à les convertir au format **COG (Cloud Optimized Geotiff)**.

* **Dossiers de Traitement :** Rendez-vous dans le dossier **`tools`** du dépôt. Vous y trouverez six sous-dossiers (correspondant aux six catégories de données), chacun contenant les scripts de préparation nécessaires.
* **Exemple (Orthophotos) :**
* Dans le dossier `ortho` (à l'intérieur de `tools`), vous trouverez plusieurs scripts.
* Le script **`0.convert_tif_to_cog.sh`** est utilisé pour convertir vos orthophotos brutes au format COG.
* Vous devez **renseigner le bon chemin** de votre dossier source dans ce script.


* **Résultat :** Le script convertit vos fichiers au format adéquat, les optimisant pour la diffusion.

#### 3. Mise à Disposition

Une fois que les fichiers sont convertis en COG (ou traités par les scripts spécifiques), il ne vous reste plus qu'à :

* **Téléverser** (Drag and Drop) les fichiers traités sur le serveur OVH avec **FileZilla**.
* Le **serveur COG** s'occupera ensuite de les mettre à disposition de Seatizen Monitoring (il faut le rédemarrer).