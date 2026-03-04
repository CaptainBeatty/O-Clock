# Mini-PRA / PCA — Incident majeur

## 1. Scénario d’incident

**Incident majeur :**  
Panne totale du contrôleur de domaine Active Directory (DC unique).

**Cause :**  
Erreur humaine entraînant une coupure électrique du serveur.

---

## 2. Périmètre impacté

- Authentification utilisateurs indisponible  
- Accès aux ressources réseau impossible  
- Imprimantes réseau inaccessibles  
- Wi-Fi sécurisé potentiellement indisponible  

---

## 3. Impact métier

- **Apprenants :** ouverture de session impossible  
- **Formateurs :** accès aux ressources pédagogiques impossible  
- **Personnel :** services réseau dégradés  

---

## 4. Hypothèses de départ

On considère que :

- Les locaux techniques sont accessibles  
- L’incident est une coupure électrique sans dommage matériel  
- Une sauvegarde récente est disponible  
- Les ressources IT sont disponibles  

---

## 5. Mesures préventives

### Mesures techniques

- Sauvegardes régulières  
- Snapshots système  
- Supervision de l’infrastructure  
- Documentation technique  

### Mesures organisationnelles

- Procédures d’exploitation  
- Étiquetage des prises  
- Sensibilisation des utilisateurs  

### Mesures de continuité

- Second contrôleur de domaine  
- Tests réguliers du PRA  
- Plan de communication  

---

## 6. Procédures de reprise

### Actions immédiates (diagnostic)

- Vérification des alertes de supervision  
- Analyse des journaux système  
- Contrôle physique du serveur  
- Vérification de l’alimentation électrique  

---

### Actions de reprise

- Remise sous tension du serveur  
- Vérification du redémarrage  
- Contrôle des services AD / DNS / DHCP  
- Validation via la supervision  

---

### Contrôles de validation

- Test d’authentification utilisateurs  
- Vérification des accès réseau  
- Vérification Wi-Fi / LAN  
- Vérification des services d’impression  
- Analyse préventive des journaux système  

---

## 7. Responsable et délai de restauration

**Responsable :**  
Équipe IT

**RTO cible :**  
1 heure

### Ordre de priorité

1. Services d’infrastructure (AD / DNS / DHCP)  
2. Accès réseau (Wi-Fi / LAN)  
3. Services utilisateurs  

---

## 8. Indicateurs de succès

- Services AD / DNS / DHCP opérationnels  
- Authentification fonctionnelle  
- Accès aux ressources validé  
- Connectivité réseau stable  
- Supervision et sauvegardes actives  
- Absence d’erreurs critiques dans les journaux système  
