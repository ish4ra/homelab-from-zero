<div align="center">

# Homelab From Zero

**A beginner-friendly path from "I have an old PC" to a useful, backed-up, remotely accessible home server.**

No rack required. No Kubernetes required. No need to install 40 containers on day one.

</div>

---

## What this guide teaches

- choosing hardware you already own or can buy cheaply;
- deciding between plain Linux, Docker and Proxmox;
- basic LAN/IP/DNS concepts that actually matter;
- running services with Docker Compose;
- storage and permissions without mystery;
- private remote access with WireGuard/Tailscale-style networking;
- reverse proxy + HTTPS when a service genuinely needs public access;
- backups that can survive a disk/server mistake;
- monitoring and update habits;
- a sensible first set of self-hosted apps.

## The simplest mental model

```text
Internet
   |
Router / Firewall
   |
Home LAN
   |
Homelab server
   |-- Docker / containers
   |     |-- App 1
   |     |-- App 2
   |     `-- Monitoring
   |
   |-- App/config storage
   `-- Backups -> another disk / another machine / off-site copy
```

## Pick your starting platform

| Situation | Good starting point |
|---|---|
| One PC, want simplicity | **Debian/Ubuntu Server + Docker Compose** |
| Want VMs/LXCs and lab experimentation | **Proxmox VE** |
| NAS-first build | A storage-focused OS/platform you understand + containers/VMs where appropriate |
| Raspberry Pi / very small ARM box | Linux + Docker/Podman, keep services lightweight |

For a first server, plain Linux + Docker Compose is often easier to understand than adding a hypervisor immediately.

## 7-stage roadmap

### 1. Hardware

Start with what you have: old desktop, mini PC, laptop, small server or Pi-class board.

Read **[HARDWARE-AND-ARCHITECTURE.md](HARDWARE-AND-ARCHITECTURE.md)**.

### 2. Install the base OS

For a simple server:

- Debian or Ubuntu Server;
- SSH enabled;
- a normal admin user;
- static DHCP lease / predictable LAN IP.

For a virtualization-focused lab, Proxmox can be a better base.

### 3. Learn the network basics

You only need a few concepts at first:

- LAN IP;
- router/default gateway;
- DNS;
- ports;
- DHCP reservation;
- private vs public exposure.

Read **[NETWORKING-AND-REMOTE-ACCESS.md](NETWORKING-AND-REMOTE-ACCESS.md)**.

### 4. Run your first container

Use Docker Compose so the configuration lives in a file you can back up and understand.

Read **[DOCKER-STARTER.md](DOCKER-STARTER.md)**.

### 5. Install one useful service

Good first choices:

- **Uptime Kuma / Gatus** — monitoring;
- **Jellyfin** — personal media;
- **AdGuard Home / Pi-hole** — DNS filtering;
- **Syncthing** — file synchronization;
- **Mealie** — recipes;
- **Miniflux / FreshRSS** — RSS.

For a curated list, see **[selfhosted-picks](https://github.com/ish4ra/selfhosted-picks)**.

### 6. Add backups before adding more apps

Your Docker Compose file is not a backup of the database/data inside the container.

Read **[BACKUPS-AND-SECURITY.md](BACKUPS-AND-SECURITY.md)** before the server becomes important.

### 7. Remote access

Prefer private VPN-style access first. Public reverse-proxy exposure should be a deliberate choice, not the default.

## A good first-month setup

```text
Week 1
- Linux/Proxmox installed
- predictable LAN IP
- SSH works

Week 2
- Docker Compose
- one useful service
- Uptime Kuma or Gatus

Week 3
- backup job
- restore test
- Tailscale/WireGuard-style private access

Week 4
- add a second/third service only if you have a real use for it
```

## What not to do on day one

- expose Docker socket casually;
- port-forward every web app;
- run databases with no persistent volume plan;
- store the only backup on the same disk as the server;
- copy giant Compose stacks you do not understand;
- install Kubernetes because a homelab diagram looked impressive;
- make your password manager the first service before you understand backups/restores.

## Strong beginner stack

| Layer | Pick |
|---|---|
| Base OS | Debian / Ubuntu Server |
| Containers | Docker Compose |
| Private access | WireGuard / Tailscale |
| Reverse proxy | Caddy |
| Monitoring | Uptime Kuma / Gatus / Beszel |
| DNS | AdGuard Home / Pi-hole |
| Backup | Restic / Kopia + another storage target |
| Media | Jellyfin |
| Photos | Immich (after backups/storage are understood) |

## Related repos

- **[selfhosted-picks](https://github.com/ish4ra/selfhosted-picks)** — what is actually worth hosting
- **[jellyfin-media-server-guide](https://github.com/ish4ra/jellyfin-media-server-guide)** — focused Jellyfin setup
- **[open-source-alternatives](https://github.com/ish4ra/open-source-alternatives)** — alternatives to popular proprietary tools
- **[android-foss-starter-kit](https://github.com/ish4ra/android-foss-starter-kit)** — FOSS-first Android setup

---

Build the boring foundations first. A homelab becomes fun when you can break an app without losing your data.