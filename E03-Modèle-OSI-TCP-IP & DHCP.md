# 🧩 Challenge Réseau — Plan d’adressage IP

## 🎯 Objectif

Vous êtes recruté par une grande entreprise qui souhaite **refaire complètement son réseau informatique**.  
L’entreprise est répartie sur **deux sites distincts : Montpellier et Bordeaux**.

L’objectif est de **proposer un plan d’adressage** clair et structuré en sous-réseaux indépendants pour chaque type de machine et chaque usage réseau.

---

## 🏢 Description de l’infrastructure

### 🌍 Site de Montpellier

| Type d’équipement | Quantité |
|--------------------|-----------|
| PC fixes           | 200       |
| PC portables       | 70        |
| Serveurs           | 20        |
| Copieurs           | 15        |

### 🌍 Site de Bordeaux

| Type d’équipement | Quantité |
|--------------------|-----------|
| PC fixes           | 100       |
| PC portables       | 40        |
| Serveurs           | 5         |
| Copieurs           | 5         |

---

## 📶 Réseaux WiFi

Sur **chaque site**, deux réseaux WiFi distincts doivent être mis en place :

- **WiFi Public** : réservé aux visiteurs  
- **WiFi Privé** : réservé aux collaborateurs (PC portables non connectés en filaire)

---

## 🔐 Exigences

- Les machines doivent être **cloisonnées** dans des **sous-réseaux indépendants** pour des raisons de sécurité.  
- Chaque site comportera donc **5 sous-réseaux** :
  1. PC fixes / portables en filaire  
  2. Serveurs  
  3. Copieurs  
  4. WiFi public  
  5. WiFi privé  

---

## 🗺️ Plan d’adressage proposé

### 🏢 Bureau de Montpellier

| Sous-réseau         | Adresse réseau      | Masque CIDR | Masque décimal     | Hôtes disponibles |
|---------------------|--------------------:|-------------:|-------------------:|------------------:|
| PC (fixes/portables) | 192.168.0.0        | /22          | 255.255.252.0      | 1022              |
| Serveurs             | 192.168.4.0        | /25          | 255.255.255.128    | 126               |
| Copieurs             | 192.168.4.128      | /26          | 255.255.255.192    | 62                |
| WiFi Public          | 192.168.8.0        | /22          | 255.255.252.0      | 1022              |
| WiFi Privé           | 192.168.12.0       | /22          | 255.255.252.0      | 1022              |

---

### 🏢 Bureau de Bordeaux

| Sous-réseau         | Adresse réseau      | Masque CIDR | Masque décimal     | Hôtes disponibles |
|---------------------|--------------------:|-------------:|-------------------:|------------------:|
| PC (fixes/portables) | 192.168.16.0       | /23          | 255.255.254.0      | 510               |
| Serveurs             | 192.168.18.0       | /26          | 255.255.255.192    | 62                |
| Copieurs             | 192.168.18.64      | /27          | 255.255.255.224    | 30                |
| WiFi Public          | 192.168.20.0       | /23          | 255.255.254.0      | 510               |
| WiFi Privé           | 192.168.22.0       | /23          | 255.255.254.0      | 510               |

---

## 📘 Remarques

- Le plan d’adressage est conçu pour **anticiper la croissance du parc informatique** sur les prochaines années.  
- Chaque sous-réseau dispose d’un **nombre d’adresses supérieur aux besoins actuels** pour éviter toute saturation future.  
- Les masques plus petits (ex : /25, /26, /27) permettent de **limiter le broadcast** et d’optimiser la sécurité entre les segments réseau.  

---

📁 Les schémas du plan d’adressage et les captures associées seront stockés dans le dossier :  
`/captures`
