# Challenge E06 — BIOS, UEFI, MBR et GPT

## 🎯 Objectif
Ce travail pratique a pour but d’expérimenter la gestion des tables de partitions **GPT** et **MBR** à travers un support USB.  
L’exercice consiste à convertir la table GPT en MBR, puis à créer plusieurs partitions formatées dans un système de fichiers **FAT32**, en manipulant l’outil **DiskPart** sous Windows 11.

---

## 🧩 Étape 1 — Préparation du support

La clé USB a été connectée à la machine virtuelle Windows 11 et identifiée comme **Disque 1**.  
L’objectif initial est de **supprimer les partitions existantes**, puis de **convertir la table GPT vers MBR**.

```cmd
diskpart
list disk
select disk 1
clean
convert mbr
````

✅ **Résultat attendu :**
DiskPart confirme la conversion du disque au format **MBR**.

**Capture :**
(./captures/diskpart1.png)

---

## ⚙️ Étape 2 — Création et formatage des partitions

Une première tentative de partition de 8 Go a échoué (espace insuffisant).
Une partition plus petite a ensuite été créée avec succès, formatée et montée.

### Première partition :

```cmd
create partition primary size=500
format fs=fat32 quick label=PART1
assign letter=Z
```

### Deuxième partition :

```cmd
create partition primary
format fs=fat32 quick label=PART2
assign letter=Y
```

✅ **Résultat :**
Les deux partitions sont correctement créées et formatées en **FAT32**, chacune avec une lettre de lecteur distincte.

**Capture :**
`captures/diskpart2.png`

---

## 📋 Étape 3 — Vérification finale

Pour vérifier la structure finale du disque :

```cmd
list volume
```

### Sortie DiskPart :

| Volume | Lettre | Nom   | Fs    | Type     | Taille | Statut |
| :----- | :----- | :---- | :---- | :------- | :----- | :----- |
| 8      | Z      | PART1 | FAT32 | Amovible | 500 M  | Sain   |
| 9      | Y      | PART2 | FAT32 | Amovible | 460 M  | Sain   |

✅ Les deux volumes apparaissent bien dans la liste, avec leurs lettres et systèmes de fichiers FAT32.

---

## 🧠 Bilan

Ce challenge a permis de :

* Manipuler **DiskPart** pour la conversion **GPT → MBR**.
* Créer et formater plusieurs partitions manuellement.
* Comprendre les limites du format **MBR** (nombre de partitions primaires limité, espace réduit).
* Vérifier la structure du disque et les volumes actifs via `list volume`.

L’ensemble des opérations a été mené à bien sur un **second support USB sain**, après diagnostic et remplacement d’une clé Kingston défectueuse.

---

## 🧰 Informations complémentaires

### Problème rencontré

La première clé testée (Kingston DataTraveler 3.0) présentait une **erreur d’E/S** et un **état en lecture seule permanent**, empêchant tout formatage ou conversion.

### Diagnostic

* `DiskPart` indiquait : *"Impossible de satisfaire à la demande en raison d’une erreur de périphérique d’E/S."*
* La clé était **verrouillée matériellement** en lecture seule (firmware de protection activé).
* Test confirmé hors VM : le blocage provenait du **contrôleur interne** de la clé.

