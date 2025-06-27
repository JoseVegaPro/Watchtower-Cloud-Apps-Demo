# Watchtower Cloud Apps Demo


This repository provides a Docker Compose setup to demonstrate a multi-service environment using Traefik as a reverse proxy for automatic HTTPS and routing.

![Architecture Diagram](https://github.com/JoseVegaPro/Watchtower-Cloud-Apps-Demo/blob/main/Diagram.png?raw=true)



## Services Included

- **Traefik**: Reverse proxy and TLS termination (Let's Encrypt).
- **OpenSpeedTest**: Lightweight internet speed test server (available at `speed.YOURDOMAIN`).
- **Owncast**: Self-hosted live video streaming platform (available at `stream.YOURDOMAIN`).
- **Firefox Container**: GUI-based browser accessible via VNC (available at `ffo.YOURDOMAIN`).

## Requirements

- Docker & Docker Compose installed
- A domain name with DNS pointing to your server
- External Docker network named `traefik_primary`
- TLS certificates managed via Let's Encrypt
- A valid email configured in `traefik.yml` for Let's Encrypt certificate registration

## Setup

1. Clone this repository.
2. Replace all instances of `USEYOUROWNDOMAIN` in the `.yaml` file with your actual domain.
3. Ensure the `traefik.yml` (with a valid email) and `acme.json` are correctly mounted and configured.
4. Launch the services:

```bash
docker-compose -f "Cloud Environment.yaml" up -d
```

## Traefik Configuration

Make sure you have a `traefik.yml` file in place with the following considerations:

- **EntryPoints**: `web` on port 80 and `websecure` on port 443 are defined.
- **Dashboard**: Enabled and exposed on port 8180 (`insecure: true`). Secure this before deploying to production.
- **CertificatesResolvers**: Configured for both staging and production using Let's Encrypt.
- **ACME Storage**: TLS certificates are saved to `acme.json`.
- **Providers**: Uses Docker and file providers for dynamic configuration.

## Port Overview

| Port | Purpose                |
|------|------------------------|
| 80   | HTTP (Traefik)         |
| 443  | HTTPS (Traefik)        |
| 8180 | Traefik Dashboard      |
| 3006 | OpenSpeedTest          |
| 1935 | Owncast RTMP           |
| 3003 | Firefox (VNC interface) |

## License

MIT License