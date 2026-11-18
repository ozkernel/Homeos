# Help and troubleshooting

Having trouble with HomeOS? Start with the checks below.

[Installation](INSTALLATION.md) · [Settings](CONFIGURATION.md) · [Report a bug](BUG_REPORTING.md)

## Common fixes

| Problem | What to check |
| --- | --- |
| Docker cannot pull HomeOS | Confirm that `ghcr.io/ozkernel/homeos:2.0.0` has been published. Check [Releases](https://github.com/ozkernel/homeos/releases). |
| HomeOS won't open | Confirm the container is **Running**, the web port is correct and your firewall allows access. Compose defaults to local-only access. |
| First-time setup asks for a token | Open the HomeOS container **Log**, or run `docker compose logs homeos`, to find the one-time token. |
| Setup token expired or administrator locked out | Use [Account recovery](INSTALLATION.md#account-recovery) locally. |
| Settings or files do not save | Check write permissions on `/config` and the shared-files storage. Unraid uses UID/GID `1000:1000`. |
| An integration is offline | Check its URL, credentials, certificate and network reachability from the **container**. |
| Seerr posters are missing | Check Seerr's **Image Caching** and `/imageproxy` access. |
| Can't access HomeOS from another device | Follow [LAN access](INSTALLATION.md#access-from-other-devices); do not expose plain HTTP publicly. |

## Check the logs

For Docker Compose:

```sh
docker compose ps
docker compose logs --tail=100 homeos
```

For Unraid, use **Docker → HomeOS → Log**. Never share unredacted passwords, tokens, cookies, personal details or `.env` files.

## Need more help?

Use [GitHub Issues](https://github.com/ozkernel/homeos/issues) to report an ordinary bug, following the [bug-reporting guide](BUG_REPORTING.md). Include the HomeOS version from **Settings → Diagnostics**, your Docker platform, the feature affected and safe steps to reproduce it.

**Security issues:** Follow [private reporting guidance](SECURITY.md), not public Issues.

HomeOS is an independently maintained project; response times and fixes are not guaranteed.

[Home](README.md)
