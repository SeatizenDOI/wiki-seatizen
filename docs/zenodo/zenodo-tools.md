# 🛠️ Présentation de Zenodo Tools

Ce tutoriel porte sur **Zenodo Tools**, un ensemble de scripts conçus pour gérer les données de session Seatizen (ou similaires) avec la plateforme Zenodo. Ces scripts offrent différentes fonctionnalités, notamment l'envoi, le téléchargement, la gestion et la mise à jour des données.

Zenodo Tools est disponible sur le dépôt GitHub suivant : [https://github.com/SeatizenDOI/zenodo-tools](https://github.com/SeatizenDOI/zenodo-tools)

### Les scripts Zenodo Tools et leurs fonctions

| Script | Description de la fonction | Détails importants |
| --- | --- | --- |
| **Zenodo Upload** | Permet d'**envoyer une session** en ligne sur Zenodo. | Ce script récupère les données **brutes** et **processées** locales, crée différentes versions, ainsi que les descriptions et les métadonnées associées. |
| **Zenodo Download** | Permet de **récupérer les données** d'une session depuis Zenodo. | Le téléchargement se fait en fonction du **nom de la session** ou du **DOI**. Il peut récupérer les données processées ou les données brutes. |
| **Zenodo Manager** | Permet de **gérer la base de données Seatizen Atlas**. | * Crée la base de données Seatizen Atlas. |
|  |  | * Peut chercher le dernier jeu de données package en ligne ou en créer un nouveau. |
|  |  | * Dans l'usage quotidien, il permet principalement d'**ajouter une session dans la base de données**. |
|  |  | **Pré-requis pour l'ajout :** La session doit être présente **en local** et contenir au moins le fichier **metadata** et le fichier **IA**. |
|  |  | * Permet également d'ajouter des **annotations** (je vous laisse explorer cette fonctionnalité). |
| **Zenodo auto-update** | Permet de **mettre à jour automatiquement** la base de données. | Contrairement à Zenodo Manager, il n'y a **pas besoin d'avoir les fichiers en local**. Il va télécharger les fichiers depuis Zenodo pour ensuite les mettre à jour. C'est un processus un peu plus long. |
| **Zenodo Monitoring** | Ancienne application Dash. | Utilisée auparavant pour l'interface web. Elle n'est **plus utilisée** mais n'est pas encore supprimée du dépôt. |