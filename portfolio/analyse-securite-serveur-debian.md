# Analyse de sécurité d’un serveur Debian exposé sur le réseau

## Contexte

Dans le cadre de ma formation d’Administrateur d’Infrastructures Sécurisées, j’ai réalisé une analyse de sécurité d’un serveur Debian accessible sur le réseau.  
L’objectif de cet atelier était d’observer le serveur **du point de vue d’un attaquant**, afin d’identifier les services exposés, les informations divulguées et les éventuelles vulnérabilités.

Cette démarche correspond à la **phase de reconnaissance utilisée lors d’un audit de sécurité ou d’un pentest**.

---

# 1. Identification du serveur

La première étape consiste à identifier la machine cible ainsi que sa configuration réseau.

Commande utilisée :

```bash
ip a
```
Cette commande permet de vérifier l’interface réseau et l’adresse IP du serveur.

Le serveur possède l'adresse IP :

10.0.0.130

Il est hébergé dans un environnement virtualisé.

2. Analyse de la surface d’exposition réseau

Un attaquant commence généralement par identifier les ports ouverts sur la machine cible.

Commande utilisée :

nmap 10.0.0.130

Résultat :

Port	Service
22	SSH
80	HTTP

La surface d’attaque du serveur est relativement limitée puisque seuls deux services sont accessibles.

3. Identification des versions des services

Une fois les services détectés, il est possible d’identifier leurs versions.

Commande utilisée :
```
nmap -sV 10.0.0.130
```
Résultat :

Service	Version
SSH	OpenSSH 10.0p2
HTTP	Apache 2.4.66

Ces informations peuvent être utilisées par un attaquant pour rechercher des vulnérabilités connues.

4. Recherche de vulnérabilités

Nmap permet d’exécuter des scripts permettant de détecter certaines vulnérabilités connues.

Commande utilisée :

nmap --script vuln 10.0.0.130

Le scan n’a pas identifié de vulnérabilités critiques au niveau du serveur web.

5. Analyse du serveur web

Afin d’identifier les informations divulguées par le serveur web, une requête HTTP a été effectuée.

Commande utilisée :
```
curl -I http://10.0.0.130
```
Le serveur divulgue l’information suivante :

Server: Apache/2.4.66 (Debian)

Cela constitue une divulgation d’informations, car un attaquant peut identifier précisément la version du serveur web.

6. Vérification des services actifs côté serveur

La commande suivante permet d’identifier les services actuellement en écoute sur le serveur.

ss -tulnp

Plusieurs services sont observés :

SSH

HTTP

services applicatifs internes

Cette étape permet de vérifier la cohérence entre la configuration interne du serveur et les résultats obtenus lors du scan Nmap.

7. Analyse des journaux système

Les journaux du serveur permettent d’identifier les activités réseau et les tentatives d’accès.

Commande utilisée :

tail -n 20 /var/log/apache2/access.log

Les logs montrent notamment des requêtes provenant de l’outil Nmap Scripting Engine, ce qui confirme qu’un scan de sécurité a été effectué sur le serveur.

Conclusion

Cette analyse a permis d’identifier les éléments suivants :

Points positifs

surface d’exposition réseau limitée

peu de ports ouverts

services identifiés

aucune vulnérabilité critique détectée

Points d’amélioration

divulgation de la version du serveur Apache

présence de services internes en écoute

exposition du serveur aux scans automatisés

Recommandations

Plusieurs mesures peuvent être mises en place pour renforcer la sécurité du serveur :

désactiver l’accès SSH pour l’utilisateur root

mettre en place une authentification par clé SSH

masquer la version du serveur Apache

déployer un pare-feu (UFW ou nftables)

surveiller régulièrement les journaux système

Conclusion générale

Cette activité permet de comprendre comment un serveur peut être observé du point de vue d’un attaquant.

La phase de reconnaissance constitue une étape essentielle dans un audit de sécurité, car elle permet d’identifier les services exposés et les informations pouvant être exploitées par un attaquant.
