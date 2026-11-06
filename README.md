<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.gif">
  <img alt="Théo — Blue Team, infra & sécurité, self-hosting" src="assets/banner-light.gif" width="100%">
</picture>

<p align="center">
  <a href="https://0x57696b61.com"><img src="assets/badges/link-site.svg" height="28" alt="Site : 0x57696b61.com"></a>
  <a href="https://docs.wicatool.cc"><img src="assets/badges/link-docs.svg" height="28" alt="Documentation : docs.wicatool.cc"></a>
  <a href="https://discord.com/users/1530935252359315518"><img src="assets/badges/link-discord.svg" height="28" alt="Discord"></a>
  <img src="assets/badges/link-cap.svg" height="28" alt="Cap : Infra & Sécu puis DevSecOps">
</p>

## À propos

Ingénieur systèmes & réseaux côté **Blue Team**. J'aime comprendre en profondeur ce que j'exploite plutôt que de cliquer jusqu'à ce que ça marche.

- **Sécurité par défaut** : chaque service exposé passe par un WAF, chaque accès admin par un mesh chiffré.
- **Automatisation reproductible** : Ansible, GitLab CI, infrastructure as code.
- **Des runbooks plutôt que l'intuition** pour les incidents à 2 h du matin.
- **Souveraineté d'abord** : auto-hébergé quand c'est possible, hébergeurs européens sinon.

---

## En ce moment

<table>
<tr>
<td width="50%" valign="top">

### <img src="assets/logos/wicaby.svg" width="36" height="36" align="center" alt=""> Wicaby
**Client multimédia multi-serveurs**<br/>
<sub>[Code](https://github.com/WicaebethTheo/wicaby) · GPL-3.0</sub>

Un seul client pour **Emby, Jellyfin, Plex et Navidrome**, plus les sources **WebDAV et SMB**.

- Windows · Linux · Android · Android TV
- Bibliothèque de démo : films libres de la Blender Foundation (CC BY)
- Android : Kotlin, Jetpack Compose, Media3 · Desktop : Electron, Svelte, TypeScript

</td>
<td width="50%" valign="top">

### <img src="assets/logos/tessera.svg" width="36" height="36" align="center" alt=""> Tessera
**Supervision d'infra et veille CVE, façon bureau**<br/>
<sub>🔒 Dépôt privé · MIT</sub>

Un tableau de bord homelab qui se comporte comme un bureau : widgets sur fond d'écran, apps en fenêtres tuilées, lanceur `Ctrl+K`.

- **37 types de widgets** : Proxmox/PBS, Docker, VictoriaMetrics & VictoriaLogs, Wazuh, UniFi, NetBird, AdGuard…
- **Suivi des CVE** et des mises à jour OS, explorateur de logs en direct, PromQL, carte réseau, SLO
- **Fil d'alertes unique** : incidents, silences, maintenances, runbooks ; notifs e-mail, ntfy, Discord
- Sécurisé par défaut : rôles, CSRF, coffre chiffré pour les jetons, CSP stricte
- React · Node (Hono) · TypeScript

</td>
</tr>
</table>

---

## Projets

<table>
<tr>
<td width="50%" valign="top">

### <img src="assets/logos/itsm-modern-ai.svg" width="36" height="36" align="center" alt=""> ITSM Modern AI
**Triage IA de tickets GLPI, auto-hébergé et souverain**<br/>
<sub>[Site](https://itsm-modern-ai.com) · [Docs](https://docs.itsm-modern-ai.com) · [Code](https://github.com/WicaebethTheo/itsm-modern-ai) · MIT</sub>

*Le LLM propose, le code décide* : une allowlist et un seuil de confiance tranchent, et aucune action n'est appliquée sans règle déterministe et auditable.

- FastAPI · React · PostgreSQL · Alembic · architecture hexagonale
- Auth *fail-closed*, Argon2, HKDF, licences hors-ligne Ed25519
- Connecteurs OpenAI / Mistral, LLM local possible
- Modèle open-core

</td>
<td width="50%" valign="top">

### <img src="assets/logos/nemesis.png" width="36" height="36" align="center" alt=""> Nemesis Hub
**Plateforme communautaire Valorant**<br/>
<sub>[nemesis-hub.com](https://nemesis-hub.com) · [Discord](https://discord.gg/Uq4MSaQBx6)</sub>

Matchmaking type FACEIT, customs 5v5, économie de points, boutique et console d'administration.

- Files, draft ou équilibrage auto, veto, MMR/Elo, classement, saisons
- Bots discord.py, API FastAPI, auth BetterAuth (OAuth Discord, RBAC), SPA React
- PostgreSQL 18, 7 stacks Docker Compose, CI/CD

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
<td valign="top" width="24%"><b>Virtualisation & systèmes</b></td>
<td>
<img src="assets/badges/proxmox.svg" height="28" alt="Proxmox VE">
<img src="assets/badges/pbs.svg" height="28" alt="Proxmox Backup Server">
<img src="assets/badges/lxc.svg" height="28" alt="LXC">
<img src="assets/badges/docker.svg" height="28" alt="Docker">
<img src="assets/badges/unraid.svg" height="28" alt="unRAID">
<img src="assets/badges/windows-server.svg" height="28" alt="Windows Server">
<img src="assets/badges/active-directory.svg" height="28" alt="Active Directory">
<img src="assets/badges/arch.svg" height="28" alt="Arch Linux">
<img src="assets/badges/debian.svg" height="28" alt="Debian">
<img src="assets/badges/ubuntu.svg" height="28" alt="Ubuntu">
<img src="assets/badges/fedora.svg" height="28" alt="Fedora">
<br/><sub>Cluster Proxmox · sauvegardes testées · Windows Server & AD (GPO, réplication)</sub>
</td>
</tr>
<tr>
<td valign="top"><b>Réseau</b></td>
<td>
<img src="assets/badges/unifi.svg" height="28" alt="UniFi">
<img src="assets/badges/mikrotik.svg" height="28" alt="MikroTik">
<img src="assets/badges/wireguard.svg" height="28" alt="WireGuard">
<img src="assets/badges/netbird.svg" height="28" alt="NetBird">
<img src="assets/badges/traefik.svg" height="28" alt="Traefik">
<img src="assets/badges/fortigate.svg" height="28" alt="FortiGate">
<img src="assets/badges/stormshield.svg" height="28" alt="Stormshield">
<img src="assets/badges/adguard.svg" height="28" alt="AdGuard Home">
<img src="assets/badges/cloudflare.svg" height="28" alt="Cloudflare">
<br/><sub>VLAN par usage · backbone 10 G · mesh WireGuard post-quantique · HA keepalived</sub>
</td>
</tr>
<tr>
<td valign="top"><b>Blue Team</b></td>
<td>
<img src="assets/badges/wazuh.svg" height="28" alt="Wazuh">
<img src="assets/badges/crowdsec.svg" height="28" alt="CrowdSec">
<img src="assets/badges/bunkerweb.svg" height="28" alt="BunkerWeb">
<img src="assets/badges/trivy.svg" height="28" alt="Trivy">
<img src="assets/badges/authentik.svg" height="28" alt="Authentik">
<img src="assets/badges/vaultwarden.svg" height="28" alt="Vaultwarden">
<img src="assets/badges/letsencrypt.svg" height="28" alt="Let's Encrypt">
<img src="assets/badges/openvas.svg" height="28" alt="OpenVAS">
<br/><sub>SIEM / HIDS · WAF · PKI interne · SSO OIDC & SAML · scan de vulnérabilités · cadre NIS2</sub>
</td>
</tr>
<tr>
<td valign="top"><b>Observabilité & automatisation</b></td>
<td>
<img src="assets/badges/victoriametrics.svg" height="28" alt="VictoriaMetrics">
<img src="assets/badges/victorialogs.svg" height="28" alt="VictoriaLogs">
<img src="assets/badges/vector.svg" height="28" alt="Vector">
<img src="assets/badges/ntfy.svg" height="28" alt="ntfy">
<img src="assets/badges/ansible.svg" height="28" alt="Ansible / AWX">
<img src="assets/badges/gitlab.svg" height="28" alt="GitLab CI">
<img src="assets/badges/portainer.svg" height="28" alt="Portainer">
<img src="assets/badges/glpi.svg" height="28" alt="GLPI">
<br/><sub>Métriques, logs et alerting auto-hébergés · infrastructure as code · pipelines CI/CD</sub>
</td>
</tr>
<tr>
<td valign="top"><b>Reverse engineering</b></td>
<td>
<img src="assets/badges/ida.svg" height="28" alt="IDA Pro">
<img src="assets/badges/idapython.svg" height="28" alt="IDAPython / IDC">
<img src="assets/badges/ue4ss.svg" height="28" alt="UE4SS">
<img src="assets/badges/dumper7.svg" height="28" alt="Dumper-7">
<img src="assets/badges/asm.svg" height="28" alt="Assembleur x86-64">
<br/><sub>Analyse de binaires natifs · signatures et adressage RIP-relatif · reconstruction de structures · dump de SDK Unreal Engine · mods C++/Lua</sub>
</td>
</tr>
<tr>
<td valign="top"><b>Code</b></td>
<td>
<img src="assets/badges/python.svg" height="28" alt="Python">
<img src="assets/badges/fastapi.svg" height="28" alt="FastAPI">
<img src="assets/badges/typescript.svg" height="28" alt="TypeScript">
<img src="assets/badges/react.svg" height="28" alt="React">
<img src="assets/badges/svelte.svg" height="28" alt="Svelte">
<img src="assets/badges/node.svg" height="28" alt="Node.js">
<img src="assets/badges/hono.svg" height="28" alt="Hono">
<img src="assets/badges/kotlin.svg" height="28" alt="Kotlin">
<img src="assets/badges/compose.svg" height="28" alt="Jetpack Compose">
<img src="assets/badges/electron.svg" height="28" alt="Electron">
<img src="assets/badges/discordpy.svg" height="28" alt="discord.py">
<img src="assets/badges/cpp.svg" height="28" alt="C++">
<img src="assets/badges/csharp.svg" height="28" alt="C#">
<img src="assets/badges/lua.svg" height="28" alt="Lua">
<img src="assets/badges/bash.svg" height="28" alt="Bash">
<img src="assets/badges/powershell.svg" height="28" alt="PowerShell">
<img src="assets/badges/postgresql.svg" height="28" alt="PostgreSQL">
<br/><sub>Tests Vitest, Playwright, pytest · images publiées sur GHCR · Renovate · i18n FR/EN · accessibilité WCAG 2.2 AA</sub>
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
