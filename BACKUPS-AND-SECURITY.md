# Backups and security basics

A homelab is useful only if you can recover it.

## The first rule

**RAID is not a backup.**

RAID can keep a service running after a disk failure. It does not protect you from:

- accidental deletion;
- ransomware;
- database corruption;
- a bad application migration;
- a broken script;
- theft/fire/electrical damage;
- deleting the wrong dataset.

## Use a 3-2-1 mindset

A useful target is:

- 3 copies of important data;
- on 2 different storage/media locations;
- with 1 copy off the primary server/site where practical.

You do not need a perfect enterprise implementation on day one, but you should avoid a single point of total loss.

## What to back up

For containerized services, usually back up:

- Compose files;
- `.env`/configuration secrets securely;
- bind-mounted app data;
- database dumps or application-consistent database backups;
- uploaded media/documents that cannot be recreated;
- reverse-proxy/DNS configuration;
- notes describing how to rebuild the server.

Do not assume copying a live database file is always a valid consistent backup. Check the application's backup documentation.

## Good tools

### Restic

Strong general-purpose encrypted backup tool with snapshots, retention policies and many storage backends.

### Kopia

Another capable snapshot/encrypted backup tool with a friendly workflow and multiple storage targets.

### Proxmox Backup Server

Excellent if your homelab is heavily based on Proxmox VMs/containers and you have appropriate separate storage/hardware.

## Test restores

A backup that has never been restored is only a theory.

Test at least one representative restore:

1. choose a small service;
2. stop it if required for consistency;
3. restore to a temporary location/VM;
4. start the restored app;
5. confirm users/data/settings actually work.

## Security basics

### Keep the host updated

Patch the base OS, container runtime, reverse proxy and internet-facing applications.

### Use SSH keys

Prefer key-based SSH authentication. If password login remains enabled, use strong credentials and do not expose SSH to the public internet without a clear reason.

### Minimize public exposure

Use private VPN-style access for admin/internal services.

### Use unique credentials

Do not reuse the same admin password across Jellyfin, Proxmox, router, dashboards and other services.

### Use MFA where supported

Especially for public-facing services and accounts that can reset/administer other systems.

### Separate service privileges

Run apps with the minimum permissions they require. Avoid privileged containers unless the application truly needs them.

### Back up secrets securely

Your recovery plan may require:

- encryption keys;
- TOTP recovery codes;
- DNS/API tokens;
- database passwords;
- backup repository passwords.

If those exist only on the failed server, the backup may be useless.

## Monitoring

At minimum, monitor:

- disk capacity;
- SMART/disk health;
- backup success/failure;
- service uptime;
- CPU/RAM/temperature if relevant.

Uptime Kuma, Gatus and Beszel are good small-lab tools for different parts of this.

## Before every major change

Use this checklist:

```text
[ ] Current backup exists
[ ] Restore path is understood
[ ] Release notes read
[ ] Enough free disk space
[ ] Config/compose files saved
[ ] Rollback limitations understood
[ ] Maintenance window is acceptable
```

Boring backups are the feature that makes the fun parts of a homelab safe.