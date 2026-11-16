<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/icon-light.svg">
    <img src="assets/icon.svg" alt="Syncthing logo" width="128">
  </picture>
</p>

<h1 align="center">aac-syncthing</h1>

<p align="center">Self-hosted deployment definition for Syncthing, a decentralized continuous file synchronization tool.</p>

<p align="center">
  <a href="https://github.com/syncthing/syncthing"><img alt="Upstream" src="https://img.shields.io/badge/upstream-syncthing%2Fsyncthing-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-lscr.io%2Flinuxserver%2Fsyncthing%3Av2.0.15--ls210-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/syncthing/syncthing/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/badge/license-MPL--2.0-green"></a>
</p>

## About

Syncthing keeps folders in sync between devices without a central server. Devices find each
other directly (or through discovery), exchange changes over encrypted connections and keep
every copy up to date. This deployment runs Syncthing as a long-lived service on a host, so
that other devices can sync their folders with it.

Key features (as provided by upstream):

- Decentralized, peer-to-peer synchronization without a cloud intermediary
- End-to-end encrypted transfers between devices
- Local discovery and global discovery of peers
- Versioning of replaced or deleted files
- Web GUI for managing folders and devices

## Upstream

- Project: <https://syncthing.net>
- Source code: <https://github.com/syncthing/syncthing>
- Container image: `lscr.io/linuxserver/syncthing:v2.0.15-ls210` (LinuxServer.io build)
- Image packaging: <https://github.com/linuxserver/docker-syncthing>
- Documentation: <https://docs.syncthing.net>
- License: MPL-2.0 (upstream project)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. For a
manual run, the external networks used for routing must exist first:

```bash
docker network create services-secured
docker network create services-exposed

docker compose up -d
```

This service defines no secrets, so no `.env` or `stack.env` file is required.

## Configuration

Environment variables (defaults from `service.yml`):

| Variable | Default | Description |
|---|---|---|
| `PUID` | `3001` | User ID the container runs as |
| `PGID` | `3001` | Group ID the container runs as |
| `TZ` | `Europe/Berlin` | Time zone |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `8384` | TCP | Web GUI (`web`) |
| `22000` | TCP | Sync protocol (`sync-tcp`) |
| `22000` | UDP | Sync protocol over QUIC (`sync-udp`) |
| `21027` | UDP | Local discovery broadcasts (`discovery`) |

Volumes:

| Mount | Description |
|---|---|
| `config` -> `/config` | Syncthing application configuration |
| `data` -> `/public` | Shared sync data folder |
| `private` -> `/private` | Private sync data folder |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes, dependencies and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-syncthing>.

## Credits and license

Syncthing is developed by the Syncthing project and licensed under MPL-2.0. The container
image is packaged by LinuxServer.io. This repository only contains the deployment definition
for it and is not a fork of the application. The logo is taken from the icon collection
listed in [`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md).
