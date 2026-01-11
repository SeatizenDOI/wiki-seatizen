# 🔋 Guide Batteries LiPo et LiHV

Ce guide couvre la signification des désignations de batterie, les différences entre LiPo et LiHV, ainsi que les procédures de charge et de sécurité essentielles.

## 1. Comprendre la tension (4S et 15,2 V)

La désignation `4S` signifie que **4 cellules** sont montées en série.

| Type de Cellule | Tension Nominale (par cellule) | Tension Max (par cellule) | Tension Nominale **4S** | Tension Max **4S** |
| --- | --- | --- | --- | --- |
| **LiPo** | 3,7 V | 4,20 V | **14,8 V** (4  3,7 V) | 16,8 V |
| **LiHV** (High Voltage) | 3,8 V | 4,35 V | **15,2 V** (4  3,8 V) | 17,4 V |

> 👉 **Conclusion :** Une batterie marquée **15,2 V** est systématiquement une **LiHV**.

---

## 2. Différences clés et précautions de charge

| Caractéristique | LiPo | LiHV |
| --- | --- | --- |
| **Chargeur** | Mode LiPo | **Mode LiHV obligatoire** |
| **Tension Max** | 4,20 V/cellule | **4,35 V/cellule** |
| **Avantages** | Meilleure longévité, tension plus stable | Boost initial, capacité légèrement supérieure |
| **Inconvénients** | Standard | Durée de vie réduite si chargée systématiquement à 4,35 V |

> ⚠️ **Avertissement de charge :**

> * **NE JAMAIS** charger une **LiPo** en mode **LiHV** (Risque d'incendie/explosion).

> * **NE JAMAIS** charger une **LiHV** en mode **LiPo** (Charge incomplète à seulement 16,8 V).
> 
> 

---

## 3. Éléments de la Batterie (Exemple 4S)

Chaque batterie 4S possède **deux connexions** distinctes, toutes deux nécessaires à la charge :

| Connexion | Rôle | Caractéristiques (Ex. 4S) |
| --- | --- | --- |
| **Principal** (Ex: XT60, XT90) | Passage de la puissance (Charge/Décharge) | 2 fils : + et − |
| **Équilibrage** (JST-XH) | Surveillance de la tension de chaque cellule | **5 fils** (4 cellules + 1 fil commun) |


<div align="center">
    <figure>
        <img src="/img/tutoriels/battery_XT60.webp" alt="Connecteur XT60">
        <figcaption>Figure 1: Le connecteur XT60, utilisé pour la puissance.</figcaption>
    </figure>
</div>


<div align="center">
    <figure>
        <img src="/img/tutoriels/battery_JST_XH.jpeg" alt="Connecteur JST-XH 5 pin">
        <figcaption>Figure 2: Le connecteur d'équilibrage **JST-XH 5 broches** pour une batterie 4S.</figcaption>
    </figure>
</div>

---

## 4. Procédure de Charge Sécuritaire

Le processus doit toujours se faire sous surveillance et sur une surface non inflammable (idéalement dans un sac LiPo ignifugé).

### ⚙️ Réglages du Chargeur (pour une LiHV 4S)

1. **Type de batterie** : `LiHV`
2. **Nombre de cellules** : `4S`
3. **Tension maximale** : `4,35 V / cellule` (17,4 V total)
4. **Courant de charge** :
* Règle standard : **1C** (Ex: 5000 mAh  **5,0 A**)
* **Astuce longévité** : Charger à **0,5C** (Ex: 5000 mAh  **2,5 A**)



### 🔌 Étapes de Connexion

1. Connecter le câble principal (puissance).
2. Connecter le câble d'équilibrage JST-XH sur l'entrée `4S` du chargeur.
> 📌 **Ne jamais forcer les connecteurs.**


3. Vérifier que le chargeur détecte bien `4 cellules`.
4. Lancer la charge et rester à proximité.

### Fin de Charge

* Une LiHV 4S pleine doit atteindre **17,4 V** total.
* L'écart entre les cellules doit être faible (0.1 V).
* Débrancher toujours le **câble principal** en premier.

---

## 5. Stockage et Décharge

### Stockage (Mode STORAGE)

Pour maximiser la durée de vie, les LiPo et LiHV doivent être stockées dans le mode **STORAGE** du chargeur, à une tension de :

* **3,80 V à 3,85 V par cellule**

### Courbe de Décharge (LiHV vs LiPo)

* **LiPo :** Tension max 4,20 V. Plateau de tension plus long et stable.
* **LiHV :** Tension max 4,35 V. Subit une **chute de tension rapide** en début de décharge.

**Pourquoi cette chute rapide ?**
La zone entre 4,20 V et 4,35 V n'est pas une zone de fonctionnement stable. Dès qu'un courant est demandé, la tension chute naturellement vers la zone stable de 4,1–4,2 V.
**Une chute rapide après 30 secondes d'utilisation NE signifie PAS que la batterie est vide.**

---

## 6. Sécurité et Erreurs à Éviter

| À Faire (Sécurité) | À Éviter (Erreurs Fréquentes) |
| --- | --- |
| ✅ Rester à proximité pendant la charge. | ❌ Oublier le câble d'équilibrage. |
| ✅ Charger sur une surface non inflammable. | ❌ Charger sur un courant excessif. |
| ✅ Utiliser le mode **STORAGE** pour le rangement. | ❌ Charger une LiHV en mode LiPo. |
| ❌ Ne jamais charger une batterie gonflée. | ❌ Laisser une charge sans surveillance. |

## Liens


<div align="center">
    <figure>
        <img src="/img/tutoriels/battery_chargeur.jpg" alt="Chargeur Gens Ace Imars Dual">
        <figcaption>Figure 3: <a href="https://www.amazon.fr/Gens-Batterie-Balance-Chargeur-D%C3%A9chargeur/dp/B09SWS6YZK?__mk_fr_FR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=2VXEVVCECIVJT&dib=eyJ2IjoiMSJ9.0n2GJwtoGJlRtcpccSP4CPeNkv2yXDK5YALRbrdQYmJu6vPSLqKR8Y2EbC_Qyuo-uYinpS4AMkcHodnozFOSIIredlc2m7xE_Ll6zIgDOOJOFP6g-GUJ_cgKGoISYgvPCyT8QbWt7kwqafe6TOUPnR6v7XxVgp16Hp0fC1VQA1DhmMSmwSRmzoFfdffDIofe5jlTA7LBxrdtRIjtBI-k5MHJFPXvUzwuKg4nJCdOCTT1MlL4JEA6qclkLW07hePlZu1bq7Fd6dR9iJ3YRjJjKlVj1LQL56GlYur8m5HQ5Y0.K8LuJfiQ3xm5SInthJpaFEpE3r8CcaIHeq3pPmkvLHk&dib_tag=se&keywords=chargeur%2Bgens%2Bace&qid=1768139738&sprefix=chargeur%2Blipo%2B4s%2Bgensace%2Caps%2C398&sr=8-7&th=1">Gens Ace Chargeur de Batterie Imars Dual</a></figcaption>
    </figure>
</div>

<div align="center">
    <figure>
        <img src="/img/tutoriels/battery_battery.webp" alt="Battery LiHV 4S">
        <figcaption>Figure 3: <a href="https://www.powerhobby.com/products/gens-ace-advanced-g-tech-10000mah-15-2v-100c-4s2p-hardcase-lipo-battery-pack-61-with-ec5-plug?variant=51467570250050&country=AT&currency=EUR&utm_medium=product_sync&utm_source=google&utm_content=sag_organic&utm_campaign=sag_organic">Gens ace Advanced G-Tech 10000mAh</a></figcaption>
    </figure>
</div>


