# Atelier SA2 – Remise en service de la machine de Mme Michu  
**Étape 1 : Diagnostic et tentative de réparation de la machine virtuelle**

---

## 🧩 Objectif

L’objectif de cette première étape est de diagnostiquer la panne rencontrée sur la machine virtuelle de Mme Michu et de tenter une remise en service du système sans réinstallation complète.  
Le travail a été réalisé dans un environnement **VirtualBox**, à partir d’une machine Windows corrompue au démarrage.

---

## ⚙️ Déroulement du TP

Après le téléchargement de la machine virtuelle (`1-download_VM.png`), celle-ci est importée dans VirtualBox puis démarrée (`2-import_VM_start.png`).  
Dès le lancement, le système affiche un échec au démarrage, indiquant la nécessité d’une réparation du boot.

L’assistant de récupération est ouvert via les outils de démarrage avancé (`10-choose_repair.png` à `12-choose_restart_tools.png`).  
Plusieurs essais de réparation automatique sont effectués, sans succès (`13-security_snapshot.png` à `16-it_doesnt_work.png`).

---

## 🧰 Intervention en mode console

Un accès à la console de récupération est alors lancé (`17-return_to_the_prompt.png` et `18-into_terminal_X.png`).  
Les commandes de diagnostic disque sont testées :

- `diskpart` pour identifier les partitions (`19-diskpart_sequence.png`),  
- puis `chkdsk E:` et `chkdsk C:` pour vérifier les volumes et tenter des corrections (`24-chkdsk_E.png`, `25-chkdsk_C.png`).

Les outils de réparation du secteur d’amorçage sont ensuite utilisés, via :

- `bootsect /nt60 SYS` (`26-bootsect_cmd.png`).

Malgré ces tentatives, les réparations ne permettent pas de rétablir un démarrage complet, ce qui mène à la prise d’un nouvel instantané pour sauvegarder l’état avant redémarrage (`27-snap_before_reboot.png`).

---

## 🖼️ Captures d’écran

Toutes les étapes sont documentées ci-dessous :

| Étape | Capture |
|:------|:--------|
| Téléchargement de la VM | ![Download VM](captures/1-download_VM.png) |
| Import et démarrage de la VM | ![Import VM](captures/2-import_VM_start.png) |
| Accès aux outils de réparation | ![Choose repair](captures/10-choose_repair.png) |
| Sélection du dépannage | ![Choose depannage](captures/11-choose_depannage.png) |
| Relance des outils système | ![Restart tools](captures/12-choose_restart_tools.png) |
| Création d’un snapshot sécurité | ![Security snapshot](captures/13-security_snapshot.png) |
| Tentatives de réparation | ![Try repair](captures/15-try_to_repair.png) |
| Retour console | ![Return prompt](captures/17-return_to_the_prompt.png) |
| Séquence DiskPart | ![DiskPart](captures/19-diskpart_sequence.png) |
| Vérifications de disque | ![CHKDSK E](captures/24-chkdsk_E.png) / ![CHKDSK C](captures/25-chkdsk_C.png) |
| Réparation du secteur de démarrage | ![Bootsect](captures/26-bootsect_cmd.png) |
| Snapshot avant redémarrage | ![Snapshot final](captures/27-snap_before_reboot.png) |

---

## 🧾 Bilan de l’étape 1

Cette première phase a permis de comprendre la nature du problème :  
la table de démarrage et certaines partitions semblent corrompues.  
Les réparations automatiques ont échoué, mais les commandes `chkdsk` et `bootsect` ont permis de restaurer partiellement les volumes, posant les bases de la prochaine étape de restauration.

---
