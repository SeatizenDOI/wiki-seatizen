# Blueboat

Ce tutoriel explique la procédure complète de mise en marche du catamaran Blue Boat pour une opération de pompage, incluant la gestion des batteries et le dépannage de l'interface de contrôle.

## I. Gérer le Blue Boat

### A. Installation des Batteries

L'installation des batteries est la première étape du démarrage.

* **Nombre de batteries :** Il faut **quatre batteries** pour alimenter le Blue Boat.
* **Tension requise :** Les quatre batteries doivent être chargées avec une différence de tension maximale de 0.1V.
* **Connexions :** Une fois les batteries installées, il faut rebrancher correctement les deux connexions à la boîte interne :
* **Connexion à 2 points :** Amène l'alimentation 12V à la boîte.
* **Connexion à 4 points :** Établit la connexion **USB** entre la carte Navigator (le cerveau du Blue Boat) et l'Arduino (qui gère les boutons de la pompe).



### B. Lancement et Vérification du Système

1. **Mise sous Tension :** Tourner le petit bouton de démarrage jusqu'à la position maximale (cela écrase les contacts et lance le système).
2. **Vérification :** Le Blue Boat a démarré lorsque la LED blanche clignote.

### C. Connexion du Logiciel QGroundControl

Pour la navigation du Blue Boat (si nécessaire), la documentation officielle est la suivante :

* [https://bluerobotics.com/learn/blueboat-software-setup/](https://bluerobotics.com/learn/blueboat-software-setup/)

> Pour que le logiciel **QGroundControl** fonctionne correctement, des prérequis de configuration spécifiques (non détaillés ici) doivent être respectés.

## II. Procédure de Pompage (Matériel et Préparation)

**(Cette section reprend la préparation du matériel et la configuration IP du PC détaillées précédemment.)**

### ⚠️ Précautions de Sécurité

* **Ne jamais démarrer le Blue Boat ou la Base Station sans son antenne vissée.** Cela peut endommager le matériel définitivement.
* **Configuration IP :** Remettre la configuration IPv4 du PC en mode automatique après l'opération pour pouvoir se connecter à Internet.

## III. Lancement de l'Application qui Gère la Pompe (eDNA Controller)

### A. Dépannage de l'Application

À l'ouverture de la page web (`192.168.2.2`), l'application **eDNA Controller** peut ne pas fonctionner immédiatement.

* **Cause probable :** L'initialisation de la connexion entre l'Arduino et la carte Navigator ne s'est pas faite correctement.
* **Vérification :** On sait que l'application marche **quand on voit les courbes évoluer**.

### B. Relance Forcée de l'Application

Pour relancer l'application, vous devez passer en **mode pirate** (`pirate mode` ou mode Terminal) :

1. Accédez à l'application **Terminal** sur l'interface Blue Boat.
2. Dans le terminal (qui est souvent en mode émulateur), tapez la commande suivante pour accéder au terminal réel : `red-pill`


3. Une fois dans le terminal de la Navigator, exécutez le script de relance : `./reload_docker.sh`.

4. L'application eDNA Controller va **disparaître et réapparaître** dans la barre des tâches.

## IV. Fonctionnement de l'Interface et Amorçage

### A. Fonctionnement de l'Interface

L'interface de contrôle présente un bouton général "Lancer la pompe", mais aussi des contrôles plus fins :

* **Boutons fins :** Les **petits boutons ronds** à côté des graphiques correspondent aux boutons physiques sur la pompe.
* **Conseil :** Il est plus intéressant de cliquer sur ces petits ronds car ils effectuent une **action directe** sur la pompe, contrairement au bouton général qui introduit un léger délai pour exécuter plusieurs commandes de sécurité.
* Passez votre souris en survol pour voir à quoi correspondent ces petits ronds.



### B. Amorçage et Surveillance

1. **Amorçage :** Il est conseillé d'amorcer la pompe en la faisant pomper dans l'eau **sans les filtres** pour garantir un bon écoulement.
2. **Vérification de l'amorçage :** Surveillez l'évolution de la pression et du débit :

| État du Processus | Pression | Débit | Diagnostic |
| --- | --- | --- | --- |
| **Début** | Monte | Baisse | La pompe expulse l'air. |
| **Pompe Amorcée** | **Descend** | **Se stabilise** | Le flux est continu. |
| **Problème** | Reste **Haute** | Ne **Monte pas** | La pompe n'est pas amorcée (elle pompe de l'air). |

### C. Ajustement du Débit Faible

En cas de débit continu très faible (ex. 0.3L), l'affichage sur l'ordinateur sera incorrect par rapport au volume réel filtré.

* **Ajustement Suggéré :** Si le débit est de 0.3L/min arrêtez le pompage à environ 16.4L affiché sur l'ordinateur pour atteindre environ 18L (le volume cible) sur la pompe elle-même.