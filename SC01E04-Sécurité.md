Mini-PRA / PCA — Incident majeur
1. Scénario d’incident

Panne totale du contrôleur de domaine Active Directory (DC unique).

Cause :
Erreur humaine entraînant une coupure électrique du serveur.

2. Périmètre impacté

Authentification utilisateurs indisponible

Accès aux ressources réseau impossible

Imprimantes réseau inaccessibles

Wi-Fi sécurisé potentiellement indisponible

3. Impact métier

Apprenants : ouverture de session impossible

Formateurs : accès aux ressources pédagogiques impossible

Personnel : services réseau dégradés

4. Hypothèses de départ

On considère que :

Les locaux techniques sont accessibles

L’incident est une coupure électrique sans dommage matériel

Une sauvegarde récente est disponible

Les ressources IT sont disponibles

5. Mesures préventives
Techniques

Sauvegardes régulières

Snapshots système

Supervision de l’infrastructure

Documentation technique

Organisationnelles

Procédures d’exploitation

Étiquetage des prises

Sensibilisation utilisateurs

Continuité

Second contrôleur de domaine

Tests réguliers du PRA

Plan de communication

6. Procédures de reprise
Actions immédiates

Vérification des alertes de supervision

Analyse des journaux système

Contrôle physique du serveur

Vérification alimentation électrique

Actions de reprise

Remise sous tension du serveur

Vérification du redémarrage

Contrôle des services AD / DNS / DHCP

Validation via supervision

Contrôles de validation

Test d’authentification utilisateurs

Vérification des accès réseau

Vérification Wi-Fi / LAN

Vérification impression

Analyse préventive des logs

7. Responsable et délai

Responsable :
Équipe IT

RTO cible :
1 heure

Priorités :

Services d’infrastructure (AD / DNS / DHCP)

Accès réseau (Wi-Fi / LAN)

Services utilisateurs

8. Indicateurs de succès

Services AD / DNS / DHCP opérationnels

Authentification fonctionnelle

Accès ressources validé

Connectivité stable

Supervision / sauvegardes actives

Absence d’erreurs critiques logs
