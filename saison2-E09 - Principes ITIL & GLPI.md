# 🧩 Intégration de GLPI Agent au serveur GLPI

*Version illustrée — environnement Linux + Windows*

---

## **1. Installation et configuration du serveur GLPI**

L’installation du serveur GLPI s’effectue sur une VM Linux (Ubuntu 24.04 dans ton cas), accessible depuis le réseau local.

**Interface d’installation de la base MySQL :**
![GLPI Setup](captures/glpi_setup.png)

> Le serveur SQL (`localhost`), l’utilisateur (`glpiuser`) et la base (`glpi`) sont configurés puis testés avec succès.

---

### Connexion réussie à la base de données :

![Connexion MySQL réussie](captures/ConnexionMySQLréussie.png)

> GLPI confirme la liaison avec la base et passe à l’étape de création des tables.

---

### Interface d’administration opérationnelle :

![GLPI Dashboard](captures/GLPI_Dashboard.png)

> Le tableau de bord de GLPI est maintenant accessible via :
> **[http://192.168.1.22/glpi](http://192.168.1.22/glpi)**

---

## **2. Installation de GLPI Agent sur une machine cliente (Windows)**

L’agent est téléchargé via le setup officiel (ex. *GLPI Agent 1.15 Target Setup*).
L’étape clé est la configuration du **Remote Target** :


> Dans “Remote Targets”, renseigner :
> `http://192.168.1.22/glpi`
> Cela permet à l’agent d’envoyer ses inventaires au serveur GLPI.

Une fois installé, le service **GLPI Agent** tourne en tâche de fond sous Windows.

---

## **3. Vérification du service et du log sur Windows**

Vérifie que le service fonctionne :

```powershell
Get-Service "GLPI Agent"
```

Et consulte le log :

```powershell
Get-Content "C:\Program Files\GLPI-Agent\logs\glpi-agent.log" -Tail 50
```

![Agent Windows Log](captures/Agent_Windows_Log.png)

> On y voit le lancement du service et la communication réussie avec le serveur GLPI.

---

## **4. Installation de GLPI Agent sur le serveur Linux**

Le fichier téléchargé est :
`glpi-agent-1.7.1-linux-installer.pl`

![Téléchargement GLPI Agent Linux](captures/Installation_GLPI_Agent_Linux.png)

Lancer ensuite l’installation :

```bash
cd ~/Téléchargements
chmod +x glpi-agent-1.7.1-linux-installer.pl
sudo ./glpi-agent-1.7.1-linux-installer.pl
```

---

### Durant l’installation :

> Saisir l’URL du serveur GLPI (ex. `http://127.0.0.1/glpi` si installé sur la même machine).
> Laisser vide le chemin local et valider.

---

### Vérification :

```bash
sudo systemctl status glpi-agent
```

Puis :

```bash
sudo glpi-agent --force --debug
```

> Cela force un inventaire immédiat.

---

## **5. Visualisation de l’inventaire dans GLPI**

Retour dans l’interface GLPI :
![GLPI Inventaire Ordinateurs](captures/GLPI_Inventaire_Ordinateurs.png)

> Le poste ou le serveur équipé de l’agent apparaît automatiquement dans **Parc → Ordinateurs**.
> GLPI détecte le matériel, le système, les logiciels, et les composants réseau.

---

## **6. Création d’un ticket depuis le poste Windows**

Depuis l’interface web GLPI, accède à :

Assistance → Créer un ticket

![GLPI Inventaire Ordinateurs](captures/GLPI_ticket1.png)

Exemple de ticket

Titre : Souris dysfonctionnelle

Description :

L’utilisateur signale un problème avec la souris, qui ne répond plus aux clics intermittents.
Le périphérique USB est reconnu mais ne fonctionne pas correctement.

![GLPI Inventaire Ordinateurs](captures/GLPI_ticket2.png)

Urgence : Moyenne

Demandeur : glpi

Attribué à : glpi

Le ticket apparaît dans la liste avec le statut En cours (Attribué), confirmant la communication complète entre l’interface utilisateur et le back-office GLPI.

## **7. Conclusion**

✅ **Résultat final :**

![GLPI Inventaire Ordinateurs](captures/GLPI_Dashboard_final.png)

* Le **serveur GLPI** est pleinement fonctionnel.
* Les **agents Windows et Linux** remontent correctement les informations d’inventaire.
* La synchronisation se fait automatiquement grâce au service GLPI-Agent.
* La remonté de ticket est fonctionnelle.

