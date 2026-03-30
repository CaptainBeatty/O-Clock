
# Réinitialisation du mot de passe Administrateur local sous Windows 10 / 11

## ⚠️ Prérequis et avertissement

- Cette méthode fonctionne **uniquement pour les comptes locaux Windows**.
- Elle ne fonctionne pas pour :
  - Compte Microsoft
  - Active Directory
- À utiliser uniquement :
  - Sur votre propre machine
  - Ou avec autorisation explicite

---

## 🧠 Principe de la méthode

La technique consiste à :
1. Démarrer sur un environnement WinPE (installation Windows)
2. Remplacer `utilman.exe` par `cmd.exe`
3. Ouvrir une invite de commande depuis l’écran de connexion
4. Réinitialiser le mot de passe avec `net user`

👉 Cette méthode exploite l’accès système hors session pour modifier la base SAM (comptes locaux) :contentReference[oaicite:1]{index=1}

---

## 🔧 Étape 1 : Démarrage sur un support Windows

### Objectif
Accéder à l’environnement de récupération (WinPE)

### Procédure

1. Créer un support bootable :
   - Clé USB Windows
   - ISO monté (VM)

2. Démarrer dessus :
   - Accéder au **Boot Menu (F2 / F12 / ESC selon machine)**
   - Sélectionner :
     - Clé USB
     - Lecteur CD/ISO

3. Attendre l’écran d’installation Windows

---

## 🖥️ Étape 2 : Ouverture de l’invite de commandes

Sur l’écran d’installation :

```

SHIFT + F10

````

➡️ Une **invite de commande système** s’ouvre

---

## 🔁 Étape 3 : Remplacement de Utilman.exe

### Objectif
Détourner le bouton d’accessibilité pour lancer un shell admin

### Commandes :

```cmd
move c:\windows\system32\utilman.exe c:\windows\system32\utilman.exe.bak
copy c:\windows\system32\cmd.exe c:\windows\system32\utilman.exe
````

---

## 🔄 Étape 4 : Redémarrage

```cmd
wpeutil reboot
```

➡️ Le système redémarre normalement sur Windows

---

## 🔓 Étape 5 : Accès à l’invite de commande depuis l’écran de connexion

1. Sur l’écran de login
2. Cliquer sur l’icône **Accessibilité** (en bas à droite)

➡️ Une **invite de commande en mode SYSTEM** s’ouvre

---

## 🔑 Étape 6 : Réinitialisation du mot de passe

### Commande :

```cmd
net user <nom_utilisateur> *
```

➡️ Saisir le nouveau mot de passe

### Exemple :

```cmd
net user Administrateur *
```

---

## 🧹 Étape 7 : Restauration de Utilman.exe (IMPORTANT)

Rebooter à nouveau sur le support Windows

Ouvrir CMD (`SHIFT + F10`) puis :

```cmd
del c:\windows\system32\utilman.exe
move c:\windows\system32\utilman.exe.bak c:\windows\system32\utilman.exe
```

---

## ✅ Résultat

* Mot de passe réinitialisé
* Accès au compte restauré

---

## ⚡ Points de vigilance (niveau pro)

* Cette méthode montre une **faille de sécurité historique** liée à la base SAM ([IT-Connect][1])
* À contrer en environnement pro :

  * Chiffrement disque (BitLocker)
  * Désactivation boot externe
  * BIOS/UEFI protégé
  * Secure Boot activé

---

## 🧠 Conclusion

Cette technique est simple, rapide et efficace pour :

* Dépannage local
* Intervention terrain

Mais elle démontre surtout une chose :

👉 **Sans chiffrement disque, un accès physique = compromission totale.**

```


[1]: https://www.it-connect.fr/chapitres/restaurer-le-mot-de-passe-administrateur-local-de-windows/?utm_source=chatgpt.com "Restaurer le mot de passe Administrateur local de Windows"
