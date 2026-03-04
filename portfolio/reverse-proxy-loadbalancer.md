# Mise en place d’un reverse-proxy, load-balancer et HTTPS

## Contexte technique

Dans le cadre de ce TP réalisé sur mon environnement **Proxmox**, j’ai déployé une architecture web distribuée composée de plusieurs serveurs Apache placés derrière un **reverse-proxy** et un **load-balancer**. L’objectif était d’illustrer la terminaison TLS, la répartition de charge et la haute disponibilité. 

L’infrastructure comprend :

* **3 serveurs web Apache** (web1, web2, web3)
* **1 serveur proxy** capable d’assurer plusieurs rôles (Nginx, Apache et HAProxy)
* **1 poste client Windows 11** permettant les tests d’accès

Tous les conteneurs sont connectés au réseau **LAN A (vmbr2 — 10.0.0.0/16)**.

---

# 1 — Création de l’infrastructure Proxmox

J’ai commencé par déployer **quatre conteneurs LXC Debian 12** dans Proxmox afin de constituer l’architecture du lab.

Chaque conteneur a été configuré avec :

* un **hostname distinct**
* une **adresse IP statique**
* une **passerelle vers le firewall pfSense**
* un **DNS public**

Une fois les conteneurs déployés, j’ai vérifié la **connectivité réseau** depuis chaque machine vers :

* la passerelle
* Internet

Cette vérification permet de confirmer que le **NAT et le routage fonctionnent correctement**.

📸 **Capture recommandée**

* Configuration réseau d’un conteneur dans Proxmox
* Test de connectivité (ping gateway / Internet)

---

# 2 — Déploiement des serveurs web Apache

J’ai installé le serveur web **Apache** sur les trois conteneurs applicatifs.

Chaque serveur héberge une page HTML distincte permettant d’identifier visuellement quel serveur répond à la requête :

* **web1** : page bleue
* **web2** : page verte
* **web3** : page rouge

Cette différenciation visuelle est utilisée plus tard pour **observer le comportement du load-balancing**.

J’ai ensuite vérifié l’accessibilité de chaque serveur depuis le poste client via HTTP.

📸 **Captures recommandées**

* Page web de **web1**
* Page web de **web2**
* Page web de **web3**

---

# 3 — Mise en place du HTTPS sur Apache

Afin d’introduire le chiffrement TLS, j’ai généré un **certificat auto-signé** sur le serveur web1.

Après avoir activé le module SSL d’Apache, j’ai configuré un **VirtualHost HTTPS** permettant d’exposer le site sur le port **443**.

Un test depuis le navigateur confirme :

* l’activation du **chiffrement HTTPS**
* l’apparition d’un **avertissement navigateur** lié au certificat auto-signé

Ce comportement est normal dans un environnement de laboratoire.

📸 **Captures recommandées**

* Avertissement navigateur HTTPS
* Détails du certificat TLS

---

# 4 — Mise en place d’un reverse-proxy avec Nginx

J’ai installé **Nginx** sur le conteneur proxy afin de le configurer comme **reverse-proxy**.

Le proxy redirige les requêtes HTTP vers les différents serveurs web selon l’URL demandée :

* `/web1` → serveur web1
* `/web2` → serveur web2
* `/web3` → serveur web3

Cette architecture permet :

* de **masquer les serveurs back-end**
* de centraliser l’accès web derrière une seule adresse IP

Les tests depuis le poste client montrent que les requêtes transitent correctement par le proxy.

📸 **Captures recommandées**

* Page d’accueil du reverse-proxy
* Accès aux trois sites via le proxy

---

# 5 — Activation du HTTPS sur le reverse-proxy

J’ai ensuite configuré **Nginx avec un certificat TLS**, permettant d’assurer la **terminaison TLS** directement au niveau du proxy.

Dans cette configuration :

* le **client communique en HTTPS avec le proxy**
* les **serveurs back-end restent en HTTP**

Cette architecture est très courante en production car elle :

* simplifie la gestion des certificats
* réduit la charge de chiffrement sur les serveurs applicatifs.

Une redirection automatique **HTTP → HTTPS** a également été configurée.

📸 **Capture recommandée**

* Navigation HTTPS sur le proxy avec cadenas

---

# 6 — Mise en place d’un reverse-proxy Apache (comparaison)

Afin de comparer les solutions, j’ai configuré **Apache comme reverse-proxy** sur le serveur proxy.

Pour éviter les conflits avec Nginx, Apache a été configuré pour écouter sur le **port 8443**.

Cette configuration permet d’observer que :

* Apache peut assurer les mêmes fonctions qu’un reverse-proxy
* la logique repose sur les directives **ProxyPass** et **ProxyPassReverse**

Les tests montrent que les requêtes sont correctement redirigées vers les serveurs web.

📸 **Capture recommandée**

* Accès HTTPS via Apache sur le port 8443

---

# 7 — Mise en place d’un load-balancer HAProxy

Dans la dernière étape, j’ai déployé **HAProxy** afin de mettre en place un **load-balancing entre les trois serveurs web**.

La configuration repose sur :

* un **frontend HTTPS**
* un **backend contenant les trois serveurs Apache**

L’algorithme utilisé est **Round Robin**, qui distribue les requêtes de manière équitable entre les serveurs.

J’ai également activé :

* les **health checks**
* une **interface de statistiques** accessible via le navigateur.

📸 **Captures recommandées**

* Page de statistiques HAProxy
* Exemple de rotation des serveurs lors des rafraîchissements

---

# 8 — Tests de fonctionnement

Plusieurs scénarios de validation ont été réalisés.

### Test de répartition de charge

En rafraîchissant la page web plusieurs fois, les réponses proviennent successivement de :

* web1
* web2
* web3

Cela confirme le fonctionnement du **load-balancing Round Robin**.

---

### Test de haute disponibilité

J’ai simulé la panne d’un serveur web en arrêtant Apache sur **web2**.

HAProxy détecte automatiquement l’indisponibilité du serveur et cesse d’envoyer des requêtes vers celui-ci.

Après redémarrage du service Apache, le serveur est automatiquement **réintégré dans le pool de charge**.

📸 **Captures recommandées**

* Serveur DOWN dans les stats HAProxy
* Retour du serveur en état UP

---

# Conclusion

Ce TP m’a permis de mettre en œuvre plusieurs composants clés d’une architecture web moderne :

* **HTTPS et chiffrement TLS**
* **Reverse-proxy**
* **Terminaison TLS**
* **Répartition de charge**
* **Détection automatique des pannes**
* **Supervision via HAProxy**

Cette architecture améliore à la fois la **sécurité**, la **scalabilité** et la **résilience** d’un service web.

---


