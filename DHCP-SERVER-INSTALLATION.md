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

## Installation et configuration d’un serveur DHCP sous Debian 12 & 13

## 🎯 Objectifs pédagogiques
- Comprendre le rôle et le fonctionnement d’un serveur **DHCP**  
- Installer et configurer un serveur DHCP sur **Debian 12 ou 13**  
- Réaliser des **réservations d’adresses IP** et ajouter des **options avancées** (NTP, exclusions)  
- Diagnostiquer et corriger les **erreurs courantes**  

---

## 🔧 Prérequis
- Une machine sous **Debian 12 (Bookworm)** ou **Debian 13 (Trixie)**, physique ou virtuelle
- Accès **root** ou **sudo**
- Une **interface réseau** connectée au LAN (non NAT uniquement si vous testez entre VMs)
- Un client (VM/PC) pour tester l’obtention d’une adresse via DHCP

---

## 🔹 Consignes générales
- Répondez aux questions de réflexion et **notez vos observations** après chaque étape.  
- Validez vos configurations par des **tests réels** (client qui récupère une IP, consultation des logs).  
- Sauvegardez vos fichiers avant modification.

---

## 1. Mise à jour du système
**Consigne :** Mettez à jour le système.  

```bash
sudo apt update && sudo apt upgrade -y
```

**Observation :**  
…

---

## 2. Installation du serveur DHCP
**Consigne :** Installez le paquet **ISC DHCP Server**.  

```bash
sudo apt install isc-dhcp-server -y
```

**Vérification :**
```bash
dpkg -l | grep isc-dhcp-server
```

**Observation :**  
…

---

## 3. Configuration du serveur DHCP
**Fichiers importants :**
- Configuration : `/etc/dhcp/dhcpd.conf`
- Interface(s) utilisée(s) : `/etc/default/isc-dhcp-server`
- Baux attribués : `/var/lib/dhcp/dhcpd.leases`

**Consigne :** Éditez le fichier principal :

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

**Exemple de configuration de base (à adapter à votre réseau) :**
```conf
authoritative;
default-lease-time 600;
max-lease-time 7200;

subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 8.8.4.4;

  # Exclusion d’une plage via un pool qui refuse tous les clients.
  # (Alternative : ne pas inclure cette plage dans 'range')
  pool {
    range 192.168.1.150 192.168.1.160;
    deny all clients;
  }

  # Serveur NTP local
  option ntp-servers 192.168.1.10;
}

# Réservation pour une imprimante (remplacer l’adresse MAC)
host imprimante {
  hardware ethernet AA:BB:CC:DD:EE:FF;
  fixed-address 192.168.1.50;
  option host-name "imprimante";
  option ntp-servers 192.168.1.10;
}
```

> 💡 **Note :** La directive `deny unknown-clients;` (si utilisée dans un subnet) limite l’attribution aux hôtes connus (avec réservation). Ne l’activez que si vous avez prévu des réservations pour tous les clients autorisés.

**Validation de la syntaxe :**
```bash
sudo dhcpd -t -4 -cf /etc/dhcp/dhcpd.conf
```

**Questions de réflexion :**
- À quoi sert la directive `authoritative` ?  
- Pourquoi exclure certaines adresses IP du pool ?  
- Quel est l’intérêt d’une réservation IP ?  
- Que permet l’option `ntp-servers` ?  

**Observation :**  
…

---

## 4. Définir l’interface réseau
**Consigne :** Indiquez l’interface à utiliser par le serveur DHCP dans `/etc/default/isc-dhcp-server` :

```bash
sudo nano /etc/default/isc-dhcp-server
```
Modifiez/ajoutez la ligne (adapter `eth0` au nom réel, vérifiable avec `ip a`) :

```bash
INTERFACESv4="eth0"
# INTERFACESv6=""
```

**Observation :**  
…

---

## 5. Démarrage et activation du service
**Consigne :** Activez et démarrez le service DHCP, puis vérifiez l’état.  

```bash
sudo systemctl enable isc-dhcp-server
sudo systemctl start isc-dhcp-server
sudo systemctl status isc-dhcp-server
```

**Observation :**  
…

---

## 6. Vérification et tests
**Logs du service :**
```bash
sudo journalctl -u isc-dhcp-server --since "today"
```

**Baux délivrés (sur le serveur) :**
```bash
sudo tail -n 20 /var/lib/dhcp/dhcpd.leases
```

**Test côté client :**
- Configurez le client pour obtenir une **IP en DHCP**.  
- Vérifiez l’IP obtenue (`ip a`), la **passerelle** (`ip route`) et le **DNS** (`/etc/resolv.conf`).

**Observation :**  
…

---

## 7. Dépannage et analyse
### Problèmes fréquents et solutions
- **Le service ne démarre pas :**
  - Vérifier la syntaxe : `sudo dhcpd -t -4 -cf /etc/dhcp/dhcpd.conf`
  - Consulter les logs : `sudo journalctl -u isc-dhcp-server -b`
  - Nom d’interface erroné dans `/etc/default/isc-dhcp-server`

- **Conflit de port (UDP 67) :**
  - Vérifier l’écoute : `sudo ss -ulpen | grep :67`
  - Arrêter le service en conflit ou changer la configuration.

- **Filtrage réseau / pare-feu :**
  - UFW : `sudo ufw allow 67/udp`
  - nftables (exemple simplifié) :
    ```bash
    sudo nft add rule inet filter input udp dport 67 accept
    ```

- **Aucun bail délivré :**
  - Câblage / VLAN / ponts virtuels mal configurés
  - Présence d’un **autre** serveur DHCP sur le même segment (conflit)
  - `deny unknown-clients;` activé par erreur

**Questions de réflexion :**
- Que faire si le service refuse toujours de démarrer après correction ?  
- Comment diagnostiquer un conflit de port ?  
- Comment vérifier qu’un autre DHCP n’opère pas sur le même LAN ?  

**Observation :**  
…

---

## 8. Pour aller plus loin (options avancées)
- **Réservations multiples**, options **domain-name**, **NTP**, **PXE** (option 66/67), etc.
- Documentation officielle Debian : https://wiki.debian.org/DHCP_Server

---

## 📚 Annexes – Exemples de configurations

### 1) Pool IP classique
```conf
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 1.1.1.1, 8.8.8.8;
}
```

### 2) Réservations multiples
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

### 3) Exclusion d’une plage IP
```conf
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.150;
  range 192.168.10.160 192.168.10.200;
  # Les adresses 192.168.10.151 à 159 sont exclues
}
```

### 4) Attribution d’options avancées
```conf
subnet 192.168.20.0 netmask 255.255.255.0 {
  range 192.168.20.50 192.168.20.100;
  option routers 192.168.20.1;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  option ntp-servers 192.168.20.2;
  option domain-name "reseau.local";
}
```

### 5) Pool pour invités (bail court)
```conf
subnet 192.168.30.0 netmask 255.255.255.0 {
  range 192.168.30.100 192.168.30.120;
  default-lease-time 300;
  max-lease-time 600;
  option routers 192.168.30.1;
}
```

**Fin du TP** ✅

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>

