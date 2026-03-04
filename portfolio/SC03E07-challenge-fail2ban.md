
# 🛡️ Sécurisation d’un serveur Linux exposé (Proxmox)

## 📌 Contexte

Dans le cadre d’un TP réalisé sur un hyperviseur **Proxmox**, j’ai déployé une machine virtuelle Debian/Ubuntu exposée avec un service SSH actif.
L’objectif était de renforcer la sécurité face aux attaques par force brute et de mettre en place un mécanisme de dissimulation du port SSH.

TP réalisé à partir du document fourni 

---

# 1️⃣ Audit préalable des connexions SSH

### 🎯 Objectif

Analyser l’activité SSH avant toute mesure corrective.

### 🔎 Actions réalisées

* Consultation des dernières connexions réussies (`last`)
* Analyse des tentatives échouées (`lastb`)
* Surveillance en temps réel du fichier `/var/log/auth.log`
* Simulation d’attaques par tentatives répétées depuis une machine cliente

### 🧠 Analyse

J’ai identifié :

* Des tentatives répétées depuis une même IP
* Des essais ciblant des comptes standards (root / utilisateurs génériques)
* Une activité nocturne caractéristique d’un scan automatisé

📸 **Capture d’écran nécessaire :**

* Extrait de `lastb`
* Extrait du `tail -f /var/log/auth.log`

➡️ Justification : preuve tangible d’activité malveillante initiale.

---

# 2️⃣ Déploiement et configuration de Fail2ban

### 🎯 Objectif

Mettre en place un bannissement automatique après plusieurs échecs SSH.

### ⚙️ Actions réalisées

* Installation du service `fail2ban`
* Activation au démarrage
* Création du fichier `/etc/fail2ban/jail.local`
* Paramétrage du jail `sshd` :

  * `maxretry = 3`
  * `findtime = 10m`
  * `bantime = 1h`
* Redémarrage du service

### 🛡️ Résultat

Le serveur bloque automatiquement une IP après dépassement du seuil d’échecs.

📸 **Captures nécessaires :**

* `systemctl status fail2ban`
* Contenu du `jail.local`
* `fail2ban-client status sshd`

➡️ Justification : validation du bon fonctionnement du service et du jail actif.

---

# 3️⃣ Tests de bannissement et validation firewall

### 🎯 Objectif

Vérifier que le mécanisme fonctionne réellement.

### 🧪 Méthode

Depuis la machine cliente :

* Tentatives SSH répétées avec mauvais mot de passe

Côté serveur :

* Vérification des IP bannies via `fail2ban-client`
* Contrôle des règles dynamiques injectées dans `iptables`

### 🛡️ Résultat observé

* L’IP cliente apparaît dans la liste des IP bannies
* Une règle spécifique Fail2ban est visible dans la chaîne INPUT


* `fail2ban-client status sshd`
* `iptables -L -n -v | grep fail2ban`

➡️ Justification : preuve technique du bannissement effectif.

---

# 4️⃣ Mise en place d’un bannissement progressif (recidive)

### 🎯 Objectif

Sanctionner plus lourdement les IP récidivistes.

### ⚙️ Actions

* Activation du jail `recidive`
* Paramétrage : 3 bans en 24h → exclusion 1 semaine

### 🧠 Intérêt

Cette mesure permet d’augmenter progressivement la sévérité et de bloquer durablement les attaquants persistants.



* `fail2ban-client status recidive`

---

# 5️⃣ Bonus – Mise en place du Port-Knocking (Knockd)

### 🎯 Objectif

Masquer totalement le port SSH aux scanners réseau.

---

## 🔒 Étape 1 – Fermeture par défaut du port 22

* Réinitialisation des règles `iptables`
* Politique INPUT en DROP
* Autorisation loopback + connexions établies
* Blocage explicite du port 22

📸 **Capture nécessaire :**

* `iptables -L -n -v`

➡️ Justification : démonstration que SSH est bloqué.

---

## 🚪 Étape 2 – Configuration de Knockd

* Installation du service
* Configuration d’une séquence :

  * Ouverture : 7000 → 8000 → 9000
  * Fermeture : séquence inverse
* Injection dynamique de règle ACCEPT pour l’IP cliente

---

## 🧪 Tests réalisés

### Avant séquence

`nmap -p 22 IP_SERVEUR`

Résultat : `filtered`

### Après séquence

Port 22 : `open`

Connexion SSH fonctionnelle.

### Après fermeture

Port 22 : `filtered` de nouveau.

📸 **Captures nécessaires :**

* Scan Nmap avant knock
* Scan Nmap après knock
* Logs knockd dans `/var/log/syslog`

➡️ Justification : validation complète du mécanisme de dissimulation.

---

# ✅ Résultats obtenus

| Contrôle         | Résultat                              |
| ---------------- | ------------------------------------- |
| Lecture des logs | Activité brute force identifiée       |
| Fail2ban         | Bannissement automatique opérationnel |
| Règles firewall  | Générées dynamiquement                |
| Récidive         | Protection renforcée                  |
| Port-knocking    | SSH invisible sans séquence           |
| Séquence inverse | Fermeture effective                   |

---

# 🎓 Analyse professionnelle

Cette mise en œuvre permet :

* Une **réduction immédiate du bruit réseau**
* Une **automatisation de la réponse défensive**
* Une **diminution drastique de la surface d’exposition**
* Une approche combinant **détection + réaction + dissimulation**

On passe d’un serveur exposé naïvement à un serveur avec :

* Défense active (Fail2ban)
* Défense adaptative (Recidive)
* Défense furtive (Port-knocking)

C’est une architecture cohérente dans un contexte PME.

---

# ⚠️ Points de vigilance identifiés

* Toujours configurer `ignoreip` pour éviter l’auto-bannissement
* Conserver une session console active avant modification firewall
* Tester dans une session distincte avant fermeture
* Documenter la séquence knock dans un coffre sécurisé

---

