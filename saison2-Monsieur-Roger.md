# 🧰 TP – Monsieur Roger

## 🔗 Consignes du challenge
[Voir le sujet sur Kourou](https://kourou.oclock.io/ressources/recap-quotidien/aldebaran-technicien-support-it-ebonus-monsieur-roger/)

---

## ⚙️ Étape 1 – Incident : Résoudre le problème de connexion (mot de passe)

Pour commencer, téléchargement de **Hiren’s Boot CD** afin de contourner le mot de passe Windows et restaurer l’accès à la session.

L’accès est rétabli avec succès.

![Hiren’s Boot](./captures/Pasted%20image%20202511101130830%20-%20Copie.png)

---

## 🌐 Étape 2 – Incident : Réparer le périphérique réseau

Le problème venait de la carte réseau désactivée.  
Il suffit de la **réactiver** depuis le panneau des connexions réseau.

![Activation de la carte réseau](./captures/Pasted%20image%2020251110111422%20-%20Copie.png)

Une fois activée, on effectue un test de connectivité :

```powershell
ping 8.8.8.8
````

Résultat : la communication est bien établie.

![Ping réussi](./captures/Pasted%20image%20202511101114422%20-%20Copie.png)

---

## 🧩 Étape 3 – Incident : Récupérer les images perdues avec Recuva

Téléchargement du logiciel **Recuva** depuis le site officiel :

![Téléchargement Recuva](./captures/Pasted%20image%20202511101114739%20-%20Copie.png)

L’outil est exécuté et le dossier *Images* est scanné :

![Scan Recuva](./captures/Pasted%20image%2020251110120808%20-%20Copie.png)

Aucun fichier supprimé pertinent n’a été retrouvé, les images d’origine étaient déjà présentes.

---

## 💾 Étape 4 – Demande : Ajouter l’historique de fichiers

Depuis le **Panneau de configuration → Système et sécurité → Historique des fichiers**,
l’option est activée afin d’assurer la sauvegarde automatique des fichiers personnels.

![Historique des fichiers](./captures/Pasted%20image%2020251110120938%20-%20Copie.png)

---

## 🛠️ Étape 5 – Demande : Créer un point de restauration

Recherche dans la barre Windows : **"point de restauration"**
Puis création manuelle d’un point.

![Création du point](./captures/Pasted%20image%2020251110120959%20-%20Copie.png)

Une fois créé, Windows confirme l’opération :

![Point de restauration créé](./captures/Pasted%20image%2020251110121501%20-%20Copie.png)

On peut ensuite le visualiser dans la liste des points existants :

![Liste des points de restauration](./captures/Pasted%20image%2020251110130830%20-%20Copie.png)

---

## ✅ Conclusion

Toutes les demandes et incidents ont été traités :

* ✅ Restauration d’accès avec Hiren’s Boot
* ✅ Réactivation du périphérique réseau
* ✅ Vérification de connectivité (ping)
* ✅ Vérification des fichiers avec Recuva
* ✅ Activation de l’historique de fichiers
* ✅ Création d’un point de restauration

**Système de Monsieur Roger pleinement fonctionnel.**

```

```

