# 🧩 TP – Diagnostic et résolution d’incidents réseau  
### Module : Technicien Support IT – E02  
### Sujet : Diagnostic et résolution d’incidents sous VirtualBox

---

## 🎯 Objectif du TP
Vérifier la connectivité entre la **machine hôte** et une **machine virtuelle Windows 10** hébergée sous VirtualBox, puis identifier et résoudre les causes empêchant la communication réseau (ping).

---

## 🧱 Étapes réalisées

### 1. Démarrage de la machine virtuelle
Lancement de la VM **oclock_vm_win10** dans VirtualBox, configurée comme poste client pour ce test.

![Ouverture de la VM Windows 10](captures/1ouverture%20VM%20win10.png)

---

### 2. Vérification de la configuration IP
Depuis PowerShell, la commande `ipconfig` a permis d’obtenir les paramètres réseau initiaux :

- Adresse IPv4 : `10.0.2.15`  
- Masque : `255.255.255.0`  
- Passerelle : `10.0.2.2`

![Vérification de l’adresse IP de la machine](captures/2vérification%20ip%20de%20la%20machine.png)

---

### 3. Premier test de connectivité
Un **ping** a été envoyé depuis la machine hôte vers l’adresse `10.0.2.15`.

➡️ **Résultat : échec**, aucun paquet n’a été reçu.

![Ping initial entre l’hôte et la VM (échec)](captures/3ping%20hote%20vers%20vm.png)

---

### 4. Analyse et reconfiguration du réseau VirtualBox
Vérification des paramètres réseau de la VM : la carte était en **mode NAT**, ce qui isole la machine virtuelle du réseau local.  
Le mode d’accès a été modifié vers **Accès par pont (Bridged Adapter)** afin de permettre la communication directe entre l’hôte et la VM.

![Configuration réseau actuelle (NAT)](captures/4config%20reseau%20actuelle.png)  
![Ajout d’un nouvel adaptateur en mode pont](captures/5ajout%20d'un%20nouvelle%20adaptateur.png)

---

### 5. Autorisation du ping dans le pare-feu Windows
Dans le **Pare-feu Windows Defender avec fonctions avancées de sécurité**, les règles ICMPv4 entrantes ont été activées pour permettre les requêtes de type ping.

![Configuration du pare-feu Windows pour autoriser le ping](captures/6config%20firewall%20vm.png)

---

### 6. Vérification de la nouvelle adresse IP
Après reconfiguration, une nouvelle adresse IPv4 a été attribuée :  
- Adresse IPv4 : `192.168.1.16`  
- Passerelle : `192.168.1.1`

![Nouvelle adresse IP de la VM](captures/7recup%20ip%20reseau%20vm%20pour%20ping.png)

---

### 7. Test final de connectivité
Un nouveau **ping** a été effectué depuis la machine hôte vers `192.168.1.16`.

➡️ **Résultat : succès**, 4 paquets envoyés et reçus, aucune perte.

![Ping réussi entre l’hôte et la VM Windows 10](captures/8ping%20ip%20hote%20vers%20vm%20joignable.png)

---

## ✅ Conclusion
Le problème initial provenait du **mode NAT**, qui empêche la communication directe entre la machine hôte et la VM.  
Pour rétablir la connectivité :
1. Le mode réseau a été modifié en **Accès par pont**.  
2. Le pare-feu Windows a été configuré pour autoriser les requêtes ICMP entrantes.  
3. Une vérification IP et un nouveau test ping ont confirmé la réussite de la connexion.

La machine virtuelle Windows 10 est désormais **joignable depuis l’hôte**.

---
