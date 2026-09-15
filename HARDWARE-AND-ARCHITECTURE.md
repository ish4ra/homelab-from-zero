# Hardware and architecture

You do not need enterprise hardware to start a homelab.

## Start with the workload

Choose hardware after deciding what you want to run.

### Light workloads

Examples:

- DNS filtering;
- RSS;
- uptime monitoring;
- small dashboards;
- Home Assistant;
- simple web apps.

A low-power mini PC, thin client or Pi-class board can be enough.

### Moderate workloads

Examples:

- several Docker services;
- Nextcloud-style collaboration;
- Paperless-ngx;
- databases;
- small VMs;
- self-hosted development tools.

A used x86 mini PC/desktop with 16 GB RAM and SSD storage is a very comfortable starting point.

### Heavy workloads

Examples:

- 4K media transcoding;
- Immich machine-learning/indexing;
- local AI;
- many VMs;
- large search indexes;
- ZFS/storage-heavy workloads.

Plan CPU/GPU, RAM, SSD endurance and storage separately.

## CPU

For a general beginner server, modern used Intel/AMD desktop/mini-PC CPUs are usually plenty.

For Jellyfin/media, Intel CPUs with supported Quick Sync generations can be especially attractive because hardware transcoding can reduce CPU load dramatically.

## RAM

Rough beginner targets:

- 4 GB: very small/light server;
- 8 GB: workable for several light containers;
- 16 GB: comfortable general-purpose starter;
- 32 GB+: useful when VMs, databases, ZFS caching or heavier apps grow.

Do not allocate RAM to VMs/containers just because it is available. Measure usage.

## Storage

A simple pattern:

```text
SSD
- OS
- container configs
- databases
- application metadata

HDD / larger SSD
- media
- bulk files
- photo library

Separate backup target
- snapshots/backups
```

A RAID mirror is availability, **not a backup**. It does not protect you from deletion, corruption, ransomware or bad application migrations.

## Network

Gigabit Ethernet is enough for many homes.

Use wired Ethernet for:

- the main server;
- NAS/storage traffic;
- high-bitrate media clients where practical.

2.5 GbE becomes useful when moving large files between fast storage devices, but it is not mandatory for a first lab.

## Plain Linux vs Proxmox

### Plain Linux + Docker

Choose this when:

- one host runs most services;
- you want fewer abstraction layers;
- you want to learn Linux and containers directly.

### Proxmox

Choose this when:

- VMs/LXCs are a major part of the goal;
- you want snapshots/virtual networks/lab environments;
- you plan to test multiple operating systems;
- isolation between workloads matters.

Do not choose Proxmox only because every homelab diagram uses it.

## Old laptops

Laptops can make surprisingly good starter servers:

- built-in screen/keyboard;
- low idle power;
- battery acts as a tiny UPS.

Trade-offs:

- limited storage expansion;
- cooling designed for intermittent laptop workloads;
- battery aging/swelling must be monitored;
- only one Ethernet interface or none.

## Power and reliability

Useful upgrades after the lab becomes important:

- UPS;
- SMART monitoring;
- temperature monitoring;
- tested backups;
- spare boot drive or documented rebuild procedure.

Reliability comes more from recoverability than from buying expensive hardware.