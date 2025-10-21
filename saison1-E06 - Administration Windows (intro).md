# 🧠 TP – Installation de Debian 13 sous VirtualBox

## 🎯 Objectifs
- Installer **Debian 13** dans une machine virtuelle VirtualBox.  
- Configurer les **partitions**, **ajouter un utilisateur**, et **installer les Additions invitées**.  
- Installer quelques logiciels essentiels : **VLC**, **7-Zip**, **Okular**.

---

## 🏗️ 1. Création de la machine virtuelle

Lors de la création de la VM, j’ai choisi Debian 13 (64-bit) comme système d’exploitation, avec une installation non automatisée.

![Création de la machine virtuelle](captures/1creation%20vm%20choix%20de%20l'iso%20debian.png)

---

## 💿 2. Démarrage de l’installateur Debian

Au lancement, j’ai sélectionné l’option **Graphical install**, permettant une installation graphique complète.

![Écran d’installation Debian](captures/2installeur%20debian.png)

---

## ⚙️ 3. Partitionnement du disque

J’ai opté pour un partitionnement **assisté**, en utilisant **tout le disque** avec **LVM** et une **partition /home séparée**.

![Partitionnement assisté](captures/3partitonnement%20du%20disque.png)
<br><br>
![Choix du schéma de partition](captures/4partitonnement%20du%20disque2.png)
<br><br>
![Validation des partitions](captures/5partitonnement%20du%20disque3.png)

---

## 📦 4. Installation des Additions invitées VirtualBox

Une fois Debian installé, j’ai monté le disque d’Additions invitées via les paramètres de stockage VirtualBox.

![Montage du disque des Additions invitées](captures/6montage%20disque%20vbox%20additions.png)

Ensuite, j’ai exécuté le script `VBoxLinuxAdditions.run` depuis le terminal pour activer le copier/coller et le redimensionnement automatique de la fenêtre.

![Installation des Additions invitées](captures/7installation%20vbox%20additions.png)

---

## 👥 5. Création d’un nouvel utilisateur

Pour simuler un environnement multi-utilisateur, j’ai ajouté un second compte nommé `user` à l’aide de la commande :

```bash
adduser user
```
## 🧰 6. Installation de logiciels

Enfin, j’ai installé trois outils pratiques pour un usage courant :

```bash
apt install vlc p7zip-full okular
```

## ✅ Conclusion

La machine virtuelle Debian 13 est désormais fonctionnelle avec :

Les Additions invitées VirtualBox actives,

Un nouvel utilisateur opérationnel,

Des applications de base installées.
