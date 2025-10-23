# 🧰 Étape 1 — Réparation complète du système de Mme Michu  
*Challenge : récupération et restauration du démarrage sous Windows 10 (machine virtuelle Oracle VirtualBox).*

---

## 🔹 1. Importation et préparation de la machine
![Téléchargement de la VM](captures/1-download_VM.png)  
![Importation de la VM](captures/2-import_VM_start.png)  
![Importation terminée](captures/3-import_VM_ok.png)  
La machine virtuelle a été importée dans VirtualBox puis protégée par un **instantané initial** avant toute modification :  
![Snapshot initial](captures/4-snapshot_before_start_vm.png)

---

## 🔹 2. Erreur de démarrage et diagnostic initial
![Erreur de démarrage](captures/5-error_starting.png)  
Le système ne démarre pas. Après plusieurs tentatives, la VM est redémarrée sur un **support ISO Windows 10** pour accéder à l’environnement de réparation.  
![Arrêt puis redémarrage sur ISO](captures/6-shutdown_again.png)  
![Ajout de l’ISO Windows 10](captures/7-to_add_iso.png)  
![Redémarrage de la VM](captures/8-To_restart_again.png)  
![Menu d’installation Windows](captures/9-Windows_installation.png)  
Choix de **"Réparer l’ordinateur"** au lieu de procéder à une installation :  
![Choix de la réparation](captures/10-choose_repair.png)

---

## 🔹 3. Accès à l’environnement de récupération
![Réparation automatique échouée](captures/11-choose_depannage.png)  
![Options avancées](captures/12-choose_restart_tools.png)  
![Ouverture de l’invite de commandes](captures/13-security_snapshot.png)  
Exploration des partitions via **DiskPart** :  
![Analyse des volumes](captures/14.png)  
Les volumes système sont identifiés :  
- `E:` → Windows  
- `C:` → Partition réservée au système  
- `F:` → Partition masquée  

---

## 🔹 4. Réparation du MBR
![Fix MBR](captures/15-try_to_repair.png)  
```cmd
bootrec /fixmbr

📸 Capture : 19-diskpart_sequence.png

On repère la partition contenant Windows (ici, la partition E:).

17️⃣ Vérification du contenu du disque

Avant d’agir sur le secteur de démarrage, un rapide listage permet de confirmer la présence du répertoire Windows sur le volume identifié.

E:
dir

📸 Capture : 20-to_repair.png

