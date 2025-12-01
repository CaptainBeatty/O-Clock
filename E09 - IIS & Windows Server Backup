# 🛡️ TP – Sauvegarde & Restauration d’Active Directory sur Windows Server

---

## 1️⃣ Sauvegarde initiale

Avant toute opération critique, une **sauvegarde complète VSS** est effectuée afin de disposer d’un instantané fiable du système.

![Sauvegarde complète](/captures/5d4ee2fc-6de2-47aa-b340-338c9670cd3a.png)

---

## 2️⃣ Suppression accidentelle des OU du domaine

Dans le cadre de l’exercice, l’OU *Promotions* et toutes ses sous-unités sont supprimées par erreur.

### Confirmation de la suppression

![Confirmation suppression Promotions](/captures/18ce9ae4-1e02-49fe-bb35-fbeeb7eb6f18.png)

### Résultat : les OU ont disparu

![OU supprimées](/captures/8e29964c-2278-4f15-b299-32632c6280bd.png)

---

## 3️⃣ Passage en mode de réparation Active Directory (DSRM)

La restauration de l’état du système nécessite de démarrer le serveur en **Directory Services Repair Mode**.

### Activation via msconfig

![Démarrage sécurisé Active Directory](/captures/9f58f952-551a-4045-89c6-1897c1aaf056.png)

> Ce mode peut également être activé via la touche **F8** au démarrage.

---

## 4️⃣ Restauration de l’état du système

Une fois en DSRM, la restauration est lancée depuis **Sauvegarde Windows Server**.

### Sélection de la sauvegarde à restaurer

![Sélection sauvegarde](/captures/dfc9f453-1ec2-469b-b0dd-32cb7120a3f5.png)

### Choix du type de restauration : État du système

![Choix état du système](/captures/55a9d44a-f295-48a2-a158-eab95978838b.png)

### Retour au démarrage normal

Une fois la restauration lancée ou terminée, le serveur est reconfiguré pour démarrer en mode standard.

![Retour mode normal](/captures/4ae9e4f8-43d0-4e7d-977b-5e001361f4eb.png)

> ⚠️ La restauration est **non autoritaire**, afin de laisser AD reprendre sa place normalement.

---

## 5️⃣ Vérification post-restauration

Après redémarrage en mode normal, l’ensemble des OU supprimées réapparaît.

![OU restaurées](/captures/e29754c2-d220-46dc-86d4-8aec8d6d94fe.png)

Les OU **Aldebaran**, **Andromède**, **Basilic**, et **Zinc** ont été restaurées avec succès.

---

## ✅ Conclusion

Ce TP démontre la maîtrise des éléments suivants :

- Réalisation d’une sauvegarde complète (VSS)
- Simulation d’un incident critique Active Directory
- Boot en mode DSRM
- Restauration de l’état du système sans restauration autoritaire
- Récupération complète de la structure Active Directory

L’annuaire a été entièrement restauré et l’environnement est de nouveau opérationnel.

---
