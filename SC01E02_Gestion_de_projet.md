# 📘 Modernisation de l’infrastructure IT du campus

## Work Breakdown Structure (WBS)

---

## 🎯 Objectif du projet

L’objectif de ce projet est de moderniser l’infrastructure informatique du campus afin de permettre le déploiement de nouveaux services numériques (serveurs de fichiers, NAS, firewall, VLAN, Wi-Fi sécurisé), tout en garantissant :

* la continuité pédagogique,
* la sécurité des accès,
* le respect des bonnes pratiques IT.

---

## 🧱 Découpage du projet (WBS)

Le **Work Breakdown Structure (WBS)** ci-dessous présente une décomposition hiérarchique du projet.
Il permet d’identifier clairement les lots, les tâches associées et, lorsque nécessaire, les sous-tâches.

> 🔎 **Lecture du WBS**
>
> * Niveau 1 : Lots du projet
> * Niveau 2 : Tâches
> * Niveau 3 : Sous-tâches (le cas échéant)

---

## 📊 Représentation graphique du WBS

```mermaid
mindmap
  root((Modernisation de l’infrastructure IT du campus))
    Lot 1 : Serveurs de fichiers
      Lot 1.1 : Choix du matériel
      Lot 1.2 : Commande du matériel
      Lot 1.3 : Réception et contrôle du matériel
      Lot 1.4 : Installation et configuration
        Lot 1.4.1 : Vérification du bon fonctionnement du matériel
        Lot 1.4.2 : Installation et configuration du système
        Lot 1.4.3 : Intégration à l’infrastructure existante
      Lot 1.5 : Tests et validation de l’infrastructure
    Lot 2 : NAS
      Lot 2.1 : Choix du matériel
      Lot 2.2 : Commande du matériel
      Lot 2.3 : Réception et contrôle du matériel
      Lot 2.4 : Installation et configuration
      Lot 2.5 : Tests et validation de l’infrastructure
    Lot 3 : Firewall
      Lot 3.1 : Choix du matériel
      Lot 3.2 : Commande du matériel
      Lot 3.3 : Réception et contrôle du matériel
      Lot 3.4 : Installation et configuration
      Lot 3.5 : Tests et validation de l’infrastructure
    Lot 4 : VLAN
      Lot 4.1 : Installation du matériel existant
      Lot 4.2 : Configuration du matériel existant
      Lot 4.3 : Tests et validation de l’infrastructure
    Lot 5 : Accès sécurisé Wi-Fi
      Lot 5.1 : Installation du matériel existant
      Lot 5.2 : Configuration du matériel existant
      Lot 5.3 : Tests et validation de l’infrastructure
```
## 📦 Lots, objectifs et livrables

| Lot | Intitulé | Objectif principal | Livrables attendus |
|----|---------|-------------------|-------------------|
| Lot 1 | Serveurs de fichiers | Mettre à disposition un service de stockage centralisé et accessible aux utilisateurs | Serveurs de fichiers installés, configurés et testés |
| Lot 2 | NAS | Fournir une solution de stockage mutualisée et sécurisée | NAS opérationnel et intégré à l’infrastructure |
| Lot 3 | Firewall | Sécuriser les flux entrants et sortants du réseau | Firewall installé, configuré et validé |
| Lot 4 | VLAN | Segmenter le réseau afin d’isoler les usages et renforcer la sécurité | VLAN fonctionnels et testés |
| Lot 5 | Accès sécurisé Wi-Fi | Offrir un accès sans fil sécurisé aux utilisateurs | Réseau Wi-Fi sécurisé et validé |

---

## 🗓️ Planning prévisionnel du projet

```mermaid
gantt
    title Planning - Modernisation de l’infrastructure IT du campus
    dateFormat  YYYY-MM-DD
    axisFormat  %d/%m

    section Lot 1 : Serveurs de fichiers
    Choix du matériel              :a1, 2026-03-01, 5d
    Commande du matériel           :a2, after a1, 5d
    Réception et contrôle          :a3, after a2, 3d
    Installation et configuration :a4, after a3, 5d
    Tests et validation            :a5, after a4, 3d

    section Lot 2 : NAS
    Choix du matériel              :b1, after a5, 4d
    Commande du matériel           :b2, after b1, 4d
    Réception et contrôle          :b3, after b2, 3d
    Installation et configuration :b4, after b3, 4d
    Tests et validation            :b5, after b4, 3d

    section Lot 3 : Firewall
    Choix du matériel              :c1, after b5, 3d
    Commande du matériel           :c2, after c1, 3d
    Réception et contrôle          :c3, after c2, 2d
    Installation et configuration :c4, after c3, 4d
    Tests et validation            :c5, after c4, 3d

    section Lot 4 : VLAN
    Installation matériel existant :d1, after c5, 3d
    Configuration                  :d2, after d1, 3d
    Tests et validation            :d3, after d2, 2d

    section Lot 5 : Accès sécurisé Wi-Fi
    Installation matériel existant :e1, after d3, 3d
    Configuration                  :e2, after e1, 3d
    Tests et validation            :e3, after e2, 2d
```
---
