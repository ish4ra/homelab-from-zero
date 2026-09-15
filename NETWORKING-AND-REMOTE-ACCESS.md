# Networking and remote access

You do not need to become a network engineer to run a home server, but a few concepts matter a lot.

## The minimum vocabulary

- **LAN IP** — private address of a device inside your home network.
- **DHCP** — normally gives devices their LAN addresses.
- **DHCP reservation** — makes the router consistently give your server the same address.
- **DNS** — converts names to IP addresses.
- **Port** — identifies a network service on a host.
- **Gateway/router** — connects your LAN to other networks/the internet.
- **Public IP** — the internet-facing address provided by your ISP; CGNAT can change what inbound access is possible.

## First step: predictable LAN address

For most beginners, create a DHCP reservation in the router for the server rather than hard-coding a static address inside Linux immediately.

Example:

```text
Router: 192.168.1.1
Server: 192.168.1.20
Laptop: 192.168.1.50
```

Then local services can be reached predictably, such as:

```text
http://192.168.1.20:8096
```

## Private remote access first

The easiest safe mental model is:

```text
Phone/laptop away from home
        |
   encrypted VPN
        |
     home LAN
        |
   private services
```

Good options include WireGuard-based networking and Tailscale-style mesh VPNs.

Benefits:

- no need to expose every app publicly;
- services can keep using private LAN/VPN addresses;
- easier security model for personal/admin services.

## Public services

Sometimes you genuinely want a normal HTTPS URL accessible to other people.

Typical architecture:

```text
Internet
   |
DNS name
   |
Router / ingress path
   |
Reverse proxy (Caddy)
   |
App on internal port
```

A reverse proxy can terminate HTTPS and forward requests to internal applications.

## Why Caddy is beginner friendly

Caddy can automatically handle TLS certificates for many normal domain setups and has a relatively small configuration surface.

Conceptual Caddyfile:

```text
app.example.com {
    reverse_proxy 127.0.0.1:3000
}
```

Do not copy this blindly without understanding your DNS, firewall and application authentication.

## Do not expose admin interfaces casually

Keep these private unless you have a strong reason otherwise:

- Proxmox admin UI;
- router admin page;
- Docker management socket/UI;
- SSH;
- database ports;
- backup dashboards;
- internal monitoring/admin tools.

## CGNAT

If your ISP uses carrier-grade NAT, classic inbound port forwarding may not work even if the router configuration looks correct.

Private mesh VPN solutions often work around this for personal access without requiring inbound port forwarding.

## DNS filtering

AdGuard Home, Pi-hole and Blocky can provide local DNS filtering/control.

A DNS filter is useful infrastructure, but remember:

- if every client depends on it, its outage affects the whole home;
- configure reliable upstream DNS;
- know how to temporarily bypass it while troubleshooting.

## VLANs are optional

VLANs are valuable for isolating IoT, guests and lab networks, but they are not a requirement for your first server.

Learn one flat LAN well first. Add segmentation when you have a reason and networking hardware that supports it.

## Remote-access checklist

Before making an app public, ask:

1. Does this really need to be public?
2. Could I use a VPN instead?
3. Does the app have strong authentication?
4. Is HTTPS configured correctly?
5. Is the app updated?
6. Is the admin interface separately protected/private?
7. Do I have logs/monitoring?
8. Do I know how to disable access quickly if something goes wrong?

Private by default is a very good homelab policy.