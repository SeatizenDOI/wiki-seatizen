
# Guide d'utilisation du catamaran Blueboat (Opération eDNA)

Ce tutoriel détaille la procédure de mise en service du Blueboat pour des opérations de pompage d'ADN environnemental, incluant la gestion logicielle et le protocole de décontamination.

<div align="center">
    <figure>
        <img src="/img/blueboat/blueboat_1.jpg" alt="Blueboat.">
        <figcaption>Blueboat.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/blueboat/blueboat_2.jpg" alt="Blueboat.">
        <figcaption>Blueboat.</figcaption>
    </figure>
</div>

## 1. Prérequis matériels

Pour une opération complète, assurez-vous de disposer des éléments suivants :

* Catamaran Blueboat.
* BaseStation (utiliser une canne/trépied pour augmenter la portée).
* 4 batteries chargées (différence de tension maximale de **0.1V** entre elles).
* Un ordinateur avec Google Chrome et QGroundControl + GPS emlid.
* Une manette Xbox.

<div align="center">
    <figure>
        <img src="/img/blueboat/matos.jpg" alt="Matériel.">
        <figcaption>Matériel.</figcaption>
    </figure>
</div>

Le GPS emlid peut être intéressant lors du sortie sur le bateau pour avoir la position de l'ordinateur par rapport au point de pompage notamment pour le pilote du bateau.

## 2. Configuration réseau de l'ordinateur

La BaseStation agit comme un point d'accès Wi-Fi. Le Blueboat possède l'adresse IP fixe `192.168.2.2`. Pour communiquer avec lui et utiliser **QGroundControl**, votre ordinateur doit impérativement être sur le même sous-réseau, idéalement avec l'adresse `192.168.2.1`.

### Forcer l'adresse IP statique

Par défaut, l'ordinateur utilise le protocole DHCP (adresse aléatoire). Il faut le désactiver pour forcer l'adresse :

<div align="center">
    <figure>
        <img src="/img/blueboat/reseau_wifi.png" alt="Wifi step 1">
        <figcaption>Se connectez au réseau BaseStation.</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/blueboat/wifi_options_adaptateur.png" alt="Wifi step 2">
        <figcaption>Allez dans les paramètres et cliquez sur "Modifier les options d'adaptateur".</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/blueboat/wifi_propriete.png" alt="Wifi step 3">
        <figcaption>Faire un clique droit sur le réseau wifi et allez dans l'onglet "Propriétés" .</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/blueboat/wifi_ipv4.png" alt="Wifi step 4">
        <figcaption>Double cliquez "Protocole Internet version 4(TCP/IPv4)".</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/blueboat/wifi_ipv4_2.png" alt="Wifi step ">
        <figcaption>Changez les valeurs.</figcaption>
    </figure>
</div>

> **Note :** Sans cette configuration, l'interface web peut fonctionner, mais le pilotage via QGroundControl sera impossible.

> Documentation officielle : [BlueRobotics Software Setup](https://bluerobotics.com/learn/Blueboat-software-setup/)

<div align="center">
    <figure>
        <img src="/img/blueboat/qground_control_ready_to_fly.png" alt="QGroundControl.">
        <figcaption>QGroundControl connecté avec le BlueBoat.</figcaption>
    </figure>
</div>

## 3. Mise en service du Blueboat

### Installation des batteries

Répartissez les batteries de chaque côté des coques pour équilibrer l'assiette du bateau (éviter qu'il ne penche). Connectez ensuite les deux types de prises sur le caisson :

* **Prise à 2 points :** Alimentation de la pompe (12V).
* **Prise à 4 points :** Connexion USB entre la carte **Navigator** et l'**Arduino** de la pompe.

⚠️ **Important :** Vissez impérativement les antennes de la BaseStation et du Blueboat **avant** la mise sous tension pour éviter d'endommager les émetteurs radio.

### Démarrage

Tournez le bouton d'allumage jusqu'à la position maximale. Le système est prêt lorsque la LED blanche clignote.

## 4. Interface de contrôle (eDNA Controller)

L'application de gestion de la pompe est accessible via l'onglet `eDNA Controller` à gauche sur la page `192.168.2.2`.

<div align="center">
    <figure>
        <img src="/img/blueboat/blueos.png" alt="Blueos">
        <figcaption>Interface Blueos du BlueBoat dans Chrome à l'url 192.168.2.2</figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/blueboat/parametre_pompe.png" alt="Paramètre">
        <figcaption>Sélectionnez les paramètres.</figcaption>
    </figure>
</div>


### Dépannage de la connexion (Mode Pirate)


<div align="center">
    <figure>
        <img src="/img/blueboat/courbe_evolue_pas.png" alt="Courbe sans évolution.">
        <figcaption>Interface sans réponse, les courbes n'évoluent pas.</figcaption>
    </figure>
</div>

Si les courbes n'évoluent pas, il faut juste relancer l'application. Cela arrive souvent surtout si la connexion entre le blueboat et la basestation est instable (distance, grosse vague, ...) :

1. Ouvrez l'application **Terminal** sur l'interface. L'application **Terminal** apparaît uniquement en mode pirate.

<div align="center">
    <figure>
        <img src="/img/blueboat/pirate_mode.png" alt="Pirate">
        <figcaption>Activez le mode pirate.</figcaption>
    </figure>
</div>

2. Tapez la commande `red-pill` pour accéder au système réel.
3. Exécutez le script : `./reload_docker.sh`.

<div align="center">
    <figure>
        <img src="/img/blueboat/terminal_restart.png" alt="Terminal">
        <figcaption>Relancez le terminal.</figcaption>
    </figure>
</div>


4. L'onglet **eDNA Controller** va redémarrer et la connexion devrait s'établir.

<div align="center">
    <figure>
        <img src="/img/blueboat/courbe_evolue.png" alt="Courbe avec évolution.">
        <figcaption>L'application fonctionne.</figcaption>
    </figure>
</div>

### Pilotage et navigation

* **Manuel :** Utilisez la manette Xbox. Une fois sur zone, passez en mode **Loiter** (maintien de position).
* **Automatique :** Définissez des points de passage (Waypoints) sur la carte et lancez la mission.

## 5. Procédure de pompage et surveillance

### Fonctionnement de l'interface

Privilégiez les **petits boutons ronds** à côté des graphiques plutôt que le bouton "Lancer la pompe". Ils agissent directement sur les relais de la pompe sans le délai de sécurité du bouton général. Survoler les boutons avec la souris pour identifier leurs fonctions.

### Amorçage et diagnostic

L'amorçage doit se faire idéalement à l'eau claire **sans filtres**. Surveillez le tableau suivant pour valider le flux :

| État du processus | Pression | Débit | Diagnostic |
| --- | --- | --- | --- |
| **Début** | Monte | Bas | Expulsion de l'air résiduel. |
| **Amorçage réussi** | **Descend** | **Stable** | Flux continu établi. |
| **Échec** | Reste **haute** | **Nul** | Pompe désamorcée (bulle d'air). |

> **Conseil débit faible :** Si le débit est très faible (~0.3L/min), un décalage peut apparaître entre l'ordinateur et l'écran physique de la pompe. Pour atteindre **18L** réels, arrêtez-vous à **16.4L** affichés sur l'interface logicielle.

---

## 6. Protocole scientifique (Acquisition ADN)

### Phase 1 : Décontamination et Blanc

1. **Désinfection :** Pomper 10L de Javel à 2 % en circuit fermé.
2. **Rincage :** Pomper 2L d'eau distillée en circuit ouvert.
3. **Surfaces :** Nettoyer le matériel à la Javel à 10 %, Ethanol à 70 % puis à l'eau MilliQ.
4. **Blanc :** Pomper 2L d'eau Milli-Q avec un filtre témoin. Pomper de l'air afin d'effectuer une dessication du filtre.

### Phase 2 : Prélèvement des triplicats

* **Réamorçage :** Toujours réamorcer la pompe à l'eau MilliQ avant d'installer les filtres.
* **Installation :** Placer le triplicat de filtres. Par forte houle, immergez-les suffisamment pour éviter les prises d'air.
* **Pompage :** Activer l'**Autostop** pour un arrêt automatique au volume cible.
* **Séchage :** Une fois revenu au bord, pomper de l'air pour dessiquer les filtres.

### Phase 3 : Fin de mission et entretien

1. **Rincage circuit :** Pomper de l'eau claire (distillée ou Milli-Q) pour rincer les tuyaux.
2. **Rincage extérieur :** Rincer l'ensemble du Blueboat à l'eau douce.
3. **Batteries :** Attendre que les coques soient parfaitement sèches avant de retirer les batteries.

> Note: Il faut noter l'heure de chacune des opérations pour faire le lien avec les logs de la pompe.