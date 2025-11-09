# Après une session

## Déchargement des données brutes

Créer un dossier `tmp` pour temporairement stocker les fichiers brutes. Le fichier du reach RS3 est à la racine et il faut créer un dossier par planche.


### GPS Base - Reach RS3

Dans un premier temps, on récupère les données sur le reach RS2/3. On l'allume sans partage de connexion et au bout d'un moment, il va se mettre lui en hotspot. On se connecte à son réseau, le code est `emlidreach`. Une fois connecté, on se rend à l'url `192.168.42.1`. On va dans le dossier `logging` et on télecharge le fichier zip du jour. Sur d'ancien firmwares, ce n'est pas un fichier zip mais plusieurs fichiers différents. Il faut télécharger le fichier LLH et RINEX.

### GPS Device - Reach M2

Pour le GPS de la planche, il faut allumer la planche. Au bout d'une minute ou deux, la planche passe en hotspot et on peut se connecter à son réseau. Le mot de passe est `emlidreach`. Une fois connecté, on se rend sur l'URL `192.168.42.1` et on va dans l'onglet logging. Sur les nouveaux firmwares, il y a 1 fichier zip par session. Sur les anciens firmwares, il faut télécharger les fichiers LLH et rinex. Téléchargez les fichiers dans le dossier de planche tmp associé

### Fichier de l'autopilote

Dans la planche, il y a un petit cube orange qui est l'autopilot. Celui-ci génère un fichier par mission, si on éteint bien la planche entre chaque mission. Pour cela, il faut retirer la carte micro SD et la brancher sur le PC. Ensuite, il faut aller dans le dossier `APM/LOG`. Celui-ci contient des fichiers .BIN et nommé selon des numéros.
Ils sont dans l'ordre chronologiques des sessions. On peut vérifier l'ordre chronologique grâce au script `utils/verify_bin.py` dans le dépôt github [plancha-workflow](https://github.com/SeatizenDOI/plancha-workflow). <!-- TODO IMAGE -->
On déplace les fichiers BIN dans le dossier planche temporaire puis on supprime les fichiers sur la carte SD ainsi que le fichier .txt de 4 octect.

### Données vidéos

On récupère les données sur la carte SD de la gopro et on déplace les données dans le fichier temporaire. On peut trier les vidéos par l'heure sur la première vidéo.

Pour différencier les sessions parmi les vidéos, il faut regarder les deux derniers chiffres du nom du fichier. il ne change pas pour une même vidéo. <!-- TODO IMAGE -->


### Rassembler les données brutes


Pour rassembler les données d'une session, il faut créer un dossier avec la nomenclature suivante : YYYYMMDD_country-code-optinal-place_device_session-number.
Par exemple pour les deux planches et sur 2 sessions chacune à saint Leu :
* 20250411_REU-ST-LEU_ASV-1_01
* 20250411_REU-ST-LEU_ASV-1_02
* 20250411_REU-ST-LEU_ASV-2_01
* 20250411_REU-ST-LEU_ASV-2_02


Chaque session doit avoir les dossiers suivant

.20250411_REU-ST-LEU_ASV-1_01
    DCIM: Vidéos des gopros
    GPS
        BASE: Données GPS de la base.
        DEVICE: Données GPS de la session.
    SENSORS: .BIN de la session.


A ce niveau là, on a rassemblé les donnés brutes au bonne endroit et elles sont prêtes à être exploité.

/!\ Il est nécessaire de noter dans un fichier excel les sessions réalisé pour faire un suivi. /!\


/!\ Dans le doute d'une mauvaise maipulation, il faut toujours copier puis supprimer les données plutôt que de déplacer les données. /!\

## Traitement des données pour synchroniser les images et les données temporelles

En tant que tel, les données ne sont pas exploitables car les vidéos n'ont pas de GPS associés, la bathymétrie n'est pas corrigée et le GPS n'est pas en PPK.
Pour réaliser cela, on utilise le dépôt [plancha-workflow](https://github.com/SeatizenDOI/plancha-workflow). Le README explique comment mettre en place l'environnement.

### Batch de sessions

Pour traiter plusieurs sessions d'un coup (même juste une ligne ça marche), on peut utiliser l'option -csv. Pour cela, il faut rassembler toutes les sessions dans un csv comme par exemple

```csv
session_name,time_first_frame,number_first_frame,filt_exclude_specific_timeUS,depth_range_max,depth_range_min,filt_exclude_specific_datetimeUTC,rgp_station
20240607_REU-HERMITAGE_ASV-1_01,2024:06:07 03:07:19.60,20,,20,,,
20240607_REU-HERMITAGE_ASV-1_02,2024:06:07 05:00:06.81,46,,,,,
20240708_REU-POINTE-DES-AIGRETTES_ASV-1_01,1970:01:01 00:00:00.0,0,"[[2574040496,2636840631]]",,3,,
20250414_REU-TROU-DEAU_ASV-1_03,2025:04:14 04:29:56.00,10,"[[201262382,219662452]]",,,,
20250519_REU-ST-LEU_ASV-1_02,2025:05:19 05:24:52.11,90,,,,"[['2025:05:19 05:24:51.0','2025:05:19 05:27:28.0'],['2025:05:19 06:05:45.0','2025:05:19 06:06:57.0']]",
```

/!\ Il est mieux d'utiliser un fichier csv car on peut garder une trace des paramètres utilisés. /!\



### Première étape.

Dans le fichier csv, on doit renseigner l'heure de la première image (`time_first_frame`) et sur quelle image on l'a vu (`number_first_frame`). Si la session n'a pas de vidéos, on met 1970:01:01 00:00:00.0 et 0. 

Pour obtenir l'heure et le numéro de l'image, il faut exécuter le script avec cette ligne de commande: `python workflow.py -csv csv/2025_plancha.csv -os`. Cela va juste découper la première vidéo en image, réaliser le PPK et la Bathy. Une fois que le script a tourné, on va dans le dossier PROCESSED_DATA/FRAMES et on cherche la frame qui contient l'heure, on la renseigne dans le fichier csv avec le numéro de la frame. 
<!-- TODO IMAGE -->

On regarde si le GPS a bien marché en regardant le fichier .png dans le dossier 

### DEuxième étape


### Troubleshooting GPS

### Troubleshooting Bathy

Valeurs aberrantes min/max

Traits en dehors de limites 


### Troubleshooting Metadate

Pour une session d'asv, il est nécessaire d'avoir un rectangle relativement parfait pour réaliser des rasters par la suite. Pour cela, on charge le fichier metadata.csv dans QGIS. Si on observe un trait comme ceci : <!-- TODO IMAGE --> on récupère les valeurs aux extrémités de l'intervalle pour supprimer cette portion en reseignant cette valeur dans le fichier CSV. <!-- TODO IMAGE -->




## Traitement de la


## Application de l'ia

## Mise en ligne sur Zenodo.

## Synchronisation sur SeatizenMonitoring