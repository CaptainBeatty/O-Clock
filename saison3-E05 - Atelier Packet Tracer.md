
# 🧩 S03 – Atelier Packet Tracer

## Objectif
Réaliser une topologie complète multi-sites entre **Paris** et **Lille** avec interconnexion, routage statique, DHCP et Wi-Fi fonctionnel.  
Le projet repose sur **Cisco Packet Tracer** et vise à simuler une architecture réseau d’entreprise.

---

## 🗺️ Étape 1 – Plan d’adressage

### Exercice : adressage réseau

| Site | Sous-réseau | Masque | Description |
|------|--------------|--------|--------------|
| Paris / LAN | 192.168.1.0 | /24 | 510 hôtes |
| Paris / DMZ | 192.168.3.0 | /24 | Serveurs |
| Paris / WiFi public | 192.168.4.0 | /24 | 1022 hôtes |
| Lille / LAN | 192.168.2.0 | /23 | 510 hôtes |
| Lille / WiFi public | 192.168.5.0 | /22 | 1022 hôtes |

---

## 🔌 Étape 2 – Câblage

Schéma global de l’interconnexion entre les deux sites, incluant le VPN, la DMZ et les points d’accès Wi-Fi.

![Topologie réseau](./captures/Pasted%20image%2020251109181100.png)

---

## ⚙️ Étape 3 – Configuration des switchs

Exemple sur **Switch3 (Paris)** :

```bash
enable
configure terminal
hostname Switch3
interface Vlan1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
show ip interface brief
enable secret admin
service password-encryption
copy running-config startup-config
````

Même principe appliqué à chaque switch du réseau.

![Switch Paris](./captures/Pasted%20image%2020251109170939.png)

---

## 🛠️ Étape 4 – Configuration initiale des routeurs

Exemple pour **Router Paris** :

```bash
Router>enable
Router#configure terminal
Router(config)#hostname routerParis
Router(config)#enable secret admin

# Interface LAN
Router(config)#int g0/0
Router(config-if)#ip address 192.168.1.1 255.255.255.0
Router(config-if)#no shutdown

# Interface DMZ
Router(config)#int g0/1
Router(config-if)#ip address 192.168.3.1 255.255.255.0
Router(config-if)#no shutdown

# Lien vers Lille
Router(config)#int s0/0/1
Router(config-if)#ip address 92.56.78.1 255.255.255.0
Router(config-if)#no shutdown

# Lien vers DMZ
Router(config)#int s0/0/0
Router(config-if)#ip address 92.12.34.1 255.255.255.0
Router(config-if)#no shutdown

copy running-config startup-config
```

---

## 🧭 Étape 5 – Routage statique

**Router Paris :**

```bash
ip route 192.168.2.0 255.255.255.0 92.12.34.2
copy run sta
```

**Router Lille :**

```bash
ip route 192.168.1.0 255.255.255.0 92.12.34.1
copy run sta
```

**Router DMZ :**

```bash
ip route 192.168.3.0 255.255.255.0 92.12.34.1
```

📺 [Vidéo de référence – Routage statique Cisco](https://www.youtube.com/watch?v=v2-DWSyJ4Rg)

---

## 💡 Étape 6 – DHCP

Exemple de configuration sur **Router Lille :**

```bash
ip dhcp pool LAN2
network 192.168.2.0 255.255.255.0
dns-server 8.8.8.8
default-router 192.168.2.1
```

> ⚠️ Si le DHCP ne distribue pas correctement les adresses, il faut **renouveler le bail** sur les postes :
>
> ```
> ipconfig /release
> ipconfig /renew
> ```

![Laptop configuré en DHCP](./captures/Pasted%20image%2020251109120724.png)
![Serveur DHCP central](./captures/Pasted%20image%2020251109182723.png)

📘 Référence : [Configurer le service DHCP sur un routeur Cisco – IT-Connect](https://www.it-connect.fr/configurer-le-service-dhcp-sur-un-routeur-cisco/)

---

## 📶 Bonus – Configuration Wi-Fi

### Point d’accès – Lille

![Point d’accès Lille](./captures/Pasted%20image%2020251109182723.png)
![Connexion laptop Wi-Fi](./captures/Pasted%20image%2020251109182505.png)
![Ajout de la carte Wi-Fi](./captures/Pasted%20image%2020251109182422.png)
![Réseau sans fil configuré](./captures/Pasted%20image%2020251109182719.png)

**Détails :**

* Authentification : WPA2-PSK
* Clé : `passwordlille`
* Chiffrement : AES
* Canal : 6
* Port : activé

Le laptop est ensuite connecté en DHCP via Wi-Fi.

---

## 🧠 Méga bonus – DHCP centralisé à Paris

1. Suppression des anciens pools DHCP sur Paris et Lille :

   ```bash
   no ip dhcp pool LAN1
   no ip dhcp pool LAN2
   ```

2. Ajout des **IP helper-address** :

   ```bash
   interface g0/0
   ip helper-address 192.168.3.20
   ```

   Et côté Lille :

   ```bash
   interface g0/0
   ip helper-address 192.168.3.20
   ip helper-address 92.12.34.1
   ```

3. Activation du service DHCP sur le serveur central :

   * Pool **LAN_Paris**
   * Pool **LAN_Lille**
   * DNS : 8.8.8.8
   * Masque : 255.255.255.0

![Serveur DHCP centralisé](./captures/Pasted%20image%2020251109182723.png)

> Le DHCP centralisé fonctionne désormais pour les deux sites.

---

## 🧾 Conclusion

L’atelier *Packet Tracer* met en œuvre :

* le cloisonnement réseau entre sites et zones fonctionnelles (LAN, DMZ, Wi-Fi, VPN),
* la configuration complète des équipements Cisco,
* la mise en place d’un **DHCP centralisé**,
* et la **connectivité Wi-Fi sécurisée** via WPA2-PSK.

Le réseau est entièrement opérationnel et respecte les exigences d’un environnement d’entreprise segmenté et sécurisé.



