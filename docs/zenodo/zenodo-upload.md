# Préparation et Envoi avec `zenodo-upload.py`

Le script `zenodo-upload.py` du dépôt **Zenodo Tools** est conçu pour automatiser l'envoi d'une session Seatizen locale vers Zenodo. L'opération nécessite la préparation de plusieurs fichiers de métadonnées.

Zenodo Upload est disponible sur le dépôt GitHub suivant : [https://github.com/SeatizenDOI/zenodo-tools](https://github.com/SeatizenDOI/zenodo-tools)

### Étape 1 : Fichiers Locaux Requis

Pour commencer l'envoi, la session doit être présente **en local** et vous devez préparer deux fichiers CSV pour le suivi des métadonnées et des contributeurs.

#### 1. Fichier de Suivi de Session (`suivi_session.csv`)

Ce fichier CSV sert à définir les informations de base de chaque session à envoyer.

Tous les champs que zenodo accepte sont ici https://developers.zenodo.org/#representation au niveau de `contributors`. 

```csv
session_name,Creators,DataManager,DataCollector,ProjectMember
20150611_REU-St-Gilles_scuba,"PM, TB, MR, LM",VI,,,
20150615_REU-La-Saline_scuba,"PM, TB, MR, LM",VI,,,
20150616_REU-La-Saline_scuba,"PM, TB, MR, LM",VI,,,
20150617_REU-St-Leu_scuba   ,"PM, TB, MR, LM",VI,,,
20150619_REU-St-Gilles_scuba,"PM, TB, MR, LM",VI,,,
```


**Exemple de structure [suivi_session.csv](https://raw.githubusercontent.com/SeatizenDOI/zenodo-tools/refs/heads/master/metadata/contributors/suivi_session.csv)** 

#### 2. Fichier des Contributeurs (`contributors.csv`)

Ce fichier associe les initiales (utilisées dans le Fichier de Suivi) aux noms complets et aux affiliés des personnes. 

```csv
id,name,affiliation,orcid
AB,Alexandre Boyer,"UMR Marbec, IRD, La Réunion", France,0009-0002-2787-1044
AJ,Alexis Joly,"INRIA Zenith, Montpellier, France",0000-0002-2161-9940
AG,Andrea Goharzadeh,"Ifremer DOI, La Réunion, France",
```


**Exemple de structure [contributors.csv](https://raw.githubusercontent.com/SeatizenDOI/zenodo-tools/refs/heads/master/metadata/contributors/contributors.csv)** 


### Étape 2 : Lier les Données via `metadata.json`

Les deux fichiers CSV (Suivi de Session et Contributeurs) doivent être liés ensemble via le fichier de configuration principal du script : **`metadata.json`**.

Ce fichier JSON est l'endroit où vous spécifiez les chemins d'accès à vos fichiers CSV et d'autres paramètres pour le processus d'upload.

**Exemple de structure [metadata.json](https://raw.githubusercontent.com/SeatizenDOI/zenodo-tools/refs/heads/master/metadata/metadata.json)** 


### Étape 3 : Logique Interne du Script

Le code de `zenodo-upload.py` est conçu pour automatiser certaines déterminations de métadonnées basées sur le contenu de la session :

* **Détermination de la Plateforme d'Acquisition :** Le script analyse automatiquement le type de plateforme utilisée (par exemple, UVC, ASV, UVC) depuis le nom de la session.
* **Note Spéciale UVC :** Les sessions UVC sont souvent traitées via une classe spéciale, notamment pour les sessions d'Amoros.
* **Note Spéciale Scuba :** L'implémentation pour Scuba peut être intégrée différemment ou dans une logique spécifique, potentiellement classée avec UVC.


* **Métadonnée ISO 19115 :**
* **Attention :** La génération de la métadonnée **ISO 19115** est actuellement configurée pour être **spécifique aux exigences d'Ifremer**.
* Si d'autres utilisateurs ou institutions souhaitent utiliser ce script, il faudra probablement **modifier les noms** ou la structure utilisée pour cette norme afin de s'adapter à leurs propres standards.