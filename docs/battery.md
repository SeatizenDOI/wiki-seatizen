# 🔋 Charger une batterie **15,2 V – 4S**

## Comprendre et utiliser correctement les batteries **LiPo** et **LiHV**

---

## 1. Signification de « 15,2 V – 4S »

* **4S** signifie **4 cellules montées en série**
* Tension **nominale par cellule** :

  * **LiPo** : 3,7 V
  * **LiHV** : 3,8 V

### Tension nominale totale

* **LiPo 4S** → 4 × 3,7 V = **14,8 V**
* **LiHV 4S** → 4 × 3,8 V = **15,2 V**

👉 **Une batterie 15,2 V est donc une batterie LiHV (High Voltage)**.

---

## 2. Différences entre batteries **LiPo** et **LiHV**

| Caractéristique   | LiPo             | LiHV                                    |
| ----------------- | ---------------- | --------------------------------------- |
| Tension nominale  | 3,7 V / cellule  | 3,8 V / cellule                         |
| Tension maximale  | 4,20 V / cellule | 4,35 V / cellule                        |
| Tension max en 4S | 16,8 V           | 17,4 V                                  |
| Capacité utile    | Standard         | Légèrement supérieure                   |
| Durée de vie      | Bonne            | Réduite si chargée à 4,35 V fréquemment |
| Mode de charge    | LiPo             | **LiHV obligatoire**                    |

⚠️ **Ne jamais charger une LiPo en mode LiHV**
⚠️ **Ne jamais charger une LiHV en mode LiPo** (charge incomplète)

---

## 3. Matériel nécessaire pour charger une LiHV 4S

* ✔ Chargeur **LiPo/LiHV avec équilibrage**
* ✔ Alimentation du chargeur (si nécessaire)
* ✔ Câble principal (XT60, XT90, EC5, etc.)
* ✔ Câble d’équilibrage **JST-XH (5 fils pour une 4S)**
* ✔ Sac ignifugé LiPo (fortement recommandé)

---

## 4. Connecteurs d’une batterie 4S

Une batterie 4S possède **toujours deux connexions**.

### 4.1 Connecteur principal

* Permet le passage de la puissance
* Utilisé pour la charge et la décharge
* Exemples : XT60, XT90, EC5, Deans

### 4.2 Connecteur d’équilibrage

* Généralement de type **JST-XH**
* **5 fils pour une 4S**
* Permet au chargeur de surveiller chaque cellule

👉 **Les deux connecteurs doivent être branchés pour une charge équilibrée et sûre**

---

## 5. Procédure de charge (pas à pas)

### Étape 1 : Installation

* Poser la batterie sur une surface **non inflammable**
* Idéalement dans un **sac LiPo**

---

### Étape 2 : Connexion du câble principal

* Brancher la batterie au câble de sortie du chargeur
* Vérifier la polarité (+ / −)

---

### Étape 3 : Connexion du câble d’équilibrage

* Brancher le connecteur JST-XH sur l’entrée **4S** du chargeur
* Le détrompeur empêche une mauvaise orientation

📌 **Ne jamais forcer un connecteur**

---

### Étape 4 : Réglages du chargeur

* **Type de batterie** : `LiHV`
* **Nombre de cellules** : `4S`
* **Tension maximale** : `4,35 V / cellule`
* **Courant de charge** :

  * Règle standard : **1C**
  * Exemples :

    * 1500 mAh → **1,5 A**
    * 5000 mAh → **5 A**

👉 Charger à **0,5C** augmente la durée de vie de la batterie

---

### Étape 5 : Lancement de la charge

* Vérifier que le chargeur détecte bien **4 cellules**
* Contrôler la tension individuelle des cellules
* Démarrer la charge

---

## 6. Sécurité pendant la charge

✅ Rester à proximité
✅ Vérifier que la batterie ne chauffe pas
❌ Ne jamais charger une batterie gonflée
❌ Ne jamais laisser une batterie en charge sans surveillance

---

## 7. Fin de charge

* Une **LiHV 4S pleine** atteint **17,4 V**
* Les cellules doivent être équilibrées (écart ≤ ±0,02 V)
* Débrancher dans l’ordre :

  1. Câble principal
  2. Câble d’équilibrage

---

## 8. Stockage des batteries

* LiPo et LiHV doivent être stockées à :

  * **3,80 à 3,85 V par cellule**
* Utiliser le mode **STORAGE** du chargeur

---

## 9. Erreurs fréquentes à éviter

❌ Charger une LiHV en mode LiPo
❌ Oublier le câble d’équilibrage
❌ Charger à un courant excessif
❌ Charger sur une surface inflammable

---

## 10. Courbe de décharge : LiPo vs LiHV

### LiPo

* Tension max : **4,20 V / cellule**
* Plateau de tension stable
* Décharge progressive

### LiHV

* Tension max : **4,35 V / cellule**
* Sur-tension temporaire en fin de charge
* Chute rapide initiale, puis plateau stable

👉 La différence majeure se situe **au début de la décharge**

---

## 11. Pourquoi une LiHV pleine chute rapidement en tension ?

### Phénomène normal

* La zone **4,20 → 4,35 V** n’est pas une zone de fonctionnement stable
* Cette surtension existe :

  * À vide
  * Juste après la charge

### Explication simplifiée

* Densité ionique plus élevée
* Résistance interne plus importante
* Stabilité chimique réduite

➡️ Dès qu’un courant est demandé :

* La tension chute rapidement vers **4,1–4,2 V**
* La capacité réelle reste disponible

---

## 12. Exemple réel (LiHV 4S)

| Situation                | Tension       |
| ------------------------ | ------------- |
| Fin de charge            | 17,4 V        |
| Après 30 s d’utilisation | 16,6 – 16,8 V |
| Régime stable            | 15,5 – 15,8 V |
| Fin de décharge          | ~13,2 V       |

👉 **Une chute rapide ≠ batterie vide**

---

## 13. Comparaison à l’usage

### LiPo

* Tension plus stable
* Meilleure longévité
* Moins de stress chimique

### LiHV

* Boost initial
* Légère capacité supplémentaire
* Vieillit plus vite si chargée systématiquement à 4,35 V

