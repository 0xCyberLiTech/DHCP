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

<!-- Optimisation SEO : DHCP, secour, failover, Dynamic Host Configuration Protocol, serveur DHCP, configuration IP, adresse IP automatique, réseau, administration système, Linux, Debian, cybersécurité, sécurité informatique, IT, open source, tutoriels, guides, formation, expertise, étudiants, professionnels, bonnes pratiques, ressources techniques, infrastructure réseau, services réseaux, supervision, logs, monitoring, cloud, virtualisation, DevSecOps -->

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


## TP Cours : Installation et configuration d’un serveur DHCP sous Debian 12 & 13

### Objectifs pédagogiques
- Comprendre le rôle et le fonctionnement d’un serveur DHCP
- Installer et configurer un serveur DHCP sur Debian
- Réaliser des réservations d’adresses IP et associer des options avancées (NTP, exclusions)
- Savoir diagnostiquer et corriger les erreurs courantes

---

### Consignes générales
Suivez les étapes ci-dessous, répondez aux questions et validez chaque étape par des tests ou observations. Ce TP est à réaliser sur une machine virtuelle ou physique sous Debian 12 ou 13.

---

### 1. Mise à jour du système
**Consigne :** Mettez à jour votre système. Notez la commande utilisée et le résultat.

```bash
sudo apt update && sudo apt upgrade -y
```

**Observation :**

---

### 2. Installation du serveur DHCP
**Consigne :** Installez le paquet ISC DHCP Server. Notez la commande et vérifiez l’installation.

```bash
sudo apt install isc-dhcp-server -y
```

**Observation :**

---

### 3. Configuration du serveur DHCP
**Consigne :** Ouvrez et modifiez le fichier `/etc/dhcp/dhcpd.conf` selon les exemples ci-dessous.

```conf
authoritative;
default-lease-time 600;
max-lease-time 7200;
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  # Exclusion d'adresses IP (ne pas attribuer ces adresses)
  deny unknown-clients;
  # Plage exclue (exemple: 192.168.1.150 à 192.168.1.160)
  pool {
    range 192.168.1.150 192.168.1.160;
    deny all clients;
  }
  # Ajout du serveur NTP (ntpsec)
  option ntp-servers 192.168.1.10;
}

# Réservation d'une adresse IP pour un client spécifique
host imprimante {
  hardware ethernet AA:BB:CC:DD:EE:FF;
  fixed-address 192.168.1.50;
  option host-name "imprimante";
  option ntp-servers 192.168.1.10;
}
```

**Questions de réflexion :**
- À quoi sert la directive `authoritative` ?
- Pourquoi exclure certaines adresses IP du pool ?
- Quel est l’intérêt de réserver une IP pour une machine ?
- Que permet l’option `ntp-servers` ?

**Observation :**

---

### 4. Définir l’interface réseau
**Consigne :** Modifiez `/etc/default/isc-dhcp-server` pour indiquer l’interface réseau utilisée.

```bash
INTERFACESv4="eth0"
```

**Observation :**

---

### 5. Démarrage et activation du service
**Consigne :** Activez et démarrez le service DHCP. Vérifiez son statut.

```bash
sudo systemctl enable isc-dhcp-server
sudo systemctl start isc-dhcp-server
sudo systemctl status isc-dhcp-server
```

**Observation :**

---

### 6. Vérification et tests
**Consigne :**
- Vérifiez les logs du service DHCP
- Testez l’attribution d’une adresse IP sur un client du réseau

```bash
sudo journalctl -u isc-dhcp-server
```

**Observation :**

---

### 7. Dépannage et analyse
**Consigne :** Listez les erreurs possibles et proposez des solutions.

**Questions de réflexion :**
- Que faire si le service ne démarre pas ?
- Comment vérifier la syntaxe du fichier de configuration ?
- Comment s’assurer que le port UDP 67 est ouvert ?

**Observation :**

---

### 8. Pour aller plus loin
**Consigne :** Explorez la documentation officielle et testez des options avancées (réservations multiples, options personnalisées).
https://wiki.debian.org/DHCP_Server

---
**Fin du TP**

### Introduction
Le serveur DHCP (Dynamic Host Configuration Protocol) permet d’attribuer automatiquement des adresses IP et autres paramètres réseau aux machines d’un réseau local. Ce tutoriel vous guide pas à pas pour installer et configurer un serveur DHCP sur Debian 12 et 13.

### Prérequis
- Une machine sous Debian 12 ou 13 (physique ou virtuelle)
- Accès root ou sudo
- Connexion réseau active

### 1. Mise à jour du système
Avant toute installation, mettez à jour votre système :

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Installation du serveur DHCP
Installez le paquet ISC DHCP Server :

```bash
sudo apt install isc-dhcp-server -y
```

### 3. Configuration du serveur DHCP
Le fichier principal de configuration est `/etc/dhcp/dhcpd.conf`.

Ouvrez-le avec votre éditeur préféré :

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Ajoutez une configuration de base (à adapter selon votre réseau) :

```conf
authoritative;
default-lease-time 600;
max-lease-time 7200;
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  # Exclusion d'adresses IP (ne pas attribuer ces adresses)
  deny unknown-clients;
  # Plage exclue (exemple: 192.168.1.150 à 192.168.1.160)
  # Pour exclure, ne pas inclure dans la plage 'range' ou utiliser 'deny' dans un pool séparé
  # Exemple de pool exclu :
  pool {
    range 192.168.1.150 192.168.1.160;
    deny all clients;
  }
  # Ajout du serveur NTP (ntpsec)
  option ntp-servers 192.168.1.10;
}

# Réservation d'une adresse IP pour un client spécifique
host imprimante {
  hardware ethernet AA:BB:CC:DD:EE:FF;
  fixed-address 192.168.1.50;
  option host-name "imprimante";
  option ntp-servers 192.168.1.10;
}
```

> **Remarque :** Adaptez l’adresse du réseau, la plage IP et la passerelle selon votre infrastructure.

### 4. Définir l’interface réseau
Indiquez l’interface à utiliser par le serveur DHCP dans `/etc/default/isc-dhcp-server` :

```bash
sudo nano /etc/default/isc-dhcp-server
```

Modifiez la ligne INTERFACESv4 :

```bash
INTERFACESv4="eth0"
```

Remplacez `eth0` par le nom de votre interface réseau (utilisez `ip a` pour la connaître).

### 5. Démarrage et activation du service
Activez et démarrez le service DHCP :

```bash
sudo systemctl enable isc-dhcp-server
sudo systemctl start isc-dhcp-server
```

### 6. Vérification du fonctionnement
Vérifiez le statut du service :

```bash
sudo systemctl status isc-dhcp-server
```

Consultez les logs pour diagnostiquer d’éventuels problèmes :

```bash
sudo journalctl -u isc-dhcp-server
```

### 7. Dépannage
- Vérifiez la syntaxe du fichier de configuration :
  - Les erreurs empêchent le démarrage du service.
- Assurez-vous que l’interface réseau est correcte.
- Vérifiez que le port UDP 67 n’est pas bloqué par un firewall.

### 8. Aller plus loin
Pour des configurations avancées (réservations d’IP, options supplémentaires), consultez la documentation officielle :
https://wiki.debian.org/DHCP_Server

---

## Exemples de configurations DHCP

### 1. Pool IP classique
```conf
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 1.1.1.1, 8.8.8.8;
}
```

### 2. Réservations pour plusieurs machines
```conf
host serveur1 {
  hardware ethernet 00:11:22:33:44:55;
  fixed-address 192.168.10.10;
}
host imprimante {
  hardware ethernet AA:BB:CC:DD:EE:FF;
  fixed-address 192.168.10.20;
}
```

### 3. Exclusion d’une plage IP
```conf
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.150;
  range 192.168.10.160 192.168.10.200;
  # Les adresses 192.168.10.151 à 159 sont exclues
}
```

### 4. Attribution d’options avancées
```conf
subnet 192.168.20.0 netmask 255.255.255.0 {
  range 192.168.20.50 192.168.20.100;
  option routers 192.168.20.1;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  option ntp-servers 192.168.20.2;
  option domain-name "reseau.local";
}
```

### 5. Pool pour invités (bail court)
```conf
subnet 192.168.30.0 netmask 255.255.255.0 {
  range 192.168.30.100 192.168.30.120;
  default-lease-time 300;
  max-lease-time 600;
  option routers 192.168.30.1;
}
```

---
Chaque exemple est à adapter selon votre réseau. Les commentaires dans chaque bloc facilitent la compréhension et l’adaptation.

---
Ce guide vous permet de mettre en place rapidement un serveur DHCP fonctionnel sur Debian 12 & 13.

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>

