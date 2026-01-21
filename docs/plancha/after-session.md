# Traiter une session de planche

## Déchargement des données brutes

Créer un dossier `tmp` pour stocker temporairement les fichiers **bruts**. Le fichier du Reach RS3 se place à la racine, tandis qu'il faut créer un dossier spécifique par planche.

### GPS Base - Reach RS3

Dans un premier temps, récupérez les données sur le Reach RS2/3 :

* Allumez-le sans partage de connexion ; au bout d'un moment, il passera en mode **point d'accès (hotspot)**.
* Connectez-vous à son réseau Wi-Fi (le code est `emlidreach`).
* Une fois connecté, rendez-vous sur l'URL `192.168.42.1`.
* Allez dans l'onglet **Logging** et téléchargez l'archive `.zip` du jour.
* *Note : Sur les anciens firmwares, téléchargez manuellement les fichiers LLH et RINEX.*

### GPS Device - Reach M2

Pour le GPS de la planche :

* Allumez la planche. Au bout d'une minute ou deux, le Reach M2 passe en hotspot.
* Connectez-vous au réseau (mot de passe : `emlidreach`).
* Rendez-vous sur l'URL `192.168.42.1` dans l'onglet **Logging**.
* Téléchargez le fichier `.zip` (ou les fichiers LLH et RINEX pour les anciens firmwares) dans le dossier `tmp` de la planche associée. Il y a autant de fichiers .zip que de sessions réalisées.

### Fichiers de l'autopilote

L'autopilote (le Cube Orange) génère un fichier par mission, à condition d'avoir éteint la planche entre chaque session.

* Retirez la carte micro-SD et branchez-la sur le PC.
* Accédez au dossier `APM/LOGS`. Il contient des fichiers `.BIN` nommés par numéros.
* Ils sont classés par **ordre chronologique**. Vous pouvez vérifier cet ordre grâce au script `utils/verify_bin.py` du dépôt GitHub [plancha-workflow](https://github.com/SeatizenDOI/plancha-workflow).
* **Copiez** les fichiers `.BIN` dans le dossier temporaire de la planche, puis supprimez les fichiers sur la carte SD ainsi que le fichier `.txt` de 4 octets.

### Données vidéo

* Récupérez les vidéos sur la carte SD de la GoPro.
* Pour différencier les fichiers appartenant à une même session, observez les deux derniers chiffres du nom de fichier (ils restent identiques pour les chapitres d'une même vidéo). ``

### Rassembler les données brutes

Pour chaque session, créez un dossier respectant la nomenclature suivante :

`YYYYMMDD_country-code-optinal-place_device_session-number`

**Exemple pour deux planches et deux sessions à Saint-Leu :**

* 20250411_REU-ST-LEU_ASV-1_01
* 20250411_REU-ST-LEU_ASV-1_02
* 20250411_REU-ST-LEU_ASV-2_01
* 20250411_REU-ST-LEU_ASV-2_02

**Chaque dossier de session doit contenir l'arborescence suivante :**

```text
.20250411_REU-ST-LEU_ASV-1_01
    ├── DCIM       (Vidéos GoPro)
    ├── GPS
    │    ├── BASE     (Données du Reach RS3)
    │    └── DEVICE   (Données du Reach M2)
    └── SENSORS    (Fichier .BIN de la session)

```

À ce stade, les données brutes sont classées et prêtes à être exploitées.

> **⚠️ Important :** Il est nécessaire de noter les sessions réalisées dans un fichier Excel pour assurer le suivi.

> **⚠️ Conseil de sécurité :** En cas de doute ou de mauvaise manipulation, privilégiez toujours le **copier-coller** suivi d'une suppression manuelle plutôt que le déplacement direct des fichiers.

---


## Traitement des données : Synchronisation Images et Données Temporelles

Actuellement, les données brutes ne sont pas directement exploitables : les vidéos ne possèdent pas de coordonnées GPS associées, la bathymétrie n'est pas corrigée et le GPS n'est pas encore traité en **PPK** (Post-Processed Kinematic).

Pour automatiser ce flux, nous utilisons le dépôt [plancha-workflow](https://github.com/SeatizenDOI/plancha-workflow). Reportez-vous au `README` pour l'installation de l'environnement.

### 1. Gestion des sessions par batch (Fichier CSV)

Pour traiter plusieurs sessions simultanément, utilisez l'option `-csv`. Il est fortement recommandé d'utiliser un fichier CSV pour conserver une trace des paramètres appliqués.

**Exemple de structure CSV :**

```csv
session_name,time_first_frame,number_first_frame,filt_exclude_specific_timeUS,depth_range_max,depth_range_min,filt_exclude_specific_datetimeUTC,rgp_station
20240607_REU-HERMITAGE_ASV-1_01,2024:06:07 03:07:19.60,20,,20,,,
20250414_REU-TROU-DEAU_ASV-1_03,2025:04:14 04:29:56.00,10,"[[201262382,219662452]]",,,,

```

> ⚠️ **Note :** Si vous n'utilisez pas de CSV, vous devrez configurer manuellement le fichier `ppk_config.json`.


### Étape 1 : Synchronisation temporelle et premières corrections

L'objectif est de trouver le décalage entre l'horloge de la caméra et le temps GPS.

1. **Récupération de l'heure :** Exécutez la commande suivante :
`python workflow.py -csv csv/2025_plancha.csv -os`
2. **Analyse des frames :** Allez dans le dossier `PROCESSED_DATA/FRAMES`. Trouvez la frame affichant l'heure, notez son numéro (`number_first_frame`) et l'heure affichée.
3. **Conversion UTC :** Soustrayez **4 heures** (pour La Réunion) pour obtenir l'heure UTC. Exemple : `07:07:19.60` devient `03:07:19.60` dans le CSV.
4. **Vérification Qualité :**
* **GPS :** Consultez `GPS/DEVICE/GPS_ppk_position_accuracy.png`. Un bon géoréférencement doit afficher environ **90% de Q1 (Fix)**.
* **Bathy :** Vérifiez `PROCESSED_DATA/BATHY/depth_samples_utmcoord_preproc.png`. Si des valeurs sont aberrantes, renseignez les colonnes `depth_range_max` ou `min` dans votre CSV.


### Étape 2 : Traitement complet

Une fois la synchronisation renseignée dans le CSV, relancez le processus global :
`python workflow.py -csv csv/2025_plancha.csv`

**Ce que fait le script :**

* **Découpage :** Extraction à **2.997 fps** (adapté au format GoPro 23.98 fps).
* **Géolocalisation :** Attribution d'une position GPS précise à chaque image.
* **Métadonnées :** Écriture des données dans les tags **EXIF** (position, attitude, etc.).


### Résultats

<div align="center">
    <figure>
        <img src="/img/plancha/fin_gps.png" alt="Fichier GPS.">
        <figcaption>Fichier représentant les positions de la planches. 70% de Q1 est suffisant pour passer à la suite.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/fin_bathy.png" alt="Fichier bathy.">
        <figcaption>Fichier pour visualiser la bathy. Ici l'interpolation a bien marché et on a pas de morceau qui sort.</figcaption>
    </figure>
</div>

On peut vérifier les métadonnées d'une image avec la commande :

```bash
exiftool -a -n -g /media/bioeos/D/202312_plancha_session/20231207_REU-HERMITAGE_ASV-1_02/PROCESSED_DATA/FRAMES/20231207_REU-HERMITAGE_ASV-1_02_1_1231.jpeg
```

et on obtient

```txt
---- ExifTool ----
ExifTool Version Number         : 12.76
---- File ----
File Name                       : 20231207_REU-HERMITAGE_ASV-1_02_1_1231.jpeg
Directory                       : /media/bioeos/D/202312_plancha_session/20231207_REU-HERMITAGE_ASV-1_02/PROCESSED_DATA/FRAMES
File Size                       : 1102133
File Modification Date/Time     : 2024:05:24 18:55:26+04:00
File Access Date/Time           : 2026:01:21 17:35:14+04:00
File Inode Change Date/Time     : 2024:05:24 19:04:29+04:00
File Permissions                : 100664
File Type                       : JPEG
File Type Extension             : JPG
MIME Type                       : image/jpeg
Exif Byte Order                 : MM
Comment                         : Lavc58.134.100
Image Width                     : 3840
Image Height                    : 2160
Encoding Process                : 0
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : 2 2
---- JFIF ----
JFIF Version                    : 1 2
Resolution Unit                 : 0
X Resolution                    : 72
Y Resolution                    : 1
---- EXIF ----
Image Width                     : 3840
Image Height                    : 2160
Bits Per Sample                 : 8Dernier point de l'interval à retirer
Camera Model Name               : HERO8 Black
X Resolution                    : 72
Y Resolution                    : 1
Resolution Unit                 : 1
Y Cb Cr Sub Sampling            : 2 2
Y Cb Cr Positioning             : 1
Exif Version                    : 0232
Date/Time Original              : 2023:12:07 03:52:57
Components Configuration        : 1 2 3 0
Exposure Compensation           : 0
Focal Length                    : 2.92
Sub Sec Time Original           : 297397000
Flashpix Version                : 0100
Color Space                     : 65535
Focal Length In 35mm Format     : 15
Sharpness                       : 1
Lens Serial Number              : C3341327809152
GPS Version ID                  : 2 3 0 0
GPS Latitude Ref                : S
GPS Latitude                    : 21.0872743786806
GPS Longitude Ref               : E
GPS Longitude                   : 55.2264079169917
GPS Altitude Ref                : 1
GPS Altitude                    : 0.69
Camera Serial Number            : C3331353137142
---- XMP ----
XMP Toolkit                     : Image::ExifTool 12.40
GPS Pitch                       : -1.09227367931115
GPS Roll                        : -2.76290844206535
GPS Track                       : 144.349900596421
Video Frame Rate                : 23.976
---- Composite ----
Image Size                      : 3840 2160
Megapixels                      : 8.2944
Scale Factor To 35 mm Equivalent: 5.13698630136986
Date/Time Original              : 2023:12:07 03:52:57.297397000
GPS Altitude                    : -0.69
GPS Latitude                    : -21.0872743786806
GPS Longitude                   : 55.2264079169917
Circle Of Confusion             : 0.00584900540241936
Field Of View                   : 100.388942610382
Focal Length                    : 15
GPS Position                    : -21.0872743786806 55.2264079169917
```


## Troubleshooting

### Problème GPS (Reach RS2/3)

Si les données de votre base RS3 sont de mauvaise qualité (GPSFix 5 ou absent), utilisez les stations du **RGP (IGN)**.

1. Identifiez la station la plus proche sur [rgp.ign.fr](https://rgp.ign.fr/).
2. Modifiez `ppk_config.json` :
```json
"rgp_station": "lepo",
"force_use_rgp": true

```

---

### Correction d'une trajectoire dérivante dans la bathymétrie

<div align="center">
    <figure>
        <img src="/img/plancha/ex_bathy_1.png" alt="Visualisation bathymétrique avec erreur de trajectoire.">
        <figcaption>Visualisation de la bathymétrie brute présentant un artefact de trajectoire.</figcaption>
    </figure>
</div>

Il arrive que le fichier `depth_samples_utm_coord_preproc.png` affiche une trajectoire sortant du transect d'étude. Ce phénomène survient généralement lors d'une mise en manuel de l'ASV ou d'une dérive lors de la mise à l'eau. Pour obtenir un raster final propre, il est nécessaire de filtrer ces données aberrantes.

**Méthode de filtrage :**

1. Ouvrez le fichier interactif `webmap_usv_track.html` situé dans le dossier `BATHY`.
2. Identifiez visuellement le segment à supprimer.
3. Cliquez sur les points correspondant au **début** et à la **fin** de la zone de dérive pour relever leurs valeurs respectives de `timeUS`.

Dans cet exemple, nous avons identifié les bornes suivantes : **641643950** et **700444139**.

<div align="center">
    <figure>
        <img src="/img/plancha/ex_bathy_2.png" alt="Sélection du point initial sur la carte.">
        <figcaption>Localisation du premier point du segment à exclure.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
    <img src="/img/plancha/ex_bathy_3.png" alt="Zoom sur les coordonnées temporelles.">
        <figcaption>Zoom sur la valeur timeUS du point initial.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/ex_bathy_4.png" alt="Sélection du point final sur la carte.">
        <figcaption>Localisation du dernier point de l'intervalle à retirer.</figcaption>
    </figure>
</div>

> **Astuce :** Pour plus de précision, vous pouvez augmenter le niveau de zoom maximal en modifiant directement les paramètres dans le code source du fichier `.html`.

**Application de la correction :**

Reportez l'intervalle temporel dans la colonne `filt_exclude_specific_timeUS` de votre fichier CSV, puis relancez le traitement :

```csv
session_name,time_first_frame,number_first_frame,filt_exclude_specific_timeUS,depth_range_max,depth_range_min,filt_exclude_specific_datetimeUTC,rgp_station
20231127_REU-HERMITAGE_ASV-2_03,2023:11:27 05:47:50.42,30,"[[641643950, 700444139]]",,,,

```

**Résultat après filtrage :**

<div align="center">
    <figure>
        <img src="/img/plancha/ex_bathy_5.png" alt="Visualisation bathymétrique corrigée.">
        <figcaption>Bathymétrie finale après exclusion du segment de dérive.</figcaption>
    </figure>
</div>

---

### Suppression des trajectoires indésirables dans les métadonnées (Session 20231213_REU-ETANG-SALE_ASV-2_01)

Une fois les deux étapes de traitement terminées, l'import du fichier `metadata.csv` dans QGIS peut parfois révéler des anomalies de trajectoire :

<div align="center">
    <figure>
        <img src="/img/plancha/ex_metadata_2.png" alt="Visualisation QGIS des métadonnées avec erreurs.">
        <figcaption>Visualisation des métadonnées montrant des segments dérivant vers la plage.</figcaption>
    </figure>
</div>

On observe ici des tracés qui s'étendent jusqu'au rivage. Ce problème survient généralement lorsque la session ne contient pas de fichier **.BIN** dans le dossier `SENSORS` : le programme ne parvient pas à détecter automatiquement les points de début et de fin de mission.

**Procédure de filtrage manuel :**

Pour nettoyer le jeu de données, nous devons définir manuellement les intervalles de temps à exclure. Dans ce cas précis, nous isolons deux périodes (le trajet aller vers la zone et le trajet retour après la mission).

<div align="center">
    <figure>
        <img src="/img/plancha/ex_metadata_3.png" alt="Identification du point de départ du transect.">
        <figcaption>Sélection du point limite avant le début effectif de l'acquisition.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/ex_metadata_4.png" alt="Identification du point de fin du transect.">
        <figcaption>Sélection du point limite après la fin de l'acquisition.</figcaption>
    </figure>
</div>

Les deux intervalles identifiés pour suppression sont :
`[['2023:12:13 03:22:04.0','2023:12:13 03:25:25.0'],['2023:12:13 04:07:03.0','2023:12:13 04:18:48.0']]`

> **Note :** Un filtrage légèrement large est préférable pour éliminer les accumulations d'images stagnantes sur un même point en bord de côte.

**Relance du traitement :**

Il est nécessaire de relancer l'intégralité de la pipeline (incluant l'extraction des images) en renseignant la colonne `filt_exclude_specific_datetimeUTC` dans votre CSV :

```csv
session_name,time_first_frame,number_first_frame,filt_exclude_specific_timeUS,depth_range_max,depth_range_min,filt_exclude_specific_datetimeUTC,rgp_station
20231213_REU-ETANG-SALE_ASV-2_01,2023:12:13 03:21:47.15,20,,,,"[['2023:12:13 03:22:04.0','2023:12:13 03:25:25.0'],['2023:12:13 04:07:03.0','2023:12:13 04:18:48.0']]",

```

**Résultat final :**

<div align="center">
    <figure>
        <img src="/img/plancha/ex_metadata_1.png" alt="Métadonnées nettoyées dans QGIS.">
        <figcaption>Jeu de données final, propre et prêt pour l'analyse.</figcaption>
    </figure>
</div>





## Application de l'IA (Inférence)

Le traitement se poursuit par l'application de deux modèles d'intelligence artificielle successifs pour trier et analyser les images.

### 1. Filtrage avec le modèle "Jacques"

La première étape utilise [Jacques](https://zenodo.org/records/8159227), un modèle basé sur l'architecture **ResNet**, conçu pour classer les images en deux catégories : **utiles** ou **inutiles**.

Le filtrage élimine les images de "grand bleu", ainsi que celles montrant le ciel ou la plage. Ce processus répond à deux objectifs :

* **Éthique et RGPD :** Garantir qu'aucun visage identifiable n'est publié sans consentement.
* **Optimisation :** Éviter de solliciter le second modèle, plus complexe, pour analyser des images sans intérêt biologique.

<div align="center">
    <figure>
        <img src="/img/plancha/utiles.png" alt="Exemples d'images utiles.">
        <figcaption>Exemples d'images considérées comme exploitables.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/inutiles.png" alt="Exemples d'images inutiles.">
        <figcaption>Exemples d'images à exclure (hors zone ou présence humaine).</figcaption>
    </figure>
</div>

### 2. Analyse biologique avec "DinoVdeau"

Le second modèle, [DinoVdeau](https://github.com/SeatizenDOI/DinoVdeau), est un modèle entraîné à partir de Dinov2. Il identifie simultanément plusieurs catégories sur une même image.

* **Données d'entraînement :** Disponible sur [HuggingFace](https://huggingface.co/datasets/lombardata/seatizen_atlas_image_dataset) et [Zenodo](https://doi.org/10.5281/zenodo.12819156).

<div align="center">
    <figure>
        <img src="/img/plancha/dinovdeau.png" alt="Architecture du modèle DinoVdeau">
        <figcaption>Architecture globale du modèle DinoVdeau.</figcaption>
    </figure>
</div>

---

### Exécution de l'inférence

Pour lancer l'analyse, utilisez le dépôt [plancha-inference](https://github.com/SeatizenDOI/plancha-inference). Une fois l'environnement configuré, exécutez la commande suivante :

```bash
python inference.py -efol -pfol /chemin/vers/votre/dossier -c

```

### Résultats obtenus

En fin de calcul, le script génère trois types de fichiers :

1. **Un fichier de prédictions :** Regroupe les scores de classification associés aux coordonnées GPS.
2. **Un raster interpolé :** Une carte de présence/absence générée à partir des prédictions (l'aspect régulier du transect est important pour obtenir une interpolation de qualité).
3. **Un rapport PDF :** Un document de synthèse récapitulant les statistiques de la session.

<div align="center">
    <figure>
        <img src="/img/plancha/fin_pdf.png" alt="Exemple de rapport PDF.">
        <figcaption>Synthèse des résultats au format PDF.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/fin_pred_score.png" alt="Visualisation des scores de prédiction">
        <figcaption>Distribution spatiale des scores (ex : présence d'Acropores branchus).</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/fin_pred_raster.png" alt="Raster de prédiction interpolé.">
        <figcaption>Raster final interpolé représentant la densité de l'espèce ciblée.</figcaption>
    </figure>
</div>


## Génération d'orthophotos

La création d'orthophotos est une tâche gourmande en ressources. Pour cette raison, l'utilisation du cluster **Datarmor** est indispensable.

Un script spécifique permet d'automatiser ce processus via **OpenDroneMap (ODM)** :

👉 **[Script ODM Datarmor (asv_light.pbs)](https://github.com/SeatizenDOI/datarmor-pbs/blob/master/odm/asv/asv_light.pbs)**

**Ce script assure les fonctions suivantes :**

* Configuration automatique de l'environnement logiciel sur Datarmor.
* Lancement d'OpenDroneMap avec des paramètres optimisés pour les données ASV.


## Publication et synchronisation des données

### 1. Archivage sur Zenodo et mise à jour de l'Atlas

Une fois les données traitées, la session doit être mise en ligne pour garantir sa pérennité. Veuillez vous référer aux guides de la section [Outils pour Zenodo](../zenodo/zenodo-tools.md) pour :

* Téléverser vos sessions sur **Zenodo**.
* Synchroniser les métadonnées avec la base de données **Seatizen Atlas**.

### 2. Hébergement des rasters sur le serveur COG

La dernière étape consiste à rendre les couches cartographiques accessibles via le serveur **COG (Cloud Optimized GeoTIFF)**.

Vous devrez rassembler les rasters suivants :

* **Bathymétrie** corrigée.
* **Prédictions IA** (cartes de scores).
* **Orthophotos** générées sur Datarmor.

Toutes les instructions de déploiement sont détaillées sur le dépôt [cog-server](https://github.com/SeatizenDOI/cog-server) ou directement dans l'onglet [Ajouter un raster sur cog-server](../tutoriels/cog-server.md) de cette documentation.
