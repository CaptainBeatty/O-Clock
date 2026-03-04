
# 🛡️ Sécurisation d’un serveur Debian 13 exposé sur Internet

**Environnement :** VM Debian 13 minimale déployée sur hyperviseur Proxmox
**Contexte :** Préparation à la mise en production d’un serveur municipal avec exigences minimales de durcissement

---

## 🎯 Objectif de la mission

J’ai réalisé un durcissement de base du système afin de :

* Sécuriser l’accès distant via SSH
* Restreindre strictement les connexions entrantes
* Mettre en place un filtrage réseau persistant
* Supprimer les vecteurs d’attaque évidents (root direct, mot de passe, etc.)

L’approche a été volontairement minimaliste, robuste et conforme aux bonnes pratiques d’administration système.

---

# 1️⃣ Installation et validation du service SSH

## Actions réalisées

* Mise à jour des dépôts système
* Installation du service **OpenSSH Server**
* Vérification du statut du service
* Vérification du port d’écoute
* Activation du service au démarrage

## Résultat obtenu

* Service SSH actif
* Écoute sur le port 22
* Service persistant au reboot

📸 **Capture d’écran nécessaire :**

* `systemctl status ssh`
* `ss -tlnp | grep ssh`

Ces éléments permettent de prouver que le service est fonctionnel et correctement exposé.

---

# 2️⃣ Mise en place du filtrage réseau avec iptables

## Stratégie appliquée

J’ai mis en place une politique de sécurité restrictive :

* Politique par défaut : **DROP en entrée**
* Autorisation du loopback
* Autorisation des connexions établies (ESTABLISHED, RELATED)
* Autorisation du SSH uniquement depuis mon IP publique
* Blocage implicite de toute autre tentative

## Implémentation

* Nettoyage complet des règles existantes
* Application des politiques par défaut
* Ajout des règles dans un ordre logique
* Installation de `iptables-persistent`
* Sauvegarde via `netfilter-persistent`

## Résultat obtenu

* Accès SSH restreint à une seule IP
* Aucune autre connexion entrante autorisée
* Règles persistantes après redémarrage

📸 **Captures d’écran nécessaires :**

* `iptables -L -n -v`
* Fichier de règles sauvegardé (`/etc/iptables/rules.v4` si utilisé)
* Test de connexion réussie depuis IP autorisée

Ces éléments démontrent la cohérence et la persistance du filtrage.

---

# 3️⃣ Durcissement du service SSH (Hardening)

## Mesures appliquées

Modification du fichier `/etc/ssh/sshd_config` :

* Désactivation du login root (`PermitRootLogin no`)
* Désactivation de l’authentification par mot de passe (`PasswordAuthentication no`)
* Activation explicite de l’authentification par clé (`PubkeyAuthentication yes`)
* Restriction des utilisateurs autorisés (`AllowUsers utilisateur`)

## Sécurisation de l’accès

* Génération d’une clé SSH de type **ed25519**
* Déploiement de la clé publique sur le serveur
* Test de connexion par clé
* Validation de la syntaxe avant rechargement (`sshd -t`)
* Rechargement du service sans interruption

## Résultat obtenu

* Accès root distant impossible
* Authentification par mot de passe désactivée
* Accès strictement limité à un utilisateur défini
* Authentification uniquement par clé asymétrique

📸 **Captures d’écran nécessaires :**

* Extrait pertinent du `sshd_config`
* Test de connexion SSH réussi
* Message d’échec lors d’une tentative root ou mot de passe

Ces preuves sont essentielles pour démontrer le durcissement réel et non théorique.

---

# 🔐 Bonus éventuels réalisés (si applicable)

* Modification du port SSH
* Création d’un utilisateur administrateur avec droits sudo
* Politique iptables enrichie

📸 Capture à prévoir si modification du port ou création utilisateur sudo.

---

# 📊 Synthèse technique

| Axe                  | Mesure appliquée              | Niveau de criticité |
| -------------------- | ----------------------------- | ------------------- |
| Service SSH          | Installation + activation     | Critique            |
| Filtrage réseau      | Politique DROP + IP autorisée | Critique            |
| Root login           | Désactivé                     | Critique            |
| Auth mot de passe    | Désactivée                    | Critique            |
| Persistance firewall | Activée                       | Critique            |

---

# ✅ Conclusion

Le serveur Debian a été sécurisé selon une logique de défense en profondeur :

1. Réduction de la surface d’exposition réseau
2. Restriction stricte des accès
3. Suppression des mécanismes d’authentification faibles
4. Validation systématique avant fermeture de session

L’environnement est désormais conforme aux exigences minimales de sécurité pour une mise en production contrôlée.

---



