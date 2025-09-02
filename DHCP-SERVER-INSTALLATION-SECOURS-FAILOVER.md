<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
  <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3ECYBERSECURITE_" alt="Titre dynamique CYBERSECURITE" />
  </a>
  
  <br></br>

  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT</h2>
  
  <p align="center">
      <a href="https://0xcyberlitech.github.io/">
        <img src="https://img.shields.io/badge/Portfolio-0xCyberLiTech-181717?logo=github&style=flat-square" alt="Portfolio" />
      </a>
      <a href="https://github.com/0xCyberLiTech">
        <img src="https://img.shields.io/badge/Profil-GitHub-181717?logo=github&style=flat-square" alt="Profil GitHub" />
      </a>
      <a href="https://github.com/0xCyberLiTech/Cybersecurite/releases/latest">
        <img src="https://img.shields.io/github/v/release/0xCyberLiTech/Cybersecurite?label=version" alt="Latest Release" />
      </a>
      <a href="https://github.com/0xCyberLiTech/Cybersecurite/blob/main/CHANGELOG.md">
        <img src="https://img.shields.io/badge/📄%20CHANGELOG-Cybersecurite-blue" alt="Changelog" />
      </a>
      <a href="https://github.com/0xCyberLiTech?tab=repositories">
        <img src="https://img.shields.io/badge/Dépôts-publics-blue?style=flat-square" alt="Dépôts publics" />
      </a>
  </p>

</div>

<!-- Optimisation SEO : DHCP, Dynamic Host Configuration Protocol, serveur DHCP, configuration IP, adresse IP automatique, réseau, administration système, Linux, Debian, cybersécurité, sécurité informatique, IT, open source, tutoriels, guides, formation, expertise, étudiants, professionnels, bonnes pratiques, ressources techniques, infrastructure réseau, services réseaux, supervision, logs, monitoring, cloud, virtualisation, DevSecOps -->

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

## 🚀 À propos & Objectifs

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

---

## Tutoriel : Installation d’un serveur DHCP de secours (Failover) sous Debian 12/13

---

## 1. Introduction

Le protocole **DHCP Failover** permet à deux serveurs DHCP de fonctionner ensemble afin d’assurer une continuité de service en cas de panne de l’un d’eux. En cas d’indisponibilité du serveur principal, le serveur de secours prend le relais et distribue les adresses IP aux clients.

---

## 2. Schéma explicatif

```
   [Clients]
      |
   -----
   |   |
[DHCP1] [DHCP2]
(Principal) (Secours)
      |      |
   ----+------+
       |
   [Réseau]
```
**DHCP1** et **DHCP2** échangent des informations sur les baux et se synchronisent via le protocole failover.

---

## 3. Prérequis

- Deux serveurs Debian 12 ou 13 (ex : `dhcp1` et `dhcp2`)
- Accès root ou sudo
- Les serveurs doivent pouvoir communiquer entre eux (même réseau ou VPN)
- Les ports TCP 647 et 647 doivent être ouverts entre les deux serveurs

---

## 4. Installation du serveur DHCP

Sur **chaque serveur** (dhcp1 et dhcp2), installez le serveur DHCP :

```bash
sudo apt update
sudo apt install isc-dhcp-server
```

---

## 5. Configuration du DHCP Failover

### 5.1. Fichier de configuration principal `/etc/dhcp/dhcpd.conf`

Exemple de configuration **failover** pour les deux serveurs.

#### **Sur DHCP1 (serveur principal)**

```conf
# /etc/dhcp/dhcpd.conf
authoritative;
ddns-update-style none;

# Définition du pool failover
failover peer "dhcp-failover" {
  primary;                     # DHCP1 est principal
  address 192.168.1.10;        # IP de DHCP1
  port 647;                    # Port failover
  peer address 192.168.1.20;   # IP de DHCP2
  peer port 647;
  max-response-delay 60;
  max-unacked-updates 10;
  load balance max seconds 3;
  mclt 3600;
  split 128;
}

subnet 192.168.1.0 netmask 255.255.255.0 {
  pool {
    failover peer "dhcp-failover";
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option domain-name "mondomaine.local";
    default-lease-time 600;
    max-lease-time 7200;
  }
}
```

#### **Sur DHCP2 (serveur de secours)**

```conf
# /etc/dhcp/dhcpd.conf
authoritative;
ddns-update-style none;

failover peer "dhcp-failover" {
  secondary;                   # DHCP2 est secours
  address 192.168.1.20;        # IP de DHCP2
  port 647;
  peer address 192.168.1.10;   # IP de DHCP1
  peer port 647;
  max-response-delay 60;
  max-unacked-updates 10;
  load balance max seconds 3;
  mclt 3600;
}

subnet 192.168.1.0 netmask 255.255.255.0 {
  pool {
    failover peer "dhcp-failover";
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option domain-name "mondomaine.local";
    default-lease-time 600;
    max-lease-time 7200;
  }
}
```

---

### 5.2. Configuration de l’interface réseau

Dans `/etc/default/isc-dhcp-server`, assurez-vous que l’interface correspond à celle connectée au réseau :

```sh
INTERFACESv4="eth0"
```
Adaptez selon votre configuration réseau.

---

## 6. Démarrage et synchronisation des serveurs

1. **Démarrez le serveur principal en premier :**
   ```bash
   sudo systemctl start isc-dhcp-server
   ```
2. **Attendez 1 minute**, puis démarrez le serveur secondaire :
   ```bash
   sudo systemctl start isc-dhcp-server
   ```

---

## 7. Vérification du fonctionnement

- **Vérifiez le statut du service :**
  ```bash
  sudo systemctl status isc-dhcp-server
  ```
- **Vérifiez les logs :**
  ```bash
  sudo tail -f /var/log/syslog
  ```
  Vous devez voir des échanges “peer” entre les deux serveurs.

---

## 8. Test du failover

1. Éteignez le serveur principal (`dhcp1`).
2. Connectez un client au réseau : il doit recevoir une IP depuis le serveur de secours.
3. Rallumez le serveur principal : la synchronisation reprend automatiquement.

---

## 9. Conseils & bonnes pratiques

- Sauvegardez régulièrement le fichier `dhcpd.leases`.
- Surveillez les logs pour détecter tout problème de synchronisation.
- Testez la bascule au moins une fois par trimestre.

---

## 10. Références

- [Documentation officielle ISC DHCP](https://kb.isc.org/docs/isc-dhcp-44-manual-pages-dhcpdconf)
- [Failover Protocol](https://kb.isc.org/docs/isc-dhcp-44-manual-pages-dhcpdconf#failover-peer)

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>

