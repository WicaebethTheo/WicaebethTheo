<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.svg">
  <img alt="0x57696b61 — Blue Team, infra & sécurité, self-hosting" src="assets/banner-light.svg" width="100%">
</picture>

<p align="center">
  <a href="https://0x57696b61.com"><img src="https://img.shields.io/badge/Site-0x57696b61.com-101114?style=flat-square" alt="Site"></a>
  <a href="https://docs.wicatool.cc"><img src="https://img.shields.io/badge/Docs-wicatool.cc-1a7f5a?style=flat-square" alt="Documentation"></a>
  <a href="https://discord.com/users/1530935252359315518"><img src="https://img.shields.io/badge/Discord-contact-5865F2?style=flat-square&logo=discord&logoColor=white" alt="Discord"></a>
  <img src="https://img.shields.io/badge/Cap-Infra_%26_Sécu_→_DevSecOps-0969da?style=flat-square" alt="Cap : Infra & Sécu puis DevSecOps">
</p>

## À propos

Ingénieur systèmes & réseaux côté **Blue Team**. J'aime comprendre en profondeur ce que j'exploite plutôt que de cliquer jusqu'à ce que ça marche.

- **Sécurité par défaut** : chaque service exposé passe par un WAF, chaque accès admin par un mesh chiffré.
- **Automatisation reproductible** : Ansible, GitLab CI, infrastructure as code.
- **Des runbooks plutôt que l'intuition** pour les incidents à 2 h du matin.
- **Souveraineté d'abord** : auto-hébergé quand c'est possible, hébergeurs européens sinon.

---

## 🚧 En ce moment

<table>
<tr>
<td width="50%" valign="top">

### 🎬 Wicaby
**Client multimédia multi-serveurs**

Un seul client pour **Emby, Jellyfin, Plex et Navidrome**, plus les sources **WebDAV et SMB**.

- Windows · Linux · Android · Android TV
- Bibliothèque de démo : films libres de la Blender Foundation (CC BY)
- Kotlin · Svelte · TypeScript
- Licence GPL-3.0

[Code](https://github.com/WicaebethTheo/wicaby)

</td>
<td width="50%" valign="top">

### 🧩 Tessera
**Supervision d'infra et veille CVE, en un seul tableau de bord**

Vue unifiée de l'infrastructure : suivi des **CVE**, supervision complète de l'infra et centralisation des logs.

- Logs **VictoriaLogs**, métriques **VictoriaMetrics**
- Veille CVE et état de l'infra au même endroit

<sub>🔒 Dépôt privé</sub>

</td>
</tr>
</table>

---

## Projets

<table>
<tr>
<td width="50%" valign="top">

### 🎫 ITSM Modern AI
**Triage IA de tickets GLPI, auto-hébergé et souverain**

*Le LLM propose, le code décide* : une allowlist et un seuil de confiance tranchent, et aucune action n'est appliquée sans règle déterministe et auditable.

- FastAPI · React · PostgreSQL · architecture hexagonale
- Auth *fail-closed*, Argon2, HKDF, licences hors-ligne Ed25519
- Connecteurs OpenAI / Mistral, LLM local possible
- Open-core, licence MIT

[Site](https://itsm-modern-ai.com) · [Docs](https://docs.itsm-modern-ai.com) · [Code](https://github.com/WicaebethTheo/itsm-modern-ai)

</td>
<td width="50%" valign="top">

### 🎯 Nemesis Hub
**Plateforme communautaire Valorant**

Matchmaking type FACEIT, customs 5v5, économie de points, boutique et console d'administration.

- Bot Discord + application web
- Livré en Docker avec CI/CD
- Utilisateurs concurrents, files d'attente, intégrité des parties

[nemesis-hub.com](https://nemesis-hub.com) · [Discord](https://discord.gg/Uq4MSaQBx6)

</td>
</tr>
</table>

| Projet | Ce que ça fait | Stack |
|---|---|---|
| [**Homelab Dashboard**](https://github.com/WicaebethTheo/homelab-dashboard) | Proxmox, sauvegardes, SIEM et services live sur un seul écran. Config dans un fichier YAML, aucun secret en dur | React · Express · Docker |
| [**Hardening Linux**](https://github.com/WicaebethTheo/harden-linux) | Durcissement SSH multi-distro : détecte les algos dispo (`ssh -Q`), ajoute le post-quantique, **auto-revert** si l'accès est coupé | Bash · OpenSSH · `sudo-rs` |
| [**Pont RCON SCUM**](https://github.com/WicaebethTheo/DivergenceRCON) | Endpoint Source RCON : pont C++ Winsock + mod Lua via un spool protégé, administrable même serveur vide | C++ · Lua · CMake · UE4SS |
| [**Wicatool.cc**](https://docs.wicatool.cc) | Hub de documentation auto-hébergé : runbooks, procédures, outils d'admin | BookStack · DocMost |

---

## Homelab « Yggdrasil »

Deux nœuds Proxmox, un NAS, des sauvegardes hors site testées, un réseau segmenté et un SIEM. C'est mon banc d'essai : rien ne part en production sans y être passé.

```mermaid
flowchart LR
    NET((Internet)) --> EDGE[VPS Edge<br/>WAF BunkerWeb]
    ADMIN([Accès admin]) --> MESH
    EDGE --> MESH{{Mesh NetBird<br/>WireGuard + Rosenpass}}
    IDP[VPS Identité<br/>SSO Authentik] -. OIDC .- MESH
    MESH --> RP[Reverse proxy HA<br/>Traefik · VIP keepalived]
    subgraph LAN [Réseau local · VLAN segmentés · backbone 10 G]
        RP --> THOR[Thor<br/>Proxmox · principal]
        RP --> FREYA[Freya<br/>Proxmox · secondaire]
        THOR --- NAS[(NAS unRAID)]
        FREYA --- NAS
    end
    THOR -. sauvegardes .-> PBS[(Mimir<br/>PBS hors site)]
    FREYA -. sauvegardes .-> PBS
```

| Machine | Rôle | CPU | RAM | Stockage | Réseau |
|---|---|---|---|---|---|
| **Thor** | Nœud Proxmox principal | Ryzen 9 7945HX | 96 Go DDR5 | 3 To NVMe (pool *Mjolnir*) | 10 Gb/s |
| **Freya** | Nœud Proxmox secondaire | Ryzen 9 7900X | 96 Go DDR5 | 2,5 To NVMe (pool *Gungnir*) | 2,5 Gb/s |
| **NAS** | Stockage SMB / NFS | Ryzen 9 3900X | 64 Go DDR4 | 1 To NVMe + 56 To HDD | 10 Gb/s |
| **VPS Edge** | WAF / reverse proxy public | 8 vCores | 16 Go | 512 Go NVMe | 2,5 Gb/s |
| **VPS Mesh** | Plan de contrôle NetBird | 4 vCores | 8 Go | 100 Go SSD | 1 Gb/s |
| **VPS Identité** | SSO | 4 vCores | 8 Go | 100 Go SSD | 1 Gb/s |

<details>
<summary><b>Ce qui tourne dessus</b></summary>
<br/>

- **Exposition** : WAF BunkerWeb en frontal, Traefik actif/passif derrière une VIP keepalived, wildcard Let's Encrypt en DNS-01
- **Accès** : mesh NetBird auto-hébergé, SSO Authentik (OIDC/SAML), couche post-quantique Rosenpass
- **Sécurité** : Wazuh (SIEM/HIDS), CrowdSec, Trivy, PKI interne, Vaultwarden, AdGuard Home
- **Observabilité** : VictoriaMetrics, VictoriaLogs, Vector, ntfy, Tessera
- **Automatisation** : Ansible + AWX, GitLab CI, Portainer
- **Réseau** : UniFi UDM Pro SE + MikroTik CRS309, un VLAN par usage (lab, prod, IoT, invités), règles inter-VLAN explicites

</details>

---

## Stack

<table>
<tr>
<td valign="top" width="22%"><b>Virtualisation & systèmes</b></td>
<td>
<img src="https://img.shields.io/badge/Proxmox_VE-30363d?style=flat-square&logo=proxmox&logoColor=E57000" alt="Proxmox VE">
<img src="https://img.shields.io/badge/PBS-30363d?style=flat-square&logo=proxmox&logoColor=E57000" alt="Proxmox Backup Server">
<img src="https://img.shields.io/badge/LXC-30363d?style=flat-square&logo=linuxcontainers&logoColor=white" alt="LXC">
<img src="https://img.shields.io/badge/Docker-30363d?style=flat-square&logo=docker&logoColor=2496ED" alt="Docker">
<img src="https://img.shields.io/badge/unRAID-30363d?style=flat-square&logo=unraid&logoColor=F15A2C" alt="unRAID">
<img src="https://img.shields.io/badge/Windows_Server-30363d?style=flat-square&logo=windows&logoColor=0078D4" alt="Windows Server">
<img src="https://img.shields.io/badge/Active_Directory-30363d?style=flat-square" alt="Active Directory">
<img src="https://img.shields.io/badge/Arch-30363d?style=flat-square&logo=archlinux&logoColor=1793D1" alt="Arch Linux">
<img src="https://img.shields.io/badge/Debian-30363d?style=flat-square&logo=debian&logoColor=A81D33" alt="Debian">
<img src="https://img.shields.io/badge/Ubuntu-30363d?style=flat-square&logo=ubuntu&logoColor=E95420" alt="Ubuntu">
<img src="https://img.shields.io/badge/Fedora-30363d?style=flat-square&logo=fedora&logoColor=51A2DA" alt="Fedora">
</td>
</tr>
<tr>
<td valign="top"><b>Réseau</b></td>
<td>
<img src="https://img.shields.io/badge/UniFi-30363d?style=flat-square&logo=ubiquiti&logoColor=0559C9" alt="UniFi">
<img src="https://img.shields.io/badge/MikroTik-30363d?style=flat-square&logo=mikrotik&logoColor=white" alt="MikroTik">
<img src="https://img.shields.io/badge/WireGuard-30363d?style=flat-square&logo=wireguard&logoColor=88171A" alt="WireGuard">
<img src="https://img.shields.io/badge/NetBird-30363d?style=flat-square&logo=netbird&logoColor=F68330" alt="NetBird">
<img src="https://img.shields.io/badge/Traefik-30363d?style=flat-square&logo=traefikproxy&logoColor=24A1C1" alt="Traefik">
<img src="https://img.shields.io/badge/FortiGate-30363d?style=flat-square&logo=fortinet&logoColor=EE3124" alt="FortiGate">
<img src="https://img.shields.io/badge/Stormshield-30363d?style=flat-square" alt="Stormshield">
<img src="https://img.shields.io/badge/AdGuard_Home-30363d?style=flat-square&logo=adguard&logoColor=68BC71" alt="AdGuard Home">
<img src="https://img.shields.io/badge/Cloudflare-30363d?style=flat-square&logo=cloudflare&logoColor=F38020" alt="Cloudflare">
</td>
</tr>
<tr>
<td valign="top"><b>Blue Team</b></td>
<td>
<img src="https://img.shields.io/badge/Wazuh-30363d?style=flat-square&logo=wazuh&logoColor=3D82F1" alt="Wazuh">
<img src="https://img.shields.io/badge/CrowdSec-30363d?style=flat-square" alt="CrowdSec">
<img src="https://img.shields.io/badge/BunkerWeb-30363d?style=flat-square" alt="BunkerWeb">
<img src="https://img.shields.io/badge/Trivy-30363d?style=flat-square&logo=trivy&logoColor=1904DA" alt="Trivy">
<img src="https://img.shields.io/badge/Authentik-30363d?style=flat-square&logo=authentik&logoColor=FD4B2D" alt="Authentik">
<img src="https://img.shields.io/badge/Vaultwarden-30363d?style=flat-square&logo=vaultwarden&logoColor=white" alt="Vaultwarden">
<img src="https://img.shields.io/badge/Let's_Encrypt-30363d?style=flat-square&logo=letsencrypt&logoColor=white" alt="Let's Encrypt">
<img src="https://img.shields.io/badge/OpenVAS-30363d?style=flat-square" alt="OpenVAS">
<br/><sub>SIEM / HIDS · WAF · PKI interne · SSO OIDC & SAML · scan de vulnérabilités · cadre NIS2</sub>
</td>
</tr>
<tr>
<td valign="top"><b>Observabilité & automatisation</b></td>
<td>
<img src="https://img.shields.io/badge/VictoriaMetrics-30363d?style=flat-square&logo=victoriametrics&logoColor=white" alt="VictoriaMetrics">
<img src="https://img.shields.io/badge/VictoriaLogs-30363d?style=flat-square&logo=victoriametrics&logoColor=white" alt="VictoriaLogs">
<img src="https://img.shields.io/badge/Vector-30363d?style=flat-square" alt="Vector">
<img src="https://img.shields.io/badge/ntfy-30363d?style=flat-square&logo=ntfy&logoColor=white" alt="ntfy">
<img src="https://img.shields.io/badge/Ansible_/_AWX-30363d?style=flat-square&logo=ansible&logoColor=white" alt="Ansible / AWX">
<img src="https://img.shields.io/badge/GitLab_CI-30363d?style=flat-square&logo=gitlab&logoColor=FC6D26" alt="GitLab CI">
<img src="https://img.shields.io/badge/Portainer-30363d?style=flat-square&logo=portainer&logoColor=13BEF9" alt="Portainer">
<img src="https://img.shields.io/badge/GLPI-30363d?style=flat-square" alt="GLPI">
</td>
</tr>
<tr>
<td valign="top"><b>Code</b></td>
<td>
<img src="https://img.shields.io/badge/Python-30363d?style=flat-square&logo=python&logoColor=3776AB" alt="Python">
<img src="https://img.shields.io/badge/FastAPI-30363d?style=flat-square&logo=fastapi&logoColor=009688" alt="FastAPI">
<img src="https://img.shields.io/badge/TypeScript-30363d?style=flat-square&logo=typescript&logoColor=3178C6" alt="TypeScript">
<img src="https://img.shields.io/badge/React-30363d?style=flat-square&logo=react&logoColor=61DAFB" alt="React">
<img src="https://img.shields.io/badge/Svelte-30363d?style=flat-square&logo=svelte&logoColor=FF3E00" alt="Svelte">
<img src="https://img.shields.io/badge/Kotlin-30363d?style=flat-square&logo=kotlin&logoColor=7F52FF" alt="Kotlin">
<img src="https://img.shields.io/badge/C++-30363d?style=flat-square&logo=cplusplus&logoColor=00599C" alt="C++">
<img src="https://img.shields.io/badge/Lua-30363d?style=flat-square&logo=lua&logoColor=white" alt="Lua">
<img src="https://img.shields.io/badge/Bash-30363d?style=flat-square&logo=gnubash&logoColor=4EAA25" alt="Bash">
<img src="https://img.shields.io/badge/PowerShell-30363d?style=flat-square&logo=powershell&logoColor=5391FE" alt="PowerShell">
<img src="https://img.shields.io/badge/PostgreSQL-30363d?style=flat-square&logo=postgresql&logoColor=4169E1" alt="PostgreSQL">
</td>
</tr>
</table>

**Poste de travail** : Omarchy (Arch) · Hyprland · Kitty · Fish · Neovim. [Dotfiles](https://github.com/WicaebethTheo/dotfiles)

---

## Ce que j'approfondis

- 🔐 **Active Directory** : durcissement, modèle de tiering, chemins d'attaque
- 🕸️ **Mesh NetBird / WireGuard** : extension entre sites
- ⚙️ **DevSecOps** : sécurisation des chaînes CI/CD et de l'infrastructure as code

---

<details>
<summary><b>🎮 Hors infra</b></summary>
<br/>

Joueur ranked, et le gaming est aussi un terrain de jeu technique : Nemesis Hub, pont RCON.

| Valorant | Overwatch 2 | Apex Legends | CS2 (Faceit) | Rainbow Six Siege |
|:-:|:-:|:-:|:-:|:-:|
| Radiant | Top 500 | Master | Level 10 | Champion |

> *Low ping en ranked, faible surface d'attaque en prod.*

</details>

<p align="center">
  <sub>Ouvert aux échanges sur l'infra, la sécurité et le self-hosting : ouvrez une issue ou passez par <a href="https://0x57696b61.com">le site</a>.</sub>
</p>
