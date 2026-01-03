# Gérer le blueboat.


## Instqllqtion des batteries

Il faut 4 batteries chargés avec une différence de tension de maximum 0.1V.

Une fois installé, il faut bien rebranché les deux connexions à la boite. Celui qui a 2 points ammène le 12V dans la boite et celui qui a 4 points est une connexion USB entre la navigator et l'arduino à l'intérieur.




## 

Lien vers la doc : https://bluerobotics.com/learn/blueboat-software-setup/


pour que QGroundControl marche, il faut que 



## Lancement de l'application qui gère la pompe

A l'ouverture de la page la première fois, il se peut que l'appli ne marche pas. C'est parce que l'initialisation de la connexion entre l'arduino et la navigator ne s'est pas faite.

Pour relancer l'appli, il faut être en pirae mode pour accéder à l'application terminal.

Dans l'application terminal, il faut taper la commande `red-pill` pour sortir du mode emulateur. Là, on exécute le script en exécutant le script `./reload_docker.sh`.

L'application va disparaître et réapparaître dans la barre des tâches :

/!\ On sait que l'application marche quand on voit les courbes évoluer. /!\


## Fonctionnement de l'interface

Il y a le bouton 