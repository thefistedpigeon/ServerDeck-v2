# ServerDeck 2.0.0

ServerDeck is a lightweight, self-contained web interface for managing Debian-family home servers and NAS systems. It is designed to keep the server itself simple: one Python file, a systemd service, on-demand monitoring, and optional add-ons only when you need them.

ServerDeck 2.0.0 targets **Debian, Ubuntu, and Raspberry Pi OS** with **Python 3.11 or newer**.

## Highlights

- Single-file Python application with an integrated HTTPS web interface.
- Linux/PAM sign-in using existing administrator accounts; optional TOTP MFA.
- Lightweight Overview and Monitor pages with on-demand / adaptive polling rather than a heavyweight monitoring stack.
- APT update management, security-only updates, scheduled updates, full upgrade, and cleanup actions.
- Guided disk preparation and persistent mounting without manually editing `/etc/fstab`.
- Network management for NetworkManager, Netplan, and ifupdown environments, including IPv4 configuration, Wi-Fi scanning/connection, and rollback protection for network changes.
- Samba share management, Linux users and groups, and guided rsync backups with dry-run validation and optional systemd schedules.
- File Browser with upload, download, create, rename, delete, permissions, and server-side URL downloads.
- Interactive browser terminal with persistent command history and automatic session startup.
- Docker management with container creation/reconfiguration, Compose import, image/network management, MACVLAN/IPVLAN guided networking, own-IP containers, logs, relationships, update/recreate workflows, and Quick Apps.
- Optional add-ons for Docker, Copyparty, MiniDLNA, DNSMASQ, WireGuard, DuckDNS, Tailscale, SMART Tools, and TuneD.
- Persistent Activity drawer for long-running administrative actions and a Needs Attention notification system.
- Unified dark/light material-style interface with configurable accent colour and font size.

## Requirements

- Debian, Ubuntu, or Raspberry Pi OS.
- Python 3.11+.
- systemd.
- Root access for installation and host-management operations.
- `openssl` for the self-signed HTTPS certificate. The installer checks for it.
- A Linux user that is root or belongs to an administrative group such as `sudo`, `admin`, or `wheel`.

Optional features install or use their own system packages only when required.

## Install

Download `serverdeck-v2.0.0.py`, then run:

```bash
chmod +x serverdeck-v2.0.0.py
sudo ./serverdeck-v2.0.0.py --install
```

The installer asks which HTTPS port to use. The default is **8443**.

Once installation completes, open the displayed HTTPS address in your browser and sign in with an authorised Linux account.

ServerDeck creates a self-signed TLS certificate on first install. Your browser will therefore show a certificate warning until you explicitly trust that certificate.

### Non-interactive port selection

```bash
sudo ./serverdeck-v2.0.0.py --install --port 8443
```

## Upgrade from an earlier ServerDeck v2 build

Run the new release with `--install` again:

```bash
sudo ./serverdeck-v2.0.0.py --install
```

ServerDeck updates the installed application and service while preserving existing configuration and state. The internal installation paths and `servermanager.service` service name are intentionally retained for upgrade compatibility.

Before a major storage or network change, keep an independent backup of important server data and configuration.

## Uninstall

Remove the application and service while preserving ServerDeck configuration and state:

```bash
sudo ./serverdeck-v2.0.0.py --uninstall
```

Remove the application **and** ServerDeck configuration, certificates, state, and terminal history:

```bash
sudo ./serverdeck-v2.0.0.py --uninstall --purge
```

## Core interface

### Overview

Shows hostname, operating system, uptime, CPU, memory, storage, network activity, SMART state when available, reboot requirements, and items that need attention.

### Monitor

Provides focused process and service management without a continuously running metrics database.

### Manage

The Manage workspace brings the core server-administration tools together:

- **Disks** — inspect physical drives and partitions, prepare storage, format supported filesystems, and create persistent UUID-based mounts.
- **Network** — inspect interfaces, switch between DHCP and static IPv4 settings, scan/connect Wi-Fi, and use timed rollback protection when changing active network configuration.
- **Shares** — create and manage Samba shares with public, authenticated, or advanced access controls.
- **Users** — create and manage Linux users, groups, passwords, and memberships.
- **Backups** — create rsync backup jobs, validate them with a dry run, run them manually, or schedule them with systemd timers.

### Updates

ServerDeck can refresh APT metadata, show available upgrades, install security-only updates, install normal upgrades, run full-upgrade, clean obsolete packages, and create unattended update schedules. Scheduled updates do not reboot the server automatically.

### File Browser

Browse local files and folders, upload/download data, create and rename entries, change permissions/ownership, and download files directly to the server from HTTP/HTTPS URLs.

### Terminal

The terminal starts automatically when the page opens, supports a persistent shell session, command history, new-session/close controls, paste, resize, and adaptive polling. The terminal runs with the privileges of the ServerDeck service, so commands can modify or destroy the host system.

## Docker

The Docker add-on provides a simpler container workflow than a full container-management suite while retaining direct control over the settings that matter on a home server.

Features include:

- Containers, images, and Docker networks.
- Guided container creation and reconfiguration.
- Port, volume, environment, capability, sysctl, restart-policy, and resource settings.
- Bridge networking plus guided own-IP networking with MACVLAN/IPVLAN.
- Automatic network defaults and IP availability checks.
- Compose YAML import for multi-container applications.
- Container update/recreate flow with rollback-oriented handling.
- Container details, recent logs, mounts, networks, and ServerDeck-detected relationships.

### Quick Apps

The 2.0.0 Quick Apps catalogue includes:

- Heimdall
- Jellyfin
- LibreSpeed
- NGINX
- NGINX Proxy Manager
- Resilio Sync
- Syncthing
- Deluge
- OpenCloud
- IT Tools
- Caddy
- AdGuard Home
- Minecraft / Crafty Controller
- Easy WireGuard (`wg-easy`)

Quick Apps pre-fill a reviewed container configuration. You should still review storage paths, ports, networking, environment values, and privileges before creating the container.

## Add-ons

Optional add-ons keep the base ServerDeck installation small:

- **Docker** — containers, Compose, networks, and Quick Apps.
- **Copyparty** — file sharing with users, per-folder access, optional thumbnails and SFTP, managed update/rollback, and service controls.
- **MiniDLNA** — lightweight DLNA media sharing.
- **DNSMASQ** — local DNS records/wildcards plus optional DHCP. DHCP requires a verified static LAN address.
- **WireGuard** — guided ServerDeck-to-ServerDeck VPN pairing and optional routed site-to-site networks.
- **DuckDNS** — dynamic DNS updates using a lightweight ServerDeck-owned systemd timer.
- **Tailscale** — CGNAT-friendly remote access and optional LAN subnet routing.
- **SMART Tools** — on-demand disk health, temperature, and power-on information.
- **TuneD** — server power/performance profile management.

## Security model

ServerDeck is a privileged server-management application. The installed service runs with the permissions required to perform administrative operations.

Security measures in 2.0.0 include:

- Linux/PAM authentication rather than a separate ServerDeck password database.
- Optional TOTP MFA and one-time recovery codes.
- Secure, HttpOnly, SameSite session cookies.
- CSRF and same-origin checks for state-changing requests.
- Login throttling.
- Re-authentication for high-risk operations.
- Session binding to the client IP address.
- Self-signed HTTPS by default.
- Destructive storage/network actions are gated and network configuration has timed rollback protection.

ServerDeck is designed primarily for direct administration on a trusted LAN or through a private VPN. Do not expose the management interface directly to the public Internet unless you understand and independently secure the deployment.

## Useful commands

Check the installed service:

```bash
sudo systemctl status servermanager.service
```

View recent ServerDeck logs:

```bash
sudo journalctl -u servermanager.service -n 100 --no-pager
```

Regenerate the self-signed certificate:

```bash
sudo ./serverdeck-v2.0.0.py --regenerate-certificate
```

Recover an account that has lost its ServerDeck MFA factor:

```bash
sudo ./serverdeck-v2.0.0.py --disable-mfa USERNAME
```

Show the release version:

```bash
./serverdeck-v2.0.0.py --version
```

## Important paths

ServerDeck retains its established internal paths for upgrade compatibility:

- Application: `/opt/servermanager/server_manager.py`
- Main configuration: `/etc/servermanager/config.json`
- Certificates: `/etc/servermanager/certs/`
- Runtime/persistent state: `/var/lib/servermanager/`
- systemd service: `servermanager.service`

Some add-ons create additional configuration and systemd files appropriate to those services.

## Release documentation

- [Changelog](CHANGELOG.md)
- [ServerDeck 2.0.0 release notes](RELEASE_NOTES_v2.0.0.md)

