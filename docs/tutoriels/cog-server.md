# Intégration des données rasters dans Seatizen Monitoring

L'intégration des données raster (Orthophotos, Bathymétrie, Prédictions, etc.) à la plateforme Seatizen Monitoring nécessite une optimisation préalable en raison de la taille importante des fichiers. Ces données sont servies par un serveur dédié, distinct du site web principal.

## 1. Le dépôt `cog-server`

Tout le processus d'optimisation, de stockage et de service des rasters est géré par le dépôt **`cog-server`** : [https://github.com/SeatizenDOI/cog-server](https://github.com/SeatizenDOI/cog-server).

Ce dépôt contient deux composantes essentielles :

| Composante | Rôle |
| --- | --- |
| **Le dossier tools** | Contient les scripts Python pour convertir vos rasters bruts en format **Cloud Optimized Geotiff (COG)**. |
| **Le serveur** | L'application web qui sert les rasters COG optimisés, permettant une visualisation rapide sur la carte. |

## 2. Préparation et organisation des fichiers rasters

### ⚙️ Étape 1 : Optimisation des rasters

Avant le déploiement, vous devez traiter vos fichiers pour les rendre compatibles avec le serveur.

Utilisez les scripts disponibles dans le dossier `cog-server/tools/`. Chaque sous-dossier (`bathy`, `ortho`, `pred_drone`, etc.) contient un `README.md` expliquant les étapes spécifiques pour optimiser les rasters de ce type.
<div align="center">
<figure>
<img src="/img/tutoriels/cog_tree.png" alt="Architecture du dossier tools">
<figcaption>Architecture du dossier tools</figcaption>
</figure>
</div>

### 📂 Étape 2 : Structure du dossier de données (`data`)

Une fois optimisés (ils devraient se terminer par `_cog.tif`), les fichiers doivent être stockés dans un dossier `data` en respectant la hiérarchie suivante (par type de données et par année) :

```bash
.
├── bathy
│   ├── 2022
│   │   ├── bathy_group_9_color_cog.tif
│   │   └── bathy_group_9_depth_cog.tif
│   ├── 2023
│   │   ├── bathy_group_9_color_cog.tif
│   │   └── bathy_group_9_depth_cog.tif
│   ├── 2024
│   │   ├── bathy_group_5_color_cog.tif
│   │   └── bathy_group_5_depth_cog.tif
│   └── 2025
│       ├── bathy_group_5_color_cog.tif
│       └── bathy_group_5_depth_cog.tif
├── ign
│   ├── 2017
│   │   └── 974-2017-0340-7640-U40S-0M20-E080_clipped_cog.tif
│   └── 2022
│       └── 974-2022-0340-7640-U40S-0M20-E080_clipped_cog.tif
├── ortho
│   ├── 2021
│   │   └── 20211202_REU-BOUCAN_AUV-1_01_ortho_cog.tif
│   ├── 2022
│   │   └── 20221025_SYC-aldabra-arm01_UAV-02_33_ortho_cog.tif
│   ├── 2023
│   │   └── 2023_ASV_AME_STLEU_odm_orthophoto_cog.tif
│   ├── 2024
│   │   └── 20240628_REU-HERMITAGE_ASV-1_01_ortho_cog.tif
│   └── 2025
│       └── 2025_ASV_AME_STLEU_odm_orthophoto_cog.tif
├── pred_asv
│   ├── 2022
│   │   ├── group_2022_ACROPORE_BRANCHED_0_colored_cog.tif
│   │   └── group_2022_THALASSODENDRON_CILIATUM_9_colored_cog.tif
│   ├── 2023
│   │   └── group_2023_THALASSODENDRON_CILIATUM_9_colored_cog.tif
│   ├── 2024
│   │   └── group_2024_THALASSODENDRON_CILIATUM_6_colored_cog.tif
│   ├── 2025
│   │   └── group_2025_THALASSODENDRON_CILIATUM_6_colored_cog.tif
│   └── color_asv_pred_by_specie.json
├── pred_drone
│   ├── 2023
│   │   ├── 20231208_REU-ST-LEU_UAV-01_06_ortho_merged_predictions_color_cog.tif
│   │   └── 20231208_REU-ST-LEU_UAV-01_06_ortho_merged_predictions_preddata_cog.tif
│   ├── 2024
│   │   ├── 20240407_REU-ST-LEU-PORT_UAV-01_03_ortho_merged_predictions_color_cog.tif
│   │   └── 20240407_REU-ST-LEU-PORT_UAV-01_03_ortho_merged_predictions_preddata_cog.tif
│   └── 2025
│       ├── 20250517_REU-LA-SALINE_UAV-01_01_ortho_merged_predictions_color_cog.tif
│       └── 20250517_REU-LA-SALINE_UAV-01_01_ortho_merged_predictions_preddata_cog.tif
├── pred_ign
│   ├── 2017
│   │   ├── 974-2017-0340-7640-U40S-0M20-E080_merged_predictions_color_cog.tif
│   │   └── 974-2017-0340-7640-U40S-0M20-E080_merged_predictions_preddata_cog.tif
│   └── 2022
│       ├── 974-2022-0310-7670-U40S-0M20-E080_merged_predictions_color_cog.tif
│       └── 974-2022-0310-7670-U40S-0M20-E080_merged_predictions_preddata_cog.tif
└── transparent.png

```

### 🎨 Gestion des doubles fichiers (Bathy, Prédictions)

Les catégories **`bathy`**, **`pred_drone`** et **`pred_ign`** requièrent deux fichiers COG par raster pour séparer la visualisation de la donnée :

| Type de Fichier | Rôle | Objectif |
| --- | --- | --- |
| **`*_color_cog.tif`** | Couche visuelle | Sert uniquement à afficher les couleurs sur la carte. |
| **`*_preddata_cog.tif`** | Couche de données | Contient les valeurs du raster (profondeur, labels de prédiction, etc.), permettant à l'utilisateur de **cliquer** sur la carte pour obtenir une information précise. |

## 3. Déploiement et mise à jour du serveur

Le déploiement et la configuration complète du serveur sont détaillés dans le `README.md` du dépôt principal `cog-server`.

🔗 **Lien du tutoriel de déploiement :** [https://github.com/SeatizenDOI/cog-server/blob/master/README.md](https://github.com/SeatizenDOI/cog-server/blob/master/README.md)

### 🚀 Mise à jour des données sur le VPS

Pour mettre à jour les données sur le serveur VPS (celui de Sylvain) après l'optimisation :

1. **Connexion :** Utilisez un client FTP/SFTP comme **FileZilla** pour vous connecter au VPS.
2. **Transfert :** Transférez le contenu de votre nouveau dossier `data` (contenant les sous-dossiers `bathy`, `ortho`, etc.) vers le répertoire de données du serveur :
`/home/debian/villien/data`
3. **Redémarrage :** Relancez le conteneur Docker pour qu'il prenne en compte les nouveaux fichiers. Utilisez la commande suivante via SSH : `docker container restart cog-server`
