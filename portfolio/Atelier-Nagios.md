
# TP — Mise en place d’une supervision avec Nagios Core

## Contexte

Dans le cadre de ce TP, j’ai déployé une solution de supervision basée sur **Nagios Core 4.5.11** sur une machine **Ubuntu 24.04**.  
L’objectif était de mettre en place une infrastructure de monitoring permettant de superviser plusieurs hôtes du réseau, notamment :

- un serveur **Windows**
- un serveur **Ubuntu**
- un serveur **Nagios Core**

Les hôtes supervisés remontent des métriques via l’agent **NCPA (Nagios Cross Platform Agent)**.

---

# 1. Installation du serveur de supervision Nagios

## 1.1 Mise à jour du système

J’ai commencé par mettre à jour le système Ubuntu afin de garantir l’installation de paquets récents et compatibles avec Nagios.

Cette étape permet d’éviter les conflits de dépendances lors de la compilation.

---

## 1.2 Installation des dépendances

J’ai installé les dépendances nécessaires au fonctionnement de Nagios Core, notamment :

- Apache (serveur web)
- PHP
- bibliothèques graphiques
- outils de compilation

Ces composants sont indispensables pour compiler Nagios et afficher son interface web.

📸 Capture recommandée  
`../images/tp-nagios/02-install-dependencies.png`

---

## 1.3 Téléchargement et compilation de Nagios Core

J’ai téléchargé l’archive **Nagios Core 4.5.11** depuis le site officiel puis extrait les sources dans le répertoire `/tmp`.

J’ai ensuite configuré l’environnement de compilation en spécifiant l’emplacement de la configuration Apache et le groupe de commandes Nagios.

La compilation a ensuite été lancée afin de générer les binaires nécessaires au fonctionnement du logiciel.

📸 Capture recommandée  
`images/tp-nagios/03-configure-nagios.png`

---

## 1.4 Installation de Nagios et configuration des permissions

J’ai créé les utilisateurs et groupes nécessaires au fonctionnement de Nagios, notamment :

- utilisateur `nagios`
- groupe `nagcmd`

Ces éléments permettent à Nagios et Apache de communiquer correctement via les commandes externes.

J’ai ensuite installé les fichiers binaires, les scripts de démarrage et les fichiers de configuration par défaut.

📸 Capture recommandée  
`images/tp-nagios/04-install-nagios.png`

---

## 1.5 Installation de l’interface Web

J’ai installé la configuration Apache permettant d’accéder à l’interface web de Nagios.

J’ai ensuite :

- activé le module **CGI**
- activé le site Nagios
- redémarré le service Apache

Cette interface web constitue le point central de supervision de l’infrastructure.

📸 Capture recommandée  
`images/tp-nagios/05-apache-nagios.png`

---

## 1.6 Création de l’utilisateur d’administration

Afin de sécuriser l’accès à l’interface web, j’ai créé l’utilisateur **nagiosadmin** avec authentification Apache.

Cet utilisateur est utilisé pour se connecter à l’interface de supervision.

📸 Capture recommandée  
`images/tp-nagios/06-create-nagios-user.png`

---

## 1.7 Démarrage du service Nagios

Une fois l’installation terminée, j’ai démarré le service Nagios et configuré son démarrage automatique au boot.

📸 Capture recommandée  
`images/tp-nagios/07-nagios-service.png`

---

## 1.8 Vérification de l’interface de supervision

J’ai vérifié le bon fonctionnement de l’installation en accédant à l’interface web via :

```

http://IP_SERVEUR/nagios

```

L’interface permet de visualiser l’état des hôtes et des services supervisés.

📸 Capture recommandée  
`images/tp-nagios/08-nagios-dashboard.png`

---

# 2. Installation des plugins Nagios

Afin d’activer les contrôles de supervision, j’ai installé les **plugins Nagios officiels**.

Ces plugins permettent notamment de :

- vérifier la disponibilité des hôtes
- contrôler les services réseau
- analyser les performances des systèmes

Après compilation et installation, les plugins ont été installés dans :

```

/usr/local/nagios/libexec

```

📸 Capture recommandée  
`images/tp-nagios/09-nagios-plugins.png`

---

## Correction du plugin Ping

Lors des tests, le plugin **check_ping** ne fonctionnait pas car la commande ping n’était accessible qu’au superutilisateur.

J’ai corrigé ce problème en modifiant les permissions du binaire ping afin de permettre son exécution par Nagios.

📸 Capture recommandée  
`images/tp-nagios/10-ping-permission-fix.png`

---

# 3. Supervision d’un serveur Windows avec NCPA

## 3.1 Installation de l’agent NCPA

Sur la machine Windows, j’ai installé **NCPA (Nagios Cross Platform Agent)**.

Cet agent expose une API sécurisée permettant à Nagios d’interroger les métriques du système.

Lors de l’installation, j’ai configuré :

- un **token d’authentification**
- le démarrage automatique du service

📸 Capture recommandée  
`images/tp-nagios/11-ncpa-install-windows.png`

---

## 3.2 Vérification du service NCPA

J’ai vérifié que le service **NCPA** était actif dans le gestionnaire des services Windows.

📸 Capture recommandée  
`images/tp-nagios/12-ncpa-service-running.png`

---

## 3.3 Test de l’interface NCPA

J’ai ensuite testé l’interface web NCPA accessible via :

```

https://IP_WINDOWS:5693

```

Cette interface permet de consulter les métriques système exposées par l’agent.

📸 Capture recommandée  
`images/tp-nagios/13-ncpa-web-interface.png`

---

# 4. Ajout du serveur Windows dans Nagios

Sur le serveur Nagios, j’ai créé un fichier de configuration dans :

```

/usr/local/nagios/etc/servers/windows_server.cfg

```

Ce fichier définit :

- l’hôte Windows supervisé
- les services surveillés (CPU et mémoire)

Après modification de la configuration, j’ai redémarré Nagios pour appliquer les changements.

📸 Capture recommandée  
`images/tp-nagios/14-windows-host-config.png`

---

# 5. Supervision d’un serveur Ubuntu avec NCPA

## 5.1 Installation de l’agent NCPA

Sur la machine Ubuntu, j’ai ajouté le dépôt Nagios puis installé l’agent NCPA via le gestionnaire de paquets.

J’ai ensuite configuré le **token API** dans le fichier :

```

/usr/local/ncpa/etc/ncpa.cfg

```

📸 Capture recommandée  
`images/tp-nagios/15-ncpa-install-ubuntu.png`

---

## 5.2 Vérification du service NCPA

J’ai démarré et activé le service NCPA afin qu’il se lance automatiquement au démarrage du système.

📸 Capture recommandée  
`images/tp-nagios/16-ncpa-service-ubuntu.png`

---

## 5.3 Ajout du serveur Ubuntu dans Nagios

J’ai ajouté un nouvel hôte dans la configuration Nagios en définissant :

- l’adresse IP du serveur
- les services supervisés (CPU, mémoire)

📸 Capture recommandée  
`images/tp-nagios/17-ubuntu-host-config.png`

---

# 6. Installation du plugin check_ncpa

Afin de permettre à Nagios d’interroger les agents NCPA, j’ai téléchargé et installé le script :

```

check_ncpa.py

```

Ce plugin agit comme une interface entre Nagios Core et les agents NCPA installés sur les serveurs supervisés.

📸 Capture recommandée  
`images/tp-nagios/18-install-check-ncpa.png`

---

# 7. Vérification finale de la supervision

Après rechargement de la configuration Nagios, j’ai vérifié dans l’interface web que :

- le serveur **Windows**
- le serveur **Ubuntu**

étaient bien visibles dans la liste des hôtes supervisés.

Les métriques CPU et mémoire remontent correctement via l’agent NCPA.

📸 Capture recommandée  
`images/tp-nagios/19-final-monitoring-dashboard.png`

---

# Résultat

À l’issue de ce TP, j’ai déployé une **plateforme complète de supervision basée sur Nagios Core** permettant :

- la surveillance des hôtes du réseau
- la collecte de métriques système via **NCPA**
- la visualisation centralisée des états dans l’interface web Nagios

Cette architecture constitue une base fonctionnelle pour la supervision d’une infrastructure système et réseau.

```

---

## Nomenclature des captures recommandées

Pour ton repo GitHub :

```
images/
└── tp-nagios
    ├── 01-update-system.png
    ├── 02-install-dependencies.png
    ├── 03-configure-nagios.png
    ├── 04-install-nagios.png
    ├── 05-apache-nagios.png
    ├── 06-create-nagios-user.png
    ├── 07-nagios-service.png
    ├── 08-nagios-dashboard.png
    ├── 09-nagios-plugins.png
    ├── 10-ping-permission-fix.png
    ├── 11-ncpa-install-windows.png
    ├── 12-ncpa-service-running.png
    ├── 13-ncpa-web-interface.png
    ├── 14-windows-host-config.png
    ├── 15-ncpa-install-ubuntu.png
    ├── 16-ncpa-service-ubuntu.png
    ├── 17-ubuntu-host-config.png
    ├── 18-install-check-ncpa.png
    └── 19-final-monitoring-dashboard.png
```
