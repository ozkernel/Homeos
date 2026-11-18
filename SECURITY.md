# Security

HomeOS connects to your own servers and services. Protect it like any other self-hosted application.

- **Use a trusted LAN or VPN.** Do not forward the HomeOS HTTP port directly to the internet.
- **Use HTTPS for remote access** behind a correctly configured reverse proxy.
- **Protect accounts and setup tokens.** Never share passwords, cookies, API keys or recovery tokens.
- **Keep your data private.** Back up the configuration and shared-file locations securely.
- **Limit integration permissions.** Use only the access each connected service needs.
- **Keep HomeOS and connected services updated** when releases are available.

## Report a vulnerability

**Please don't publish security details in a GitHub Issue.**

Use [GitHub private vulnerability reporting](https://github.com/ozkernel/homeos/security/advisories/new) if it is enabled. Otherwise, open a public issue requesting a private contact method **without including vulnerability details**, and wait for a private channel.

When reporting privately, include the affected HomeOS version, feature and safe reproduction steps. Redact personal information and all credentials.

[Help](SUPPORT.md) · [Home](README.md)
