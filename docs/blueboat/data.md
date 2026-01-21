# Récupération des données de pompage

Une fois la mission terminée, les données peuvent être extraites de deux manières complémentaires : via le stockage physique de la pompe ou via l'interface réseau du Blueboat.

## 1. Extraction par clé USB (Méthode physique)

C'est la méthode la plus simple pour récupérer les sessions de pompage brutes :

1. Allumez la pompe.
2. Branchez une clé USB sur le port dédié de l'unité de pompage.
3. Le système lance automatiquement la copie de l'historique des sessions sur la clé.

Une fois l'opération terminée, vous pouvez ouvrir les fichiers pour extraire les indicateurs clés :

* **Volume total pompé** (Litre)
* **Heure de début** (Horodatage)
* **Durée du pompage** (Secondes/Minutes)

## 2. Extraction via l'interface réseau (Fichiers logs)

Vous pouvez accéder aux logs directement depuis le système embarqué.

### Localisation des fichiers

Les logs de l'application **eDNA Controller** sont stockés dans le système de fichiers de la carte Navigator.
Le chemin d'accès dans le terminal est généralement : `/home/pi/logs/edna/`. <!-- Mettre le nom du dossier avec un screen. -->

### Récupération via FileZilla (SFTP)

Pour télécharger les fichiers facilement sur votre ordinateur :

1. Connectez votre PC au réseau du Blueboat (Wi-Fi BaseStation ou câble).
2. Ouvrez **FileZilla** et configurez une nouvelle connexion :
* **Hôte :** `sftp://192.168.2.2`
* **Identifiant :** `pi`
* **Mot de passe :** `blueos` (ou mot de passe configuré)
* **Port :** `22`


3. Naviguez jusqu'au dossier des logs et glissez-déposez les fichiers sur votre ordinateur.

## 3. Consolidation des données

Lors de la mise en relation des fichiers logs avec les données relevées manuellement, une règle de priorité s'applique :

> ⚠️ **Important :** Si vous observez une différence entre le volume affiché sur l'écran physique de la pompe et celui rapporté par l'interface web (fréquent en cas de débit très faible), **utilisez toujours la valeur affichée sur la pompe** comme référence pour vos analyses scientifiques.
