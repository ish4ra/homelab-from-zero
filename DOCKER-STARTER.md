# Docker Compose starter

Docker Compose gives you a repeatable way to define a small stack in a text file.

## Why Compose is a good beginner tool

Instead of remembering a long `docker run` command, you keep configuration in `compose.yml`.

Example:

```yaml
services:
  whoami:
    image: traefik/whoami:latest
    restart: unless-stopped
    ports:
      - "8080:80"
```

Start it with:

```bash
docker compose up -d
```

See status:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs -f
```

Stop/remove the containers while keeping declared persistent data:

```bash
docker compose down
```

## Use one directory per stack

A simple layout:

```text
/srv/compose/
├── monitoring/
│   └── compose.yml
├── jellyfin/
│   └── compose.yml
└── mealie/
    └── compose.yml
```

Keep secrets out of public Git repositories.

## Persistent data

Containers are disposable. Your data should not be.

Two common patterns:

### Bind mount

```yaml
volumes:
  - ./config:/config
```

Easy to see and back up.

### Named volume

```yaml
volumes:
  - appdata:/data

volumes:
  appdata:
```

Also valid, but understand where Docker stores it and how you will back it up.

## Permissions

A large percentage of beginner Docker problems are ownership/permission problems.

When a container cannot read/write a mounted folder:

- check host ownership;
- check container UID/GID requirements;
- do not "fix" everything with `chmod 777`;
- read the image's upstream documentation.

## Image tags

`latest` is convenient but can introduce unexpected major changes.

For important services, consider pinning a major/minor version and reviewing release notes before upgrades.

## Updates

A basic manual pattern:

```bash
docker compose pull
docker compose up -d
```

Before upgrading stateful services:

1. read release notes;
2. take a backup;
3. understand database migration/rollback limitations;
4. update;
5. check logs and app health.

## Do not expose everything

If a port only needs to be reached by other containers, you often do not need to publish it to the host at all.

A Compose network lets containers communicate by service name.

## Docker socket warning

Mounting `/var/run/docker.sock` into a container can effectively grant very powerful control over the host.

Do it only when the application genuinely needs it and you understand the security implications.

## A sensible first stack

Start with one app plus monitoring rather than ten apps.

Example learning sequence:

```text
1. Uptime Kuma
2. AdGuard Home or a simple web app
3. Jellyfin / Mealie / Miniflux — something you will use
4. Backups
5. Reverse proxy or VPN access
```

The goal is to understand your deployment well enough that you can rebuild it.