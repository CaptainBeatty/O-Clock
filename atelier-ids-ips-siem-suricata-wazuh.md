
# Atelier — IDS/IPS & SIEM (Suricata + Wazuh) — Lab Proxmox

## Contexte
- Hyperviseur : Proxmox
- Réseau LAN : vmbr2 — 10.0.0.0/16
- Gateway/Firewall : pfSense — 10.0.0.1
- Client : Windows — 10.0.0.10
- IDS/IPS : Suricata — 10.0.0.50
- SIEM : Wazuh (All-in-one) — 10.0.0.40

## Objectif
Mettre en place une chaîne SOC : **détection (Suricata)** → **collecte (Wazuh agent)** → **corrélation/visualisation (Wazuh Dashboard)**.

---

# Étape 0 — Pré-check infra & capacité
## Actions
- [*] Vérifier RAM dispo (>= 10 Go sur l’hôte Proxmox)
- [ ] Vérifier vmbr2 opérationnel + routage/NAT via pfSense
- [ ] Valider DNS/Internet sortant depuis le LAN

## Preuves (captures)
- [ ] Proxmox: résumé ressources (RAM)
- [ ] pfSense: règles LAN/NAT (si nécessaire)
- [ ] Ping gateway + 8.8.8.8 depuis une VM LAN

## Résultat
- ✅ OK / ❌ NOK
- Notes:

---

# Étape 1 — Déployer Suricata (CT/VM) + config IDS
## Actions
- [ ] Créer CT/VM sur vmbr2 avec IP statique 10.0.0.50/16 gw 10.0.0.1
- [ ] Installer Suricata + suricata-update
- [ ] Configurer HOME_NET, interface capture, EVE JSON enrichi
- [ ] Télécharger règles ET Open
- [ ] Démarrer service + valider écoute

## Preuves (captures)
- [ ] `ip a`
- [ ] `suricata --build-info | head`
- [ ] Extrait `suricata.yaml` (HOME_NET, af-packet, eve-log alert)
- [ ] `grep -c "^alert" ...suricata.rules`
- [ ] `systemctl status suricata` + log “all AFP capture threads are running”

## Résultat
- ✅ OK / ❌ NOK
- Notes:

---

# Étape 2 — Générer un événement Suricata (test de règle)
## Actions
- [ ] `curl http://testmynids.org/uid/index.html`
- [ ] Vérifier alerte dans `eve.json` + `fast.log`

## Preuves (captures)
- [ ] sortie curl
- [ ] `jq 'select(.event_type=="alert")'` (1 alerte minimum)
- [ ] `tail` de `fast.log`

## Résultat
- ✅ OK / ❌ NOK
- Notes:

---

# Étape 3 — Déployer Wazuh (All-in-one)
## Actions
- [ ] Créer VM 10.0.0.40/16 (8 Go RAM mini, 50 Go disk)
- [ ] Installer Wazuh via script `wazuh-install.sh -a`
- [ ] Valider services (manager/indexer/dashboard)
- [ ] Accéder au dashboard https://10.0.0.40

## Preuves (captures)
- [ ] `systemctl status wazuh-*`
- [ ] écran login dashboard + menu latéral
- [ ] récupération credentials install

## Résultat
- ✅ OK / ❌ NOK
- Notes:

---

# Étape 4 — Connecter Suricata -> Wazuh (agent + collecte eve.json)
## Actions
- [ ] Installer wazuh-agent sur Suricata (manager=10.0.0.40)
- [ ] Enregistrer/valider agent “Active”
- [ ] Ajouter `<localfile>` sur `/var/log/suricata/eve.json` (json)
- [ ] Vérifier réception dans Discover (agent.name: Suricata)

## Preuves (captures)
- [ ] `manage_agents -l` côté manager
- [ ] capture dashboard Agents “Active”
- [ ] extrait `ossec.conf` localfile
- [ ] Discover avec événements Suricata

## Résultat
- ✅ OK / ❌ NOK
- Notes:

---

# Étape 5 — Validation end-to-end (détection visible dans Wazuh)
## Actions
- [ ] Rejouer test `curl testmynids`
- [ ] Filtrer Wazuh : `rule.groups: suricata` ou `data.alert.signature_id: 2100498`

## Preuves (captures)
- [ ] `fast.log` / `eve.json`
- [ ] Discover avec SID 2100498

## Résultat
- ✅ OK / ❌ NOK
- Notes:

---

# Bonus — Règle Suricata custom + remontée Wazuh
## Actions
- [ ] `local.rules` avec SID 1000001
- [ ] Charger `local.rules` dans `suricata.yaml`
- [ ] Déclencher via curl/netcat
- [ ] Vérifier dans Wazuh (SID 1000001)

## Preuves (captures)
- [ ] contenu `local.rules`
- [ ] `jq` sur eve.json (SID 1000001)
- [ ] Discover SID 1000001

## Résultat
- ✅ OK / ❌ NOK
- Notes:
