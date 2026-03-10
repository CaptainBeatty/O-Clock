# Déploiement de GLPI avec Docker Compose

## Objectif
J’ai déployé une instance de GLPI de manière conteneurisée avec Docker Compose.  
L’environnement comprend une base de données MariaDB, l’application GLPI et, en bonus, Adminer pour l’administration de la base.

## Étapes réalisées

### 1. Préparation du projet
J’ai créé un dossier de travail dédié nommé `glpi-docker` puis j’ai initialisé un fichier `docker-compose.yml`.

### 2. Déploiement du service MariaDB
J’ai ajouté un service `db` basé sur l’image officielle `mariadb`.  
J’ai défini les variables d’environnement nécessaires à l’initialisation de la base :
- `MYSQL_ROOT_PASSWORD`
- `MYSQL_DATABASE`
- `MYSQL_USER`
- `MYSQL_PASSWORD`

J’ai également configuré un volume Docker pour assurer la persistance des données de la base.

### 3. Déploiement du service GLPI
J’ai ensuite ajouté un service `glpi` basé sur l’image `diouxx/glpi`.  
J’ai exposé le port `80` du conteneur sur le port `8080` de la machine hôte afin d’accéder à l’interface web depuis un navigateur.

J’ai mis en place une dépendance vers le service `db` pour que GLPI puisse utiliser MariaDB comme backend.

### 4. Mise en place du réseau Docker
J’ai créé un réseau Docker dédié afin d’assurer la communication entre les différents services de la stack.  
Le service GLPI utilise le nom d’hôte `db` pour joindre le serveur MariaDB.

### 5. Installation initiale de GLPI
Après le démarrage des conteneurs avec `docker compose up -d`, j’ai accédé à GLPI via `http://localhost:8080`.  
J’ai suivi l’assistant d’installation en renseignant les paramètres de connexion à la base MariaDB.

### 6. Ajout d’Adminer
En complément, j’ai ajouté un service `adminer`, exposé sur le port `8081`, afin d’administrer la base de données via une interface web.

### 7. Sécurisation de la configuration
J’ai externalisé les variables sensibles dans un fichier `.env` afin d’éviter de laisser les mots de passe en clair dans le fichier `docker-compose.yml`.  
J’ai également ajouté `.env` dans `.gitignore`.

### 8. Fiabilisation du démarrage
J’ai configuré un `healthcheck` sur MariaDB afin que GLPI ne tente de démarrer qu’une fois la base réellement prête à accepter les connexions.

## Résultat
À la fin du TP, la stack Docker permet de déployer GLPI de manière reproductible, lisible et maintenable.  
Les données sont persistantes grâce aux volumes Docker et les services communiquent sur un réseau dédié.

## Captures d’écran conseillées
- Création du dossier de projet

  images/01-creation-dossier-glpi-docker.png
- Conteneur MariaDB démarré
- Logs MariaDB
- Fichier `docker-compose.yml`
- Liste des conteneurs avec `docker compose ps`
- Réseau Docker inspecté
- Assistant d’installation GLPI
- Paramètres de connexion à la base
- Interface GLPI après installation
- Interface Adminer
- Fichier `.env`
- Configuration du `healthcheck`
