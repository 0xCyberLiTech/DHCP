<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3EDHCP_" alt="Titre dynamique DHCP" />
  </a>
  
  <br></br>

  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT.</h2>
  
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

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

<div align="center">
  
## À propos & Objectifs.

</div>

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

---

# Installation et configuration d'un serveur DHCP sur Debian 13

**Auteur :** Marc Malet  
**Créé :** 06-02-2026  
**Révisé :** 07-02-2026

## Objectif
Ce README explique, pas à pas et de manière pédagogique, comment installer et configurer un serveur DHCP (`isc-dhcp-server`) sur une machine Debian 13. Il inclut vérifications, sauvegardes et conseils de sécurité.

## Prérequis
- Compte avec droits `sudo`.
- Accès SSH ou console.
- Connaître l'interface LAN (ex. `ens18`, `eth0`).
- Sous-réseau (ex. `10.100.100.0/24`).
- IP statique réservée pour le serveur (ex. `10.100.100.200`).
- Passerelle (ex. `10.100.100.1`) et DNS (ex. `1.1.1.1`, `1.0.0.1`).

> Remarque : Debian peut utiliser `ifupdown`, `systemd-networkd` ou `NetworkManager`. Ce guide suppose `/etc/network/interfaces`. Adaptez si nécessaire.

---

## 1) Vérifier l'état réseau

Lister interfaces et adresses :

```sh
ip a
```

Afficher configuration :

```sh
cat /etc/network/interfaces
cat /etc/network/interfaces.d/*
```

Exemple de configuration statique pour `/etc/network/interfaces` :

```ini
# The primary network interface
allow-hotplug ens18
iface ens18 inet static
  address 10.100.100.200
  netmask 255.255.255.0
  gateway 10.100.100.1
  broadcast 10.100.100.255
  dns-nameservers 1.1.1.1 1.0.0.1
```

Après modification, redémarrer le service réseau ou la machine selon votre configuration.

---

## 2) Vérifier `/etc/hosts` et `/etc/resolv.conf`

- `/etc/hosts` : associer l'IP à un nom d'hôte local si besoin :

```
127.0.0.1       localhost
10.100.100.200  srv-lab-01.homelab.local  srv-lab-01
```

- `/etc/resolv.conf` : définir les serveurs DNS (attention : fichier géré par des gestionnaires réseau) :

```
search homelab.local
nameserver 1.1.1.1
nameserver 1.0.0.1
```

---

## 3) Installer `isc-dhcp-server`

```sh
sudo apt update
sudo apt install isc-dhcp-server -y
```

---

## 4) Indiquer l'interface pour le service DHCP

Identifier l'interface :

```sh
ip a
```

Éditer `/etc/default/isc-dhcp-server` :

```ini
INTERFACESv4="ens18"
INTERFACESv6=""
```

---

## 5) Configuration principale — `/etc/dhcp/dhcpd.conf`

Faire une sauvegarde puis éditer :

```sh
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak
sudo truncate -s 0 /etc/dhcp/dhcpd.conf
sudo nano /etc/dhcp/dhcpd.conf
```

Exemple minimal et commenté :

```conf
ddns-update-style none;
option domain-name "homelab.local";
option domain-name-servers 1.1.1.1, 1.0.0.1;

default-lease-time 3600;
max-lease-time 86400;

authoritative;

subnet 10.100.100.0 netmask 255.255.255.0 {
  range 10.100.100.100 10.100.100.150;
  option routers 10.100.100.1;
  option broadcast-address 10.100.100.255;
  option domain-name-servers 1.1.1.1, 1.0.0.1;
}

# Exemples de réservations statiques (à adapter)
#host srv-zabbix {
#  hardware ethernet aa:bb:cc:dd:ee:ff;
#  fixed-address 10.100.100.101;
#  option host-name "srv-zabbix";
#}
```

Notes : `range` = plage dynamique, `fixed-address` = réservation pour une MAC.

---

## 6) Forwarding et routage (quand nécessaire)

Le DHCP n'est pas un routeur — il attribue des adresses et paramètres.

- Si le serveur DHCP est sur le même LAN que la gateway Internet, vous n'avez pas besoin d'activer `ip_forward`.
- Si la machine Debian agit comme passerelle (au moins 2 interfaces), activez `ip_forward` et potentiellement NAT.

Vérifier l'état :

```sh
cat /proc/sys/net/ipv4/ip_forward
```

Activer temporairement :

```sh
sudo sysctl -w net.ipv4.ip_forward=1
```

Rendre persistant : ajouter `net.ipv4.ip_forward=1` dans `/etc/sysctl.conf`.

Exemple NAT (si nécessaire) :

```sh
sudo iptables -t nat -A POSTROUTING -o <interface-externe> -j MASQUERADE
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

Préférez `nftables` ou un service systemd pour restaurer les règles proprement.

---

## 7) Démarrer et vérifier le service

```sh
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
sudo systemctl status isc-dhcp-server
sudo journalctl -u isc-dhcp-server -e
sudo cat /var/lib/dhcp/dhcpd.leases
```

---

## 8) Tests pratiques

- Connectez un client et vérifiez qu'il obtient une IP dans la `range`.
- Depuis le client : `ipconfig /all` (Windows) ou `ip a` (Linux).
- Pinguer la passerelle et Internet :

```sh
ping -c 4 10.100.100.1
ping -c 4 8.8.8.8
```

---

## 9) Sauvegarde et bonnes pratiques

- Sauvegarder les fichiers modifiés :

```sh
sudo cp /etc/dhcp/dhcpd.conf /root/dhcpd.conf.bak
sudo cp /etc/network/interfaces /root/network.interfaces.bak
```

- Sécuriser SSH (authentification par clé, désactiver `PasswordAuthentication` si possible).
- Mettre à jour régulièrement : `sudo apt update && sudo apt upgrade -y`.
- Restreindre accès au serveur DHCP via firewall si nécessaire.

---

## 10) FAQ détaillée (avec schémas)

**Question :** Dois‑je activer `net.ipv4.ip_forward` si ma Debian a déjà une IP fixe et un accès Internet ?  
**Réponse :** Non, sauf si la Debian doit router le trafic d'autres machines.

**Explication et schémas :**

Cas 1 — Pas de forwarding nécessaire (configuration courante)

```
         Internet
            |
     [ Routeur / GW ]
       10.100.100.1
            |
   ----------------------
   |                    |
   [PC Linux]          Autres hôtes
   10.100.100.200
```

Flux : PC Linux → 10.100.100.1 → Internet  (net.ipv4.ip_forward = 0)

Cas 2 — Serveur DHCP uniquement (toujours pas de forwarding)

```
                  Internet
                     |
              [ Routeur / GW ]
                10.100.100.1
                     |
   ------------------- ------------------
   |                 |                  |
   [PC Linux]        [Client]           [Client]
   (DHCP)            (DHCP)             (DHCP)
```

Les clients parlent directement au routeur.

Cas 3 — PC Linux utilisé comme routeur (forwarding nécessaire)

```
         Internet
            |
     [ Routeur / Box ]
            |
         (ens18)
       [ PC Linux ]
         (ens19)
            |
      -------------
      |           |
   Client A    Client B
```

Ici `net.ipv4.ip_forward = 1` (OBLIGATOIRE) et généralement NAT/masks via iptables/nftables.

**Q :** Mon `/etc/resolv.conf` est écrasé à chaque reboot.  
**R :** Un gestionnaire réseau peut réécrire ce fichier. Options : configurer DNS via le gestionnaire (`NetworkManager`, `systemd-resolved`, `dhclient`), ou ajuster `/etc/dhcp/dhclient.conf`.

**Q :** Bail DHCP vs réservation statique ?  
- Bail = IP attribuée dynamiquement depuis `range` pour une durée.  
- Réservation statique = bloc `host` + `fixed-address` lié à une MAC.

---

## Annexes / références
- `man dhcpd.conf`  
- Debian Network docs: https://www.debian.org/doc/

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>

