# Projet MPLS VPN – Conception d’un backbone sécurisé

**Auteurs :**  
- Fayrouz Jbeli  
- Wajid Hamdi  
- Yossri Hamdouni  

**Encadrant :** Barrani.Nour  

---

## Architecture finale

![Architecture globale](architecture_finale.png)

*Schéma complet du projet : backbone MPLS (PE1, PE2, P1, P2), sites clients (CE11, CE12, CE21, CE22), fédérateurs, switchs d’accès, serveurs (RADIUS, Zabbix) et postes administrateurs.*

---

## Organisation du dépôt

- `cahier_de_charge.pdf` – le document officiel des exigences.
- `rapport_projet.pdf` – le rapport complet (contient toutes les captures d’écran et explications).
- `architecture_finale.png` – image de la topologie globale.
- `Partie1_Backbone_MPLS/configs/` – configurations des routeurs P, PE et CE.
- `Partie2_Siege_Branches/configs/` – configurations des switchs L2/L3, fédérateurs.
- `Partie3_AAA_Monitoring/configs/` – configurations AAA, RADIUS, SNMP, logging, NTP, IP SLA.

---

## Détail des trois parties

### Partie 1 – Backbone MPLS et VPN

**Objectif :**  
Construire un cœur de réseau MPLS permettant la communication entre sites clients tout en isolant leurs flux (VPN).

**Protocoles et technologies utilisés :**  
- **OSPF (Area 0)** : routage dynamique entre les routeurs P et PE.  
- **MPLS et LDP** : commutation de labels pour accélérer le forwarding.  
- **VRF (VPN Routing and Forwarding)** : création d’une table de routage virtuelle dédiée au client (`VPN_SECURITY`).  
- **MP‑BGP (famille d’adresses VPNv4)** : échange des routes VPN entre PE1 et PE2.  
- **Redistribution OSPF ↔ BGP** dans la VRF.

**Importance :**  
Cette partie est le cœur du projet. Elle permet la **connectivité inter‑sites** (Siège → Branches) et l’**isolation** des flux de différents clients, pierre angulaire des réseaux d’opérateurs.

**Fichiers de configuration :**  
`Partie1_Backbone_MPLS/configs/` contient les configurations complètes de PE1, PE2, P1, P2, CE11, CE12, CE21, CE22.

---

### Partie 2 – Site Siège et Branches (LAN et redondance)

**Objectif :**  
Déployer l’infrastructure LAN du siège et des branches avec segmentation, redondance et services de base.

**Protocoles et technologies utilisés :**  
- **VLAN (210 DATA1, 215 DATA2, 220 Management, 300 Inter-Fed)** : isolation du trafic.  
- **EtherChannel (LACP)** : agrégation de liens entre les deux fédérateurs (bande passante accrue et redondance).  
- **HSRP (Hot Standby Router Protocol)** : redondance de passerelle par défaut (IP virtuelle partagée).  
- **DHCP** : attribution dynamique d’adresses IP aux postes du siège (les 10 premières adresses sont réservées).  
- **Routage inter‑VLAN** : les fédérateurs assurent la communication entre les VLANs et avec le backbone MPLS.

**Importance :**  
Cette partie apporte la **haute disponibilité** (EtherChannel, HSRP) et la **flexibilité** (VLAN, DHCP). Elle garantit une infrastructure résiliente pour les utilisateurs finaux.

**Fichiers de configuration :**  
`Partie2_Siege_Branches/configs/` contient les configurations des fédérateurs (Federateur1, Federateur2) et des switchs d’accès (SW1, SW2, SW3, SW4).

---

### Partie 3 – AAA, Monitoring et Sécurité

**Objectif :**  
Sécuriser l’accès aux équipements, centraliser la supervision et mesurer les performances du réseau.

**Protocoles et technologies utilisés :**  
- **AAA (Authentication, Authorization, Accounting)** :  
  - Méthode 1 : RADIUS (serveur FreeRADIUS sur Ubuntu, IP 172.16.220.200, clé `VPN_SECURITY`).  
  - Méthode 2 : locale (fallback avec utilisateur `fayrouz` / `iyed`).  
- **ACL étendue** : sur les lignes VTY, autorisation des seules IP des administrateurs (172.16.220.100 et 172.16.220.150).  
- **SNMP** : communauté `SSIR` en lecture seule, activation des traps (linkdown, linkup, ospf, config, sla).  
- **Logging** : envoi des logs syslog vers le serveur Zabbix (172.16.220.250).  
- **NTP** : synchronisation horaire avec le serveur Zabbix.  
- **IP SLA** : sondes UDP-jitter (ou icmp-echo) entre CE12/CE22 et CE11/CE21 pour mesurer latence, jitter et perte.  
- **Zabbix** : installation sur VM Ubuntu, interface web, collecte SNMP, supervision des équipements.

**Importance :**  
Cette partie répond aux besoins de **sécurité** (authentification centralisée, restriction d’accès), de **supervision** (SNMP, logging, Zabbix) et de **qualité de service** (IP SLA). Elle est indispensable pour l’exploitation d’un réseau d’entreprise.

**Fichiers de configuration :**  
`Partie3_AAA_Monitoring/configs/` contient les configurations AAA, RADIUS, SNMP, logging, NTP, IP SLA pour tous les équipements.

---

## Validation et résultats

Toutes les fonctionnalités ont été testées dans GNS3 (pings inter‑sites, redondance HSRP, authentification AAA, collecte SNMP, etc.). Les captures d’écran et les détails des tests sont disponibles dans le **rapport** (`rapport_projet.pdf`).

---

## Documents joints

- **Cahier des charges** : `cahier_de_charge.pdf`  
- **Rapport complet** : `rapport_projet.pdf`  

---

*Projet réalisé dans le cadre du cours SSIR – 2026*
