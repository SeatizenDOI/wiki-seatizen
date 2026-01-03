# Traitement d’une session drone

Ce tutoriel décrit le **workflow complet de traitement d’une session drone**, depuis la récupération des images brutes jusqu’à la préparation finale des données pour un dépôt sur **Zenodo**, en s’appuyant sur les outils développés dans le cadre du projet **Seatizen**.
---

## 1. Récupération des données du drone

La première étape consiste à **extraire les images depuis la carte mémoire du drone**.

⚠️ **Contraintes importantes :**

* Les **vidéos ne sont pas utilisables** dans ce workflow.
* Les vidéos ne permettent pas de récupérer des métadonnées GPS exploitables.
* Il est indispensable de travailler avec **des images contenant des métadonnées GPS (EXIF)**.

---

## 2. Organisation des images (dossier `DCIM`)

Toutes les images doivent être regroupées dans un dossier nommé :

```
DCIM/
```

### Points de vigilance

* Certains drones répartissent les images dans **plusieurs sous-dossiers**.
* Il peut exister **des images avec le même nom**.
* Si toutes les images sont copiées dans un seul dossier sans renommage, **elles risquent de s’écraser**.

👉 Avant de poursuivre :

* Vérifiez les doublons,
* Renommez les fichiers si nécessaire.

---

## 3. Production de l’orthophoto avec OpenDroneMap (ODM)

L’orthophoto est un **élément central du workflow**, notamment pour les étapes d’intelligence artificielle.

Nous recommandons donc d’exécuter **OpenDroneMap (ODM)** en premier.

### Paramètres ODM utilisés

Voici les paramètres OpenDroneMap utilisés dans notre workflow :

```bash
--project-path /datasets/code/PROCESSED_DATA/PHOTOGRAMMETRY \
--auto-boundary \
--cog \
--orthophoto-resolution 1.0 \
--rolling-shutter \
--use-exif \
--max-concurrency 128 \
--fast-orthophoto \
--optimize-disk-space \
--feature-quality ultra
```

Ces paramètres permettent notamment :

* l’utilisation des métadonnées EXIF,
* la génération d’une **orthophoto optimisée (COG)**,
* une résolution de 1 m,
* une optimisation des performances et de l’espace disque,
* une qualité élevée pour la détection des points caractéristiques.

---

## 4. Exécution d’ODM sur Datarmor (script PBS)

Dans notre cas d’usage, les calculs sont effectués sur le **cluster Datarmor**.

### Transfert des données

* Les données sont envoyées sur Datarmor via **FileZilla** (ou équivalent).
* Une fois les données disponibles sur le cluster, ODM est lancé via un **script PBS**.

### Script PBS ODM

Le script PBS utilisé est disponible ici :

👉 **Script ODM Datarmor**
[https://github.com/SeatizenDOI/datarmor-pbs/blob/master/odm/drone/drone_serge_may2025.pbs](https://github.com/SeatizenDOI/datarmor-pbs/blob/master/odm/drone/drone_serge_may2025.pbs)

Ce script :

* configure l’environnement Datarmor,
* lance OpenDroneMap avec les paramètres adaptés,
* gère l’exécution sur les nœuds de calcul du cluster.

---

## 5. Workflow d’extraction des métadonnées

Une fois l’orthophoto générée, nous appliquons le workflow **drone-workflow** afin d’extraire et structurer les métadonnées des images.

### Commande Datarmor

```bash
echo "Working with ${SESSION_PATH}"

singularity run \
  --pwd /home/seatizen/app/ \
  --bind ${PATH_DRONE_FOLDER}:/home/seatizen/sessions \
  drone_workflow.sif \
  -efol \
  -pfol /home/seatizen/sessions \
  -c \
  -ip ${PBS_ARRAY_INDEX}
```

Ce workflow permet de :

* lire les images et leurs métadonnées GPS,
* structurer les informations de session,
* préparer les fichiers nécessaires aux étapes suivantes.

---

## 6. Inférence automatique sur les images

Une première étape d’inférence est ensuite réalisée à l’aide du conteneur **drone-inference**.

### Commande Datarmor

```bash
singularity run --nv \
  --pwd /home/seatizen/app/ \
  --bind /home1/datawork/villien/models:/home/seatizen/app/models \
  --bind ${PATH_DRONE_FOLDER}:/home/seatizen/sessions \
  --bind /home1/scratch/villien/:/tmp/ \
  drone_inference.sif \
  -efol \
  -pfol /home/seatizen/sessions \
  -c \
  -ip ${PBS_ARRAY_INDEX}
```

Cette étape permet :

* d’exploiter les modèles pré-entraînés,
* d’appliquer une première analyse automatique sur les images,
* de produire des sorties intermédiaires pour la segmentation finale.

---

## 7. Segmentation avec *The Point Is The Mask*

La segmentation fine est réalisée avec l’outil **The Point Is The Mask**, basé sur une intelligence artificielle de segmentation.

### Activation de l’environnement Conda

```bash
cd /home1/datahome/villien/project_hub/the-point-is-the-mask

. /appli/anaconda/latest/etc/profile.d/conda.sh
conda activate /home1/datawork/villien/conda-env/segment_env
```

### Lancement de l’inférence de segmentation

```bash
python3 inference.py \
  -eses \
  -pses ${SESSION_PATH} \
  -po /home1/scratch/villien/drone_uav_tmp \
  -psm ./models/SegForCoral-b2-2025_06_30_19127-bs16_refine \
  --is_seatizen_session
```

Cette étape :

* segmente l’orthophoto,
* réalise des prédictions sur **5 classes**,
* génère des masques exploitables pour l’analyse et l’annotation.

---

## 8. Dépôts GitHub à consulter

Pour comprendre et adapter chaque étape du workflow, nous vous invitons à consulter les dépôts suivants :

* **Drone workflow (métadonnées)**
  [https://github.com/SeatizenDOI/drone-workflow](https://github.com/SeatizenDOI/drone-workflow)

* **Drone inference**
  [https://github.com/SeatizenDOI/drone-inference](https://github.com/SeatizenDOI/drone-inference)

* **The Point Is The Mask (segmentation)**
  [https://github.com/SeatizenDOI/the-point-is-the-mask/](https://github.com/SeatizenDOI/the-point-is-the-mask/)

---

## 9. Préparation finale pour Zenodo

Une fois toutes les étapes terminées :

* orthophoto produite,
* métadonnées structurées,
* segmentation réalisée,

👉 la **session drone est complète** et **prête à être déposée sur Zenodo**.