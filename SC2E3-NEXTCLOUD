# 📦 TP – Déploiement et Administration d'une Plateforme Collaborative Nextcloud  
## Projet : EduLearn

## 🎯 Objectif

Mettre en place une plateforme collaborative interne pour une organisation fictive nommée **EduLearn**, en simulant un environnement d’entreprise :

- Gestion des utilisateurs
- Structuration des services par départements
- Gestion des droits d’accès
- Collaboration (Fichiers, Agenda, Talk, Deck)
- Automatisation via scripts
- Validation fonctionnelle

---

## 🏗️ Architecture technique

- OS : Ubuntu Linux (VM)
- Solution : Nextcloud (installation Snap)
- Accès : HTTP (environnement de lab)
- Services activés :
  - Files
  - Calendar
  - Talk
  - Deck
  - Nextcloud Office (Collabora intégré)

---

## 👥 Gestion des utilisateurs

Création automatisée de comptes via script Bash utilisant `nextcloud.occ`.

Exemples de comptes :
- alice.martin
- bob.durand
- charlie.dev
- hannah.prof
- etc.

Les mots de passe ont été générés automatiquement pour respecter les règles de sécurité Nextcloud.

---

## 👥 Gestion des groupes

Création des groupes suivants :

- Direction
- Developpement
- Pedagogie
- Marketing
- Support

Attribution des utilisateurs à leurs groupes respectifs.

---

## 📁 Structuration des dossiers

Création automatisée d’une arborescence projet :

EduLearn/
├── 1_Direction/
├── 2_Developpement/
├── 3_Pedagogie/
├── 4_Marketing/
├── 5_Support/
└── Commun/


Sous-dossiers spécifiques par service (Code_Source, Documentation, Templates, etc.).

Indexation via `nextcloud.occ files:scan --all`.

---

## 🔐 Gestion des permissions

- Chaque dossier départemental partagé uniquement avec son groupe
- Droits :
  - Lecture / écriture pour le groupe concerné
  - Aucun accès pour les autres
- Dossier `Commun` accessible à tous les groupes

Isolation validée via tests multi-utilisateurs.

---

## 📅 Agenda partagé

Création d’agendas d’équipe :

- Réunions Équipe
- Congés et Absences
- Événements Marketing

Partage des calendriers avec les groupes correspondants.
Validation par connexion multi-utilisateurs.

---

## 🗂️ Gestion de projet (Deck)

Création d’un board :

**Roadmap EduLearn Q1 2025**

Colonnes :
- Backlog
- À faire
- En cours
- Terminé

Simulation de tâches :
- Intégration SSO
- Refonte page d’accueil
- RGPD cookies
- Déploiement interne

---

## 🎥 Collaboration en temps réel

- Test édition collaborative via Nextcloud Office
- Test messagerie instantanée via Talk
- Appel audio/vidéo limité en HTTP (HTTPS requis pour WebRTC)

---

## ✅ Tests fonctionnels réalisés

| Test | Résultat |
|------|----------|
| Isolation des dossiers | OK |
| Partage groupe | OK |
| Dossier commun | OK |
| Agenda partagé | OK |
| Board collaboratif | OK |
| Édition temps réel | OK |

---

## 📌 Compétences démontrées

- Administration Nextcloud
- Automatisation via Bash
- Gestion des droits d’accès
- Structuration collaborative d’entreprise
- Tests et validation fonctionnelle
- Logique DevOps de déploiement

---

## 🚀 Perspectives d’amélioration

- Mise en place HTTPS (Let’s Encrypt)
- Intégration LDAP / Active Directory
- Mise en place sauvegarde automatisée
- Reverse proxy (Nginx)
- Déploiement Docker ou HA

---

## 📖 Conclusion

Ce TP simule le déploiement d’une plateforme collaborative en environnement professionnel.  
L’objectif était de structurer, sécuriser et tester une solution opérationnelle adaptée à une organisation multi-services.

L’infrastructure est fonctionnelle et validée.
