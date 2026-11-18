# Install HomeOS

HomeOS runs from a prebuilt **Linux amd64** Docker image. Use **Unraid** or **Docker Compose**; you don't need the application source code.

**Image:** `ghcr.io/ozkernel/homeos:2.0.0` — available to pull once the v2.0.0 release is published. Check [GitHub Releases](https://github.com/ozkernel/homeos/releases) first.

## Unraid

### 1. Get the template

In the Unraid web interface, open **Terminal** and paste:

```sh
mkdir -p /boot/config/plugins/dockerMan/templates-user && curl -fL https://raw.githubusercontent.com/ozkernel/homeos/main/templates/homeos.xml -o /boot/config/plugins/dockerMan/templates-user/my-HomeOS.xml
```

This saves the official template where Unraid looks for user templates.

### 2. Add HomeOS

1. Open **Docker → Add Container**.
2. Select **HomeOS** under **User Templates**.
3. Check the settings below, then click **Apply**.

| Setting | Default |
| --- | --- |
| Network | Bridge |
| Web port | `3080` |
| Configuration | `/mnt/user/appdata/homeos/config` |
| Shared files | `/mnt/user/appdata/homeos/myfiles` |

Once the image is published and downloaded, the container should show **Running** in the Docker tab. If it fails, open its **Log** and check [Troubleshooting](SUPPORT.md).

### 3. Open HomeOS and create an administrator

Open `http://YOUR-UNRAID-IP:3080` in your browser. Replace `YOUR-UNRAID-IP` with the address of your Unraid server.

Follow the **first-run setup screen** to create your administrator account. On a network-accessible installation, you may be asked for a one-time setup token:

1. In Unraid, open **Docker** and click the **HomeOS** container.
2. Open **Log**, find the setup-token message, and copy the token.
3. Return to HomeOS and complete the setup screen.

The setup token expires after 24 hours and is invalidated after successful setup. For an expired token or lost administrator access, see [Account recovery](#account-recovery).

**Demo Mode** starts with sample data so you can explore before connecting services. After setting up your account, use **Settings → Data Mode** to switch to Live Mode, then add services under **Integrations**. Both steps are optional.

### Folder permissions (only if needed)

The container uses UID/GID **1000:1000**. If HomeOS cannot write to its configuration or shared-files folders, run this in the Unraid Terminal:

```sh
mkdir -p /mnt/user/appdata/homeos/config /mnt/user/appdata/homeos/myfiles
chown -R 1000:1000 /mnt/user/appdata/homeos/config /mnt/user/appdata/homeos/myfiles
```

Do not change ownership unnecessarily on an existing installation, and **never delete your data** to fix a permissions problem.

## Docker Compose

You need Docker Engine with Compose v2, or Docker Desktop using Linux containers.

### 1. Get the installation files

```sh
git clone https://github.com/ozkernel/homeos.git
cd homeos
cp .env.example .env
```

Alternatively, download the public repository ZIP and extract it, then copy `.env.example` to `.env` beside `docker-compose.yml`.

### 2. Review `.env`

By default, HomeOS listens on `127.0.0.1:3080`, accessible **from the Docker host only**. Leave the administrator fields empty to use the first-run setup screen.

You can change the port with `HOMEOS_HOST_PORT`. Keep `.env` private and see [Configuration](CONFIGURATION.md) for optional settings.

### 3. Start HomeOS

```sh
docker compose config --quiet
docker compose pull
docker compose up -d
docker compose ps
```

Open **http://127.0.0.1:3080** on the Docker host and create your administrator account.

### 4. Connect services (optional)

In HomeOS, open **Settings → Data Mode** to switch to Live Mode, then **Integrations** to add the services you already use. You can keep Demo Mode if you just want to explore.

## Access from other devices

The supplied Compose file binds to `127.0.0.1` by default. For access from devices on a **trusted LAN**, change this line in `.env`:

```dotenv
HOMEOS_BIND=0.0.0.0
```

Then recreate HomeOS:

```sh
docker compose up -d
```

Open `http://YOUR-DOCKER-HOST-IP:3080`. Initial setup over the LAN may require the one-time token shown by `docker compose logs homeos`.

**Do not forward port 3080 directly to the internet.** For remote access, use a VPN or a properly configured HTTPS reverse proxy. Only add trusted proxy addresses to `HOMEOS_TRUSTED_PROXIES`. See [Configuration](CONFIGURATION.md).

## Backups and updates

Back up **both** storage locations before updating:

| Data | In the container | Unraid default |
| --- | --- | --- |
| Database, settings, encryption key | `/config` | `/mnt/user/appdata/homeos/config` |
| Shared files, notes, wiki and music | `/app/data/myfiles` | `/mnt/user/appdata/homeos/myfiles` |

With the supplied Compose file, these are **Docker volumes** (`homeos_config` and `homeos_myfiles`) by default, unless you set `MYFILES_HOST_PATH`. Stop HomeOS before taking a consistent backup and back up the actual volumes or mapped host paths. Keep the database and encryption key together.

For **Unraid**, change the image tag in the container's **Edit** screen to a newer *published* version, then apply the update. A pinned `2.0.0` tag will not automatically become `2.0.1`.

For **Compose**, change `HOMEOS_IMAGE` in `.env` to a newer published version, then run:

```sh
docker compose pull
docker compose up -d
```

Keep your existing volumes, file paths and configuration. **Do not run `docker compose down -v` during an update**; it removes named volumes.

## Account recovery

If you lose administrator access or need a fresh setup token, run the recovery command locally:

```sh
# Docker Compose
docker compose exec homeos node dist/recover.mjs

# Unraid (use your container name if different)
docker exec -it homeos node dist/recover.mjs
```

Follow the instructions it prints. Back up your configuration first, and do not share the recovery token.

[Settings](CONFIGURATION.md) · [Troubleshooting](SUPPORT.md) · [Security](SECURITY.md) · [Home](README.md)
