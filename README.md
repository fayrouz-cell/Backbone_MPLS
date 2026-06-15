# Projet MPLS VPN – Conception d’un backbone sécurisé

**Auteurs :**  
- Fayrouz Jbeli  
- Wajid Hamdi  
- Yossri Hamdouni  

**Encadrant :** Mr. Tarek Hdiji  

---

## Architecture finale

![Architecture globale](architecture_finale.png)

*Schéma complet du projet : backbone MPLS (PE1, PE2, P1, P2), sites clients (CE11, CE12, CE21, CE22), fédérateurs, switchs d’accès, serveurs (RADIUS, Zabbix) et postes administrateurs.*

---

## Organisation du projet

Le projet est découpé en trois parties indépendantes mais complémentaires. Chaque partie est documentée en détail dans le **rapport** (PDF) et les configurations sont fournies dans les dossiers correspondants.

### 📁 Structure du dépôt

- `cahier_de_charge.pdf` – le document officiel des exigences.
- `rapport_projet.pdf` – le rapport complet avec captures d’écran et explications.
- `architecture_finale.png` – schéma global du réseau.
- `Partie1_Backbone_MPLS/configs/` – configurations des routeurs P, PE et CE.
- `Partie2_Siege_Branches/configs/` – configurations des switchs L2/L3, fédérateurs.
- `Partie3_AAA_Monitoring/configs/` – configurations AAA, RADIUS, SNMP, logging, NTP, IP SLA.

### 📌 Partie 1 – Backbone MPLS et VPN

**Protocoles :** OSPF (Area 0), MPLS/LDP, VRF, MP‑BGP (VPNv4).  
**Rôle :** Cœur du réseau – assure la connectivité inter‑sites et l’isolation des flux client.

### 📌 Partie 2 – Site Siège et Branches

**Protocoles :** VLAN, EtherChannel (LACP), HSRP, DHCP, routage inter‑VLAN.  
**Rôle :** Infrastructure LAN redondante et segmentée (DATA1, DATA2, Management).

### 📌 Partie 3 – AAA, Monitoring et Sécurité

**Protocoles :** AAA (RADIUS + local), ACL étendue, SNMP, syslog, NTP, IP SLA, Zabbix.  
**Rôle :** Sécurisation des accès, supervision centralisée, mesure de performances.

---

## Validation

L’ensemble des exigences du cahier des charges a été implémenté et testé sous GNS3. Les captures d’écran et les résultats sont présentés dans le rapport.

Pour plus de détails, veuillez consulter le **rapport** (`rapport_projet.pdf`) et le **cahier des charges** (`cahier_de_charge.pdf`).

---

*Projet réalisé dans le cadre du cours SSIR – 2026*
