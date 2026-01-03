# Préparation d'une session d'acquistion

## Conditions météo et heure d'acquisition

Une session d'acquisition se réalise le matin, idéalement au levé du soleil pour éviter les caustics au fond de l'eau.


## Préparation du matériel d'acquisition pour une session

- Ordinateur de terrain avec Mission Planner
- Le boitier de télemétrie pour réaliser la communication 
- La manette pour piloter la planche
- 2 Batteries 4S 10A 1C (Peux faire deux sessions)
- 1 Gopro - Batteries (1 par sessions) - caisson étanche + outils pour accrocher le caisson
- Un reach RS2/3 (bouboule) qui ferra office de Base GPS avec son trépied et un téléphone pour faire un partage de connexion.
<!-- TODO Image -->

Les gopros doivent être configuré avec cette configuration :
<!-- TODO Image -->
4K - 24 FPS - 

## Procédure sur la plage

### Préparation du REACH RS2/3

Pour plus d'information, [voir le tutoriel d'emlid](https://docs.emlid.com/reachrs3/)

* Déployer le trepied dans un espace dégagé d'arbres et de batiments.
* Allumer le partage de connexion sur son téléphone
* Allumer le reach et attendre que les leds deviennent bleus.
* Vérifier que la base en FIX dans l'application emlidflow. Il faut le paramétrer pour qu'il se connecte à une base Centipède. https://docs.centipede.fr/docs/centipede/3_connect_caster.html

<!-- TODO Image -->

### Préparation Planche


* Vérifier les tensions des batteries au multimètre et verifier que la tension n'a pas plus de 0.1V de différence. Si la différence est supérieur, il ne faut pas les utiliser sous peine de les endommager.
* On met les batteries dans la planche et on l'allume. /!\ Il faut ouvrir le caisson de la planche doucement, car l'appel d'air peut faire entrer des gouttes d'eau dedans.
* On met une batterie dans la gopro et mettre la gopro dans le caisson. /!\ Il faut nettoyer le joint du caisson s'il y a des grains de sables dessus.
* Juste avant de mettre la planche à l'eau, il faut allumer la gopro et filmer l'heure d'une application d'horloge atomique. Exemple d'appli : https://play.google.com/store/apps/details?id=partl.atomicclock&hl=fr&pli=1
* Redresser les antennes pour qu'elle soit comme-ceci :
<!-- TODO Image -->

Cela optimise la communication avec le PC.


### Préparation de l'ordinateur


* On branche la boite de télémetrie à l'ordinateur.
* On branche la manette d'XBOX à l'ordinateur.
* On allume le PC et on ouvre l'application Mission Planneur.
* On se connecte à la planche avec les paramètres suivants: <!-- TODO Image -->. Normalement, la planche devrait apparaitre sur la carte.
Il faut mettre le homepoint sur la planche pour éviter des ennuis

Dans plan, on dessine d'abord un polygone. Le menu est accessible avec un clique droit. On place 4 points histoire de faire un rectangle.
<!-- TODO Image -->
<!-- TODO Image -->


Une fois que la forme est satisfaisant, on click droit puis -> Auto-WP -> Survey grid. Une popup s'ouvre et montre le transect.
Bien vérfier qu'on est à 1 m/s à droite. En bas on peut voir le temps de la mission. Si celui-ci est énorme, il faut vérifier que le HomePoint (Lettre H sur la carte) est bien à côté de la planche. Une mission doit durer environ 1h (50min - 1h15). Au-dela, la gopro n'aura pas assez de batteries. En haut, on peut aller dans le deuxième tab menu pour changer le type de transect et mettre en cross grid ou linear.

Si tout est bon on retourne dans la première tab et on clique sur valider en bas à droite.
Un rectangle jaune apparait. 
* On clique sur Save pour sauvegarder localement le fichier de waypoints au cas ou (Ex: 20251110_REU-ST-LEU_ASV-1_01.waypoints)
* Ensuite, on clique sur write pour écrire les données sur la planche. Une fois que les waypoints sont envoyés, on peut mettre la planche à l'eau.
<!-- TODO Image -->

* On va dans Data.
* On va dans la fenêtre Actions pour connecter la manette. Joystick > Enable.

Fonctionnement de la manette :
- Joystick gauche haut-bas: Avancer et reculer (Eviter de reculer la planche s'enfonce)
- Joystick droit gauche-droite: La planche tourne à gauche ou à droite.
- Y: Désarmer les moteurs
- B: Armer les moteurs => Armer les moteurs seulement s'il n'y a personne autour (En cas d'urgence, toujours désarmer les moteurs.)
- X: Mode Manuelle => L'utilisateur pilote la planche.
- A: Mode Auto => La planche suit le plan que l'on vient de write dans sa mémoire.

Une fois que la planche est envoyé, on peut aller dans Quick et regarder les infos :
- Vérifier que l'echosondeur renvoir une valeur.
- Vérifier que GPS2 est en 3D fix.
<!-- TODO Image -->
A la fin de la mission, il est important d'éteindre et de rallumer la planche pour que les fichiers de la mission soit bien séparé. Sinon, la planche enregistera 2 missions sur le même fichier.

## Entretien de la planche.

- Graisser le joint qui ferme la pellicase.
- Après chaque sortie, il faut rincer tous le matos qui a touché l'eau de mer avec de l'eau douce.


## FAQ

*Est-ce que le REACH RS2/3 communique avec la planche ?*

Non, nous ne sommes pas en RTK. Le REACH RS2/3 est connecté au réseau Centipède à travers le partage de connexion et reçoit des corrections en temps réels. La planche n'est ni connecté à la bouboule ni à internet pour recevoir des corrections de trames.

*Comment est synchronisé la camera et le GPS ?*

Lors du démarrage de la session, on montre une horloge atomique avec une application sur téléphone. Quand on va découper les vidéos en images, on va noter l'heure affiché sur l'image et ensuite ce timestamp va servir à synchroniser temporellement les images et le GPS.

*Pourquoi faire du PPK au lieu du RTK ?*

On pourrait faire du RTK mais comme on a pas besoin d'avoir une bathymétrie en temps réel et qu'on doit recalculer le GPS pour recalibrer les images, on ne l'a pas mis.

*Que se passe-t-il si on perd la communication ordinateur-planche ?*

Rien, la communication peut être interrompu sans soucis. La planche est autonome et en fonction de la configuration revient à la plage quand la mission est fini ou bien reste à son dernier waypoints.