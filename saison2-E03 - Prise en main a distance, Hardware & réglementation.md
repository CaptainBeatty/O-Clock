
# Challenge – Prise en main à distance avec AnyDesk

## Objectif
Mettre en œuvre et tester une connexion à distance entre deux machines virtuelles :
- **Cible** : Ubuntu (VM)
- **Initiateur** : Windows 11 (VM)

Logiciel utilisé : **AnyDesk** (version testée : 7.1.x)

---

## Matériel / environnement
- Hôte : poste de travail (machine physique) exécutant l’hyperviseur (VirtualBox / VMware selon le lab)
- VM cible : **Ubuntu** (desktop)
- VM initiatrice : **Windows 11**
- Réseau : NAT / Host-Only selon configuration de labo (les captures ci-dessous montrent une connexion interne fonctionnelle entre VMs)

---

## Contenu du dossier `captures/`
(Noms d’images tels qu’utilisés dans le document)
- `1-download_anydesk on the vm for test.png`  
- `2-install and start app.png`  
- `3-install and start app on another vm.png`  
- `4-check the address number on the target.png`  
- `5-paste the adress on windows 11 vm.png`  
- `6-remote connection on vm linux.png`  

---

## Étapes réalisées (chronologie)

### 1. Téléchargement
Depuis la VM **Ubuntu**, téléchargement du paquet AnyDesk depuis le site officiel.
![Téléchargement d’AnyDesk](captures/1-download_anydesk%20on%20the%20vm%20for%20test.png)

### 2. Installation et lancement sur la VM Ubuntu
- Extraction du tarball / exécution du script `install.sh` fourni dans l’archive.
- Lancement de l’application AnyDesk (interface affichée dans l’environnement GNOME).
![Installation et démarrage d’AnyDesk](captures/2-install%20and%20start%20app.png)

> Commande typique (exemple) :
> ```bash
> tar -xzf anydesk-7.1.0.tar.gz
> cd anydesk-7.1.0
> sudo ./install.sh
> anydesk &      # lancer l'app en arrière-plan
> ```

### 3. Vérification de l’application et de l’ID
- Vérification que l’application AnyDesk est lancée et que l’**ID AnyDesk** s’affiche sur la VM cible (Ubuntu).
![Application lancée sur la VM Linux](captures/3-install%20and%20start%20app%20on%20another%20vm.png)

### 4. Récupération de l’adresse AnyDesk
- Relevé du code / adresse de la machine cible (ex. `1 352 590 816`).
![Adresse AnyDesk sur la cible](captures/4-check%20the%20address%20number%20on%20the%20target.png)

### 5. Connexion depuis Windows 11
- Sur la VM **Windows 11**, ouverture d’AnyDesk.
- Saisie de l’adresse de la cible et envoi de la requête de connexion.
![Saisie de l'adresse sur Windows](captures/5-paste%20the%20adress%20on%20windows%2011%20vm.png)

### 6. Acceptation et session à distance
- Acceptation de la connexion sur la VM Ubuntu (via la fenêtre d’autorisation AnyDesk).
- Contrôle à distance établi — vérification du contrôle clavier/souris et de l’affichage (capture montrant la vue distante depuis Windows).
![Session distante active](captures/6-remote%20connection%20on%20vm%20linux.png)

---

## Observations et points notables
- **Permissions** : sur Linux desktop, AnyDesk peut demander des permissions supplémentaires (ex. accès à l’écran, contrôle) selon la version et le gestionnaire de fenêtres. Vérifier les popups et les paramètres système.
- **Licence** : l’application signale la licence « Free (non-professional use) ». Pour usage professionnel / multi-postes, prévoir licence.
- **Qualité / latence** : la qualité dépend fortement des ressources allouées aux VMs et de la latence réseau. Ajuster la qualité d’image dans les paramètres AnyDesk si nécessaire.
- **Menu et fonctions** : transfert de fichiers, enregistrement de session, Wake-On-LAN (si activé), carnet d’adresses (my.anydesk) — Certaines fonctions sont limitées selon la licence.

---

## Checklist sécurité / conformité (à valider avant toute prise en main réelle)
- [ ] Obtenir **consentement explicite** de l’utilisateur final (acceptation visible sur la machine cible).
- [ ] Vérifier que la session respecte la **politique interne** (journalisation, trace des actions).
- [ ] Ne pas accéder à des données sensibles sans autorisations additionnelles.
- [ ] S’assurer que la version d’AnyDesk utilisée est à jour (corriger vulnérabilités connues).
- [ ] Bloquer/filtrer l’application via pare-feu si usage non autorisé (contrôler sorties réseau nécessaires).
- [ ] Conserver capture d’écran / log de session si la politique l’exige (prévenir l’utilisateur).
- [ ] Vérifier le chiffrement des connexions (AnyDesk utilise son propre chiffrement ; documenter pour la conformité).

---

## Partie 2 – Diagnostic matériel : test de la mémoire avec MemTest86

### Objectif
Vérifier l’intégrité de la mémoire vive de la machine virtuelle Windows 10 à l’aide de **PassMark MemTest86**.

---

### Étapes de réalisation

#### 1. Téléchargement de MemTest86
Téléchargement de la version **Free v11.5** depuis le site officiel.  
![Téléchargement MemTest86](captures/1-test_memoire-download_memtest86.png)

#### 2. Création d’un support USB virtuel
Dans **VirtualBox**, ajout d’un disque virtuel simulant une clé USB (`oclock_flah_usb_drive.vdi`).  
![Création du stockage USB virtuel](captures/2-creation%20stockage%20virtuelle%20usb%20susr%20virtualbox.png)

#### 3. Flash de la clé USB
L’image ISO fournie par MemTest86 est **flashée** sur le disque virtuel (méthode manuelle via un outil de gravure ou copie directe sur le VDI).

#### 4. Redémarrage sur la clé USB
Redémarrage de la VM sur le média virtuel contenant MemTest86 (boot USB activé dans l’ordre de démarrage).  
![Démarrage sur la clé USB](captures/Redemarrage%20sur%20clé%20usb%20+%20mem%20test.png)

#### 5. Exécution du test
MemTest86 effectue automatiquement plusieurs passes de test mémoire :
- **CPU détecté** : AMD Ryzen AI 9 HX 370  
- **Nombre de passes** : 4  
- **Erreurs** : 0  
- **Résultat final** : mémoire stable et conforme  
![Test mémoire en cours](captures/3-memtest86-running.png) *(nom à adapter selon ton dossier)*

---

### Interprétation des résultats
- **Pass = OK** : aucun secteur mémoire défectueux détecté.  
- **Errors > 0** : modules de RAM potentiellement instables ou défectueux.  

> Dans ce cas de test, **aucune erreur n’a été détectée** après 4 passes.  
> Le module virtuel fonctionne correctement et la VM dispose d’une mémoire fiable.

---

## Conclusion générale
Les deux exercices du challenge ont permis :
- De **valider la prise en main à distance** entre deux systèmes hétérogènes (Windows ↔ Ubuntu) via AnyDesk.  
- De **vérifier la fiabilité de la mémoire** grâce à un outil de diagnostic bas-niveau.  

Ces manipulations s’inscrivent dans les missions typiques d’un **technicien support IT**, combinant assistance à distance, diagnostic matériel et bonnes pratiques de sécurité.
