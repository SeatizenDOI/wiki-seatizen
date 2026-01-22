# Session d'acquisition Plancha

## Conditions météo et heure d'acquisition

Une session d'acquisition se réalise le matin, idéalement au **lever** du soleil pour éviter les **caustiques** au fond de l'eau.

## Préparation du matériel d'acquisition pour une session

* **Ordinateur de terrain** avec Mission Planner.
* **Boîtier de télémétrie** pour établir la communication.
* **Manette** pour piloter la planche.
* **2 batteries** 4S 10A 1C (permettent de réaliser deux sessions).
* **1 GoPro avec ses batteries** : prévoir les batteries (1 par session), le caisson étanche et les outils de fixation.
* **Un Reach RS2/RS3** (« bouboule ») qui fera office de base GPS, avec son trépied et un téléphone pour le partage de connexion.

<div align="center">
    <figure>
        <img src="/img/plancha/matos.jpg" alt="Matos">
        <figcaption>Matériel.</figcaption>
    </figure>
</div>

Les gopros doivent être configurés comme ceci :

<div align="center">
    <figure>
        <img src="/img/plancha/gopro_1.png" alt="Configuration GoPro partie 1">
        <figcaption>Configuration GoPro partie 1.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/gopro_2.png" alt="Configuration GoPro partie 2">
        <figcaption>Configuration GoPro partie 2.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/gopro_3.png" alt="Configuration GoPro partie 3">
        <figcaption>Configuration GoPro partie 3.</figcaption>
    </figure>
</div>


## Procédure sur la plage

### Préparation du REACH RS2/3

Pour plus d'informations, [voir le tutoriel d'Emlid](https://docs.emlid.com/reachrs3/).

* Déployer le **trépied** dans un espace dégagé (sans arbres ni **bâtiments**).
* Activer le partage de connexion sur son téléphone.
* Allumer le Reach et attendre que les LEDs deviennent **bleues**.
* Vérifier que la base est en **FIX** dans l'application Emlid Flow. Il faut la paramétrer pour qu'elle se connecte à une base Centipede : [Lien tutoriel Centipede](https://docs.centipede.fr/docs/centipede/3_connect_caster.html).

<div align="center">
    <figure>
        <img src="/img/plancha/serveur_ntrip_bouboule.jpeg" alt="Liste des serveurs ntrip sur la bouboule.">
        <figcaption>Liste des serveurs ntrip.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/setup_bouboule.jpeg" alt="Setup de la Bouboule sur la plage.">
        <figcaption>Setup de la bouboule sur la plage.</figcaption>
    </figure>
</div>



### Préparation de la planche

* **Vérifier la tension des batteries** au multimètre et s'assurer que l'écart entre elles ne dépasse pas **0,1 V**. Si la différence est supérieure, ne pas les utiliser sous peine de les endommager.
* **Installer les batteries** dans la planche et l'allumer.
> **⚠️ Attention :** Ouvrir le caisson de la planche doucement, car l'appel d'air peut aspirer des gouttes d'eau à l'intérieur.


* **Installer la batterie dans la GoPro** et placer celle-ci dans son caisson.
> **⚠️ Important :** Nettoyer soigneusement le joint du caisson s'il présente des grains de sable.


* **Juste avant la mise à l'eau**, démarrer l'enregistrement de la GoPro et filmer l'heure sur une application d'horloge atomique (pour la synchronisation). Exemple : [Atomic Clock](https://play.google.com/store/apps/details?id=partl.atomicclock&hl=fr&pli=1).
* **Optimiser la communication** avec le PC en redressant bien les antennes.

<div align="center">
    <figure>
        <img src="/img/plancha/plancha_ouverte.jpeg" alt="Positionnement des batteries.">
        <figcaption>Positionnement des batteries.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/vissage_camera.jpeg" alt="Fixation de la caméra">
        <figcaption>Fixation de la caméra. ⚠️ Elle doit être allumée ⚠️</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/heure_gps.png" alt="Synchronisation de l'heure.">
        <figcaption>Synchronisation de l'heure.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/antennes.png" alt="Antennes.">
        <figcaption>Antennes.</figcaption>
    </figure>
</div>

### Préparation de l'ordinateur


<div align="center">
    <figure>
        <img src="/img/plancha/setup_pc_plage.jpeg" alt="Setup du matériel.">
        <figcaption>Setup du matériel.</figcaption>
    </figure>
</div>

### Configuration logicielle et lancement

* **Brancher le boîtier de télémétrie** et la **manette Xbox** à l'ordinateur.
* Lancer l'application **Mission Planner**.
* **Se connecter à la planche** avec les paramètres suivants : 

<div align="center">
    <figure>
        <img src="/img/plancha/plancha_com_10.png" alt="COM">
        <figcaption>Le port COM change à chaque fois mais souvent c'est celui en dessous des 3 Communications.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/connexion_en_cours.png" alt="Connexion">
        <figcaption>Connexion en cours, chargement des paramètres de la planche.</figcaption>
    </figure>
</div>


Une fois connectée, la planche doit apparaître sur la carte.

<div align="center">
    <figure>
        <img src="/img/plancha/asv_on_map.png" alt="ASV on Map">
        <figcaption>Apparition de la planche sur le carte. Ici à Ifremer au Port.</figcaption>
    </figure>
</div>

* **Définir le "Home Point"** sur la position actuelle de la planche pour éviter tout incident (RTL - Return to Launch).

#### Création du plan de vol (Mission)

1. Dans l'onglet **Plan**, dessiner un polygone : faire un clic droit sur la carte pour accéder au menu et placer 4 points pour former un rectangle. 
2. Une fois la forme satisfaisante, faire un clic droit -> **Auto-WP** -> **Survey (Grid)**. Une fenêtre contextuelle s'ouvre affichant le tracé (transect).
3. **Vérifications :**
* **Vitesse :** Vérifier que la vitesse est bien réglée sur **1 m/s** (panneau de droite).
* **Temps de mission :** Vérifier la durée estimée en bas de l'écran. Elle doit être comprise entre **50 min et 1h15**. Au-delà, l'autonomie de la GoPro sera insuffisante.
* **Home Point :** Si le temps estimé est anormalement élevé, vérifier que le point **H** (Home) est bien situé à côté de la planche.
* **Type de tracé :** Dans le deuxième onglet du menu, vous pouvez changer le type de transect (Cross Grid ou Linear).
4. Si tout est correct, revenir sur le premier onglet et cliquer sur **Accept** (ou Valider). Un rectangle jaune apparaît sur la carte.

<div align="center">
    <figure>
        <img src="/img/plancha/interface_1.png" alt="Step 0">
        <figcaption>On place le Homepoint à côté de la session que l'on va dessiner.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/draw_polygon.png" alt="Step 1">
        <figcaption>Clique droit sur la carte pour afficher la fenêtre.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/polygon_1.png" alt="Step 2">
        <figcaption>Dessinez un polygon.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/survey_grid.png" alt="Step 3">
        <figcaption>Clique droit sur la carte pour afficher la fenêtre.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/first_parameters.png" alt="Step 4">
        <figcaption>On peut voir la vitesse de la planche et le temps d'acquisition.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/second_parameters.png" alt="Step 5">
        <figcaption>On peut régler la forme du transect et si c'est bon on valide.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/survey_grid_2.png" alt="Step 6">
        <figcaption>On obtient un transect.</figcaption>
    </figure>
</div>


#### Enregistrement et transfert

* **Sauvegarder localement :** Cliquer sur **Save** pour enregistrer le fichier de waypoints par sécurité (Ex: `20251110_REU-ST-LEU_ASV-1_01.waypoints`).
* **Envoyer à la planche :** Cliquer sur **Write** pour transférer les données vers la mémoire de la planche. Une fois le transfert terminé, la planche peut être mise à l'eau.
* Aller dans l'onglet **Data**, puis dans la fenêtre **Actions** pour activer la manette : **Joystick > Enable**.

<div align="center">
    <figure>
        <img src="/img/plancha/save_survey.png" alt="Sauvegarde du transect.">
        <figcaption>On sauvegarde le transect.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/write_on_asv.png" alt="On écrit le transect sur la planche.">
        <figcaption>On envoie le transect sur la planche en cliquant sur Write.</figcaption>
    </figure>
</div>

---

### Commandes de la manette (Xbox)

* **Joystick gauche (Haut/Bas) :** Avancer / Reculer.
> **Note :** Éviter de reculer, car l'arrière de la planche a tendance à s'enfoncer dans l'eau.


* **Joystick droit (Gauche/Droite) :** Direction (Virages).
* **Bouton X :** Désarmer les moteurs.
* **Bouton B :** Armer les moteurs.
> **⚠️ Attention :** N'armer que si la zone est dégagée. En cas d'urgence, désarmer immédiatement.


* **Bouton Y :** Mode **Manual** (Pilotage manuel).
* **Bouton A :** Mode **Auto** (La planche suit les waypoints enregistrés).

<div align="center">
    <figure>
        <img src="/img/plancha/joystick_1.png" alt="Joystick 1.">
        <figcaption>On ouvre l'onglet Joystick.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/joystick_2.png" alt="Joystick 2.">
        <figcaption>On active la manette.</figcaption>
    </figure>
</div>

---

### Suivi de mission et fin de session

Une fois la planche lancée, surveiller les indicateurs dans l'onglet **Quick** :

* Vérifier que l'**échosondeur** renvoie une valeur de profondeur.
* Vérifier que le **GPS2** affiche un statut **3D Fix** (ou mieux, RTK Fixed).

<div align="center">
    <figure>
        <img src="/img/plancha/quick.png" alt="Quick.">
        <figcaption>On peut surveiller les valeurs de la planche. Ici on est à terre et dans un bureau donc le GPS2 n'est pas en 3DFix et l'echosondeur indique 0.12m.</figcaption>
    </figure>
</div>

**Important :** À la fin de chaque mission, il est impératif d'éteindre, puis de rallumer la planche. Cela permet de séparer correctement les fichiers de log. Dans le cas contraire, les données de deux missions successives seront enregistrées dans le même fichier, compliquant le post-traitement.


## Entretien de la planche

* **Graisser le joint** d'étanchéité de la Pellicase (utiliser de la graisse silicone si nécessaire).
* **Rinçage systématique :** Après chaque sortie, il est impératif de rincer abondamment à l'eau douce tout le matériel ayant été en contact avec l'eau de mer (coque, moteurs, capteurs, visserie).


## FAQ

**Est-ce que le REACH RS2/3 communique avec la planche ?**

Non, nous ne travaillons pas en **RTK** (Real Time Kinematic). Le REACH RS2/3 est connecté au réseau **Centipede** via le partage de connexion et reçoit des corrections en temps réel. La planche, quant à elle, n'est connectée ni à la « bouboule » ni à Internet pour recevoir des trames de correction.

**Comment sont synchronisés la caméra et le GPS ?**

Lors du démarrage de la session, on filme une application d'horloge atomique sur téléphone. Lors de l'extraction des images de la vidéo, on note l'heure précise **affichée** sur l'image. Ce **timestamp** (horodatage) servira ensuite à synchroniser temporellement les clichés avec les données du log GPS.

**Pourquoi faire du PPK au lieu du RTK ?**

Le RTK serait possible, mais comme nous n'avons pas besoin d'une bathymétrie en temps réel et que nous devons de toute façon recalculer la position du GPS pour géoréférencer les images avec précision, le **PPK** (Post-Processed Kinematic) est privilégié. C'est plus simple logistiquement sur le terrain.

**Que se passe-t-il si l'on perd la communication entre l'ordinateur et la planche ?**

Rien de grave : la communication peut être **interrompue** sans souci. La planche est autonome. Selon sa configuration, elle revient à son point de départ (**RTL**) une fois la mission terminée ou reste en attente sur son dernier waypoint. Il est même possible d'utiliser 2 planches sur le même ordinateur.

**Comment régler la fréquence entre la planche et le boîtier ?**

On utilise le logiciel **RFDTools** (disponible sur le site de [RFDesign](https://files.rfdesign.com.au/tools/)).

* **Connexion :** Brancher le boîtier de télémétrie à l'ordinateur, sélectionner le bon port COM et cliquer sur **Connect**.
* **Lecture des paramètres :** Cliquer sur **Load Settings** pour voir la configuration actuelle de la radio locale.
* **Synchronisation :** Pour que la planche et le boîtier communiquent, les paramètres suivants doivent être **identiques** sur les deux radios :
* **Net ID :** L'identifiant du réseau (ex: 25).
* **Air Speed :** La vitesse de transmission.
* **Min/Max Frequency :** La plage de fréquences autorisée.


* **Validation :** Une fois les modifications effectuées, cliquer sur **Save Settings**.

> **Note :** Il est conseillé de ne pas modifier les fréquences d'usine sans une connaissance précise de la réglementation locale sur les bandes de fréquences (868 MHz en Europe / 915 MHz aux USA).

<div align="center">
    <figure>
        <img src="/img/plancha/rfd_tools.png" alt="RFD Tools.">
        <figcaption>Exemple d'une connexion avec la planche jaune.</figcaption>
    </figure>
</div>

**Comment régler la vitesse de la planche ?**

On peut la régler à 0.5m/s si on est dans le lagon pour faire de la photogrammétrie mais sinon, il faut la laisser à 1m/s en pente externe.

<div align="center">
    <figure>
        <img src="/img/plancha/asv_1ms.png" alt="Asv 1ms">
        <figcaption>Paramètres de la planche pour avancer à 1 m/s.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/plancha/asv_05ms.png" alt="Asv 0.5ms">
        <figcaption>Paramètres de la planche pour avancer à 0.5 m/s.</figcaption>
    </figure>
</div>
