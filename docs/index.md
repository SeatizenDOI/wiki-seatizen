# Wiki de l'environnement Seatizen

Vous retrouverez sur ces pages ce qu'il pour commprendre :

* Comment déployer les planches
* Mettre en place le blueboat
* Faire une acquisition de GCP
* 


## Comprendre la structure d'un programme

Pour bien exploiter un outil ou un tutoriel, il est essentiel de comprendre comment interagir avec le code. La première étape consiste à identifier les **arguments d'entrée**, qui définissent le comportement du programme.

### Identifier le point d'entrée

Dans un dépôt GitHub, le script principal (celui que l'on exécute) se nomme généralement `main.py`, `inference.py` ou encore `workflow.py`. C'est là que se trouve la logique globale du projet.

### Analyser la fonction `parse_args`

La plupart des scripts Python utilisent une fonction nommée `parse_args` (souvent basée sur la bibliothèque `argparse`). Elle sert de "menu" en listant tous les paramètres que l'utilisateur peut modifier en ligne de commande.

Prenons l'exemple du script `zenodo-auto-update.py` issu du dépôt [zenodo-tools](https://github.com/SeatizenDOI/zenodo-tools) :

```python
def parse_args():
    parser = argparse.ArgumentParser(
        prog="zenodo-auto-update", 
        description="Récupère les sessions en ligne pour alimenter le Seatizen Atlas."
    )

    # Communautés à parcourir
    parser.add_argument("-fc", "--fetch_communities", default=["seatizen-data"], 
                        help="Liste des communautés Zenodo à interroger.")

    # Chemins de stockage et métadonnées
    parser.add_argument("-psa", "--path_seatizen_atlas_folder", default="./seatizen_atlas_folder", 
                        help="Dossier de stockage des données.")
    parser.add_argument("-pmj", "--path_metadata_json", default="./metadata/metadata_seatizen_atlas.json", 
                        help="Chemin vers le fichier de métadonnées JSON.")

    return parser.parse_args()

```

### Ce qu'il faut retenir

En lisant ce bloc de code, on comprend immédiatement les besoins du programme :

1. **La source** : Le nom de la communauté Zenodo à cibler (`--fetch_communities`).
2. **La destination** : L'emplacement où les données seront téléchargées (`--path_seatizen_atlas_folder`).
3. **La configuration** : Le fichier JSON contenant les métadonnées de référence (`--path_metadata_json`).

Chaque argument possède une valeur par défaut (`default`), ce qui permet de lancer le programme rapidement, tout en gardant la flexibilité de personnaliser les chemins selon vos besoins.


## Liste des environnements conda présent sur le pc

Pour lister les environnements conda, on utilise la commande `conda env list`:

```bash
# conda environments:
#
base                  *  /home/bioeos/miniconda3                                    # Environnement de base, ne rien télécharger ici.
aina_env                 /home/bioeos/miniconda3/envs/aina_env                      # Environnement pour le dépôt https://github.com/SeatizenDOI/cpce-workflow
cog_server_env           /home/bioeos/miniconda3/envs/cog_server_env                # Environnement pour le dépôt https://github.com/SeatizenDOI/cog-server
ctoi_env                 /home/bioeos/miniconda3/envs/ctoi_env                      # Environnement pour le dossier /home/bioeos/Documents/project_hub/worldfish
dinovdeau_env            /home/bioeos/miniconda3/envs/dinovdeau_env                 # Environnement pour le dépôt https://github.com/SeatizenDOI/dinovdeau
drone_inference_env      /home/bioeos/miniconda3/envs/drone_inference_env           # Environnement pour le dépôt https://github.com/SeatizenDOI/drone-inference
drone_upscaling_env      /home/bioeos/miniconda3/envs/drone_upscaling_env           # Environnement pour le dépôt https://github.com/SeatizenDOI/drone-upscaling
drone_workflow_env       /home/bioeos/miniconda3/envs/drone_workflow_env            # Environnement pour le dépôt https://github.com/SeatizenDOI/drone-workflow
ign_upscaling_env        /home/bioeos/miniconda3/envs/ign_upscaling_env             # Environnement pour le dépôt https://github.com/SeatizenDOI/ign-upscaling
inference_env            /home/bioeos/miniconda3/envs/inference_env                 # Environnement pour le dépôt https://github.com/SeatizenDOI/plancha-inference
kevine_yolo_env          /home/bioeos/miniconda3/envs/kevine_yolo_env               # Environnement pour les codes de kevine dans /home/bioeos/Documents/project_hub/miscellanous/kevine_yolo
plancha_env              /home/bioeos/miniconda3/envs/plancha_env                   # Environnement pour le dépôt https://github.com/SeatizenDOI/plancha-workflow
recif3D_env              /home/bioeos/miniconda3/envs/recif3D_env                   # Environnement pour le dépôt https://github.com/SeatizenDOI/recif3d-workflow
sam3                     /home/bioeos/miniconda3/envs/sam3                          # Environnement de test pour le dépôt Sam3
seatizen_monitoring_env     /home/bioeos/miniconda3/envs/seatizen_monitoring_env    # Environnement pour le dépôt https://github.com/SeatizenDOI/seatizen-monitoring pour la partie backend
segment_env              /home/bioeos/miniconda3/envs/segment_env                   # Environnement pour le dépôt https://github.com/SeatizenDOI/the-point-is-the-mask
test_env                 /home/bioeos/miniconda3/envs/test_env                      # Environnement de test avec python 3.12
wiki_env                 /home/bioeos/miniconda3/envs/wiki_env                      # Environnement pour le dépôt https://github.com/SeatizenDOI/wiki-seatizen
zenodo_env               /home/bioeos/miniconda3/envs/zenodo_env                    # Environnement pour le dépôt https://github.com/SeatizenDOI/zenodo-tools
```

On active un environnement avec la commande suivante : `conda activate zenodo_env`