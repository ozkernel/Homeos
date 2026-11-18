# Settings and integrations

HomeOS starts in **Demo Mode** with sample data. After creating an administrator account, you can switch to **Live Mode** in **Settings → Data Mode** and connect your own services in **Integrations**. Everything can be set up gradually.

[Install HomeOS](INSTALLATION.md) · [Help](SUPPORT.md)

## Connect your services

Open **Integrations**, choose a service, enter its address and credentials, and use **Test connection** before saving.

Use an address the **HomeOS container** can reach. `localhost` inside a container refers to that container, not another server on your network.

| Service | What you need |
| --- | --- |
| Unraid | Unraid URL and an API key with the needed permissions. |
| Proxmox VE | HTTPS endpoint, token ID and token secret. Read-only access can use `PVEAuditor`; guest power controls additionally need `VM.PowerMgmt` and must be enabled in HomeOS. |
| UniFi | UniFi Network URL and API key. For local HTTPS, choose **WAN port** or automatic detection. |
| AdGuard Home | Web URL, username and password. |
| Plex and Jellyfin | Server URL and a Plex token or Jellyfin API key. |
| Seerr | URL and Seerr API key or local account. For posters, enable **Image Caching** in Seerr and allow its `/imageproxy` route. |
| Sonarr, Radarr and Prowlarr | Base URL and API key. Keep any proxy path; don't add API suffixes. |
| qBittorrent | Web UI URL and credentials, or an API key with qBittorrent 5.2 or later. |

All integrations are optional. For self-signed certificates on local Proxmox or UniFi connections, verify the certificate before accepting it. Only give each integration the permissions it needs.

## Accounts, appearance and diagnostics

- **Accounts:** Use **Settings → User management** to add administrators and viewers. Review or revoke sign-ins under **Active sessions**.
- **Appearance:** Use **Customisation** to choose themes, icons and widgets.
- **Diagnostics:** Find the running version and status in **Settings → Diagnostics**. Use `docker compose logs homeos` or Unraid's container **Log** for more detail.
- **Administrator recovery:** Follow [Account recovery](INSTALLATION.md#account-recovery) if you lose access.

## Docker settings

For **Docker Compose**, edit `.env` beside `docker-compose.yml`, then run `docker compose up -d`. For **Unraid**, edit the container settings and mapped directories.

| Setting | Purpose |
| --- | --- |
| `HOMEOS_IMAGE` | Docker image tag; supplied default is `ghcr.io/ozkernel/homeos:2.0.0`. |
| `HOMEOS_BIND` | Compose: `127.0.0.1` (local only) or `0.0.0.0` (trusted LAN). |
| `HOMEOS_HOST_PORT` | Host web port; default `3080`. |
| `HOMEOS_DEMO` | Initial Demo Mode for a new database. Later changes are saved in Settings. |
| `ADMIN_USERNAME`, `ADMIN_PASSWORD` | Optional initial administrator credentials for automated setup. Leave blank for interactive first-run setup. |
| `ADMIN_PASSWORD_FILE` | Optional password-file **path inside the container** for automated setup. |
| `MYFILES_HOST_PATH` | Optional absolute host directory for shared files in Compose. |
| `MYFILES_STORAGE_PATH` | Shared-files directory in the container; default `/app/data/myfiles`. |
| `HOMEOS_HTTPS_ONLY` | Require HTTPS for authentication and protected endpoints when configured appropriately. |
| `HOMEOS_TRUSTED_PROXIES` | Comma-separated trusted proxy IPs or CIDR ranges. |
| `HOMEOS_LOG_LEVEL` | Logging level; default `INFO`. |
| `TZ` | Container timezone; supplied example uses `Australia/Melbourne`. |

For automated setup, remove the bootstrap password or secret from your deployment configuration once setup is complete. Never publish `.env`, API keys or setup tokens.

## Storage and safe access

- HomeOS keeps its database, settings and encryption key under `/config`.
- Shared notes, files, wiki pages and music live under `/app/data/myfiles` by default. Move or rename library items through HomeOS.
- The supplied Compose file uses named Docker volumes unless you set `MYFILES_HOST_PATH`. Unraid maps host folders directly.
- Back up **both** locations before changing storage paths or updating. See [Backups and updates](INSTALLATION.md#backups-and-updates).

For remote use, prefer a VPN or secured HTTPS reverse proxy. Only trust your own proxy addresses in `HOMEOS_TRUSTED_PROXIES`; configure the proxy to preserve `Host`, forward `X-Forwarded-For` and `X-Forwarded-Proto`, and support WebSocket upgrades on `/api/monitor`.

[Installation](INSTALLATION.md) · [Help](SUPPORT.md) · [Home](README.md)
