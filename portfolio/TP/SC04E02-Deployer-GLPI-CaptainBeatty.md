# 🐋 Déployer GLPI avec Docker Compose


## 🗂 Contexte

Vous êtes administrateur système dans une PME. Votre responsable vous demande de déployer **GLPI**, l'outil de gestion de parc informatique open-source, de façon reproductible et conteneurisée.

Plutôt que d'installer GLPI directement sur un serveur, vous allez utiliser **Docker Compose** pour orchestrer plusieurs services : l'application GLPI elle-même, sa base de données MariaDB, et en bonus un outil de gestion de BDD via une interface web.

> 💡 **Pourquoi Docker Compose ?**
> - Tout l'environnement est décrit dans un seul fichier : `docker-compose.yml`
> - Une seule commande pour tout démarrer, tout arrêter, tout reconstruire
> - L'environnement est identique sur tous les postes de l'équipe

---

## 🎯 Objectifs

À la fin de cet exercice, vous aurez :

- Rédigé un fichier `docker-compose.yml` fonctionnel de zéro
- Configuré un service MariaDB avec variables d'environnement
- Déployé GLPI et réalisé sa configuration initiale via le navigateur
- Mis en place la persistance des données avec des volumes Docker
- *(Bonus)* Ajouté Adminer pour administrer la base de données

---

## 📋 Contraintes & Règles du jeu

> ⚠️ **Important — À respecter impérativement**
>
> ✗ Ne pas copier-coller un `docker-compose.yml` tout fait depuis Internet  
> ✗ Ne pas utiliser d'image GLPI non-officielle ou préconfigurée  
> ✓ Partir des images officielles : `mariadb` et `diouxx/glpi` ou `glpi/glpi`  
> ✓ Construire votre fichier étape par étape en consultant la documentation  
> ✓ Tester chaque ajout avant de passer à l'étape suivante

---

## 🔧 Étapes guidées

### Étape 1 — Mise en place du projet

- Créez un dossier dédié pour votre projet : `mkdir glpi-docker && cd glpi-docker`
- Créez le fichier `docker-compose.yml` vide et préparez la structure de votre projet
- Réfléchissez aux services dont vous aurez besoin avant de commencer à écrire

### Étape 2 — Service MariaDB

- Ajoutez un service `mariadb` dans votre `docker-compose.yml`
- Définissez les variables d'environnement nécessaires : `MYSQL_ROOT_PASSWORD`, `MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD`
- Montez un volume pour persister les données de la base
- Testez que le conteneur démarre correctement avec : `docker compose up -d db`

### Étape 3 — Service GLPI

- Ajoutez le service `glpi` en utilisant l'image `glpi/glpi ou diouxx/glpi`
- Exposez le port 80 du conteneur sur un port de votre machine
- Configurez la dépendance vers le service `db` avec `depends_on`
- Montez les volumes nécessaires pour les fichiers GLPI (config, fichiers uploadés...)

### Étape 4 — Réseau & Communication

- Créez un réseau Docker dédié pour que vos services puissent communiquer
- Rattachez chaque service à ce réseau
- Vérifiez que GLPI peut joindre MariaDB : le nom d'hôte à utiliser est le **nom du service** `db`

### Étape 5 — Démarrage & Configuration initiale

- Lancez l'ensemble des services : `docker compose up -d`
- Ouvrez votre navigateur sur `http://localhost:8080`
- Suivez l'assistant d'installation GLPI en renseignant les informations de connexion à la BDD
- Connectez-vous avec les identifiants par défaut (`glpi` / `glpi`) et changez-les !

---

## 🔍 Indices & Documentation

Consultez ces ressources si vous êtes bloqués — mais essayez d'abord par vous-même !

| Ressource | URL / Commande |
|---|---|
| Doc Docker Compose | `docs.docker.com/compose/` |
| Image MariaDB (Docker Hub) | `hub.docker.com/_/mariadb` |
| Image GLPI | `hub.docker.com/r/diouxx/glpi` ou `hub.docker.com/r/glpi/glpi` |
| Image Adminer | `hub.docker.com/_/adminer` |
| Variables MariaDB | Voir section *Environment* dans la doc de l'image |
| Logs d'un service | `docker compose logs -f glpi` |
| Entrer dans un conteneur | `docker compose exec db bash` |
| Lister les conteneurs | `docker compose ps` |

---

## 📁 Structure de fichiers attendue

Votre projet doit avoir au minimum cette structure à la fin :

```
glpi-docker/
├── docker-compose.yml
└── (volumes gérés par Docker ou dossiers locaux)
```

Votre fichier `docker-compose.yml` doit contenir :

```yaml
version: '3.8'          # ou plus récent

services:
  db:                   # service MariaDB
    image: mariadb:...
    ...

  glpi:                 # service GLPI
    image: diouxx/glpi  # ou glpi/glpi:latest
    ...

  adminer:              # (bonus) interface d'admin BDD
    image: adminer
    ...

volumes:               # déclaration des volumes nommés
  ...

networks:              # réseau dédié
  ...
```

---

## ⭐ Bonus — Aller plus loin

### 🏆 Bonus 1 — Adminer

Ajoutez le service **Adminer** à votre stack. Adminer est une interface web légère pour administrer des bases de données. Exposez-le sur le port `8081` et connectez-vous avec les identifiants de votre base GLPI.

### 🏆 Bonus 2 — Fichier `.env`

Déplacez tous les mots de passe et variables sensibles dans un fichier `.env` et utilisez la syntaxe `${VARIABLE}` dans votre `docker-compose.yml`. Ajoutez `.env` à un fichier `.gitignore` pour ne jamais le commiter.

### 🏆 Bonus 3 — Healthcheck

Ajoutez un `healthcheck` sur le service `db` pour que GLPI n'essaie de démarrer qu'une fois que MariaDB est réellement prêt à accepter des connexions.

> Indice : `condition: service_healthy` dans `depends_on`
