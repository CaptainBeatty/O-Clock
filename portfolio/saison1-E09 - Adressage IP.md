# TP - Calculs d’adresses IP et sous-réseaux

## Objectif
Savoir déterminer le réseau, le broadcast, la plage d’hôtes et le nombre d’hôtes utilisables à partir d’une adresse IP donnée, en notation classique ou CIDR.

---

## Exercice 1 — 192.168.13.67/24
**Remarque :**
Explication concernant le numéro CIDR /24.  
Il y a 24 uns dans les 4 octets : `11111111.11111111.11111111.00000000`  
soit un masque = `255.255.255.0`, étant donné que 255 = 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1.

**Résultats :**
- Réseau : 192.168.13.0  
- Broadcast : 192.168.13.255  
- Hôtes utilisables (nombre) : 2^(32 − 24) − 2 = 254  
- Plage d’hôtes : 192.168.13.1 → 192.168.13.254  

---

## Exercice 2 — 172.16.0.1 /24
**Remarque :**
Le masque `255.255.255.0` signifie que le découpage s’applique sur le 4ᵉ octet.  
L’adresse réseau est donc 172.16.0.0 (et non 172.16.1.0), car le 3ᵉ octet reste inchangé avec un /24.

**Résultats :**
- Réseau : 172.16.0.0  
- Broadcast : 172.16.0.255  
- Hôtes utilisables : 254  
- Plage d’hôtes : 172.16.0.1 → 172.16.0.254  

---

## Exercice 3 — 172.16.27.32 /23
**Remarque :**
Il y a 23 bits à 1 : `11111111.11111111.11111110.00000000` → masque `255.255.254.0`  
Nombre magique = 256 − 254 = 2  
Le multiple de 2 inférieur à 27 est 26.

**Résultats :**
- Réseau : 172.16.26.0  
- Broadcast : 172.16.27.255  
- Hôtes utilisables : 2^(32 − 23) − 2 = 510  
- Plage d’hôtes : 172.16.26.1 → 172.16.27.254  

---

## Exercice 4 — 10.7.5.1 /17
**Remarque :**
Masque `255.255.128.0` → 17 bits réseau.  
Nombre magique = 256 − 128 = 128.  
Le 3ᵉ octet de l’adresse (5) se situe dans le bloc 0–127.

**Résultats :**
- Réseau : 10.7.0.0  
- Broadcast : 10.7.127.255  
- Hôtes utilisables : 2^(32 − 17) − 2 = 32 766  
- Plage d’hôtes : 10.7.0.1 → 10.7.127.254  

---

## Exercice 5 — 10.42.0.82 /12
**Remarque :**
12 bits à 1 : `11111111.11110000.00000000.00000000` → masque `255.240.0.0`.  
Nombre magique = 256 − 240 = 16.  
Octet visé : le 2ᵉ (42) → multiple inférieur = 32.

**Résultats :**
- Réseau : 10.32.0.0  
- Broadcast : 10.47.255.255  
- Hôtes utilisables : 2^(32 − 12) − 2 = 1 048 574  
- Plage d’hôtes : 10.32.0.1 → 10.47.255.254  

---

## Conclusion
Les exercices confirment la méthode générale :
1. Identifier l’octet du masque inférieur à 255 → IP_octet.  
2. Calculer le nombre magique : `256 − valeur_du_masque`.  
3. Base = ⌊IP_octet / magique⌋ × magique.  
4. Broadcast = base + (magique − 1).  
5. Plage = base+1 → broadcast−1.  
6. Hôtes = 2^(32 − préfixe) − 2.

Ces calculs permettent de déterminer précisément la structure d’un sous-réseau, qu’il s’agisse d’un découpage sur le 2ᵉ, 3ᵉ ou 4ᵉ octet.
