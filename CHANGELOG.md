# Changelog

All notable changes for the ServerDeck v2 line are documented here.

## [2.0.0] - 2026-09-16

ServerDeck 2.0.0 is the first stable release of the v2 application line.

### Added

- New unified ServerDeck v2 interface with Overview, Updates, Monitor, Manage, File Browser, Apps, Terminal, Options, Add-ons, Notifications, Power controls, and persistent Activity output.
- Lightweight CPU, memory, storage, SMART, service, process, and network monitoring designed around on-demand reads and adaptive polling.
- Guided disk preparation workflow with persistent UUID-based mounts and filesystem-specific options.
- NetworkManager, Netplan, and ifupdown support with DHCP/static IPv4 configuration, Wi-Fi scanning/connection, and timed rollback protection.
- Samba share management and Linux user/group management.
- Guided rsync backups with mandatory dry-run validation and optional systemd schedules.
- APT update management, security-only upgrades, normal upgrades, full upgrade, cleanup, reboot-required detection, and scheduled unattended updates.
- File Browser with upload, download, create, rename, delete, ownership/permission changes, and server-side HTTP/HTTPS downloads.
- Interactive xterm-based browser terminal with automatic session startup, adaptive polling, and reusable command history.
- Docker management with image/network/container views, Compose import, container reconfiguration, update/recreate workflow, MACVLAN/IPVLAN networking, own-IP guidance, IP checks, logs, and relationship inspection.
- Docker Quick Apps for Heimdall, Jellyfin, LibreSpeed, NGINX, NGINX Proxy Manager, Resilio Sync, Syncthing, Deluge, OpenCloud, IT Tools, Caddy, AdGuard Home, Crafty Controller, and Easy WireGuard.
- Optional add-ons for Docker, Copyparty, MiniDLNA, DNSMASQ, WireGuard, DuckDNS, Tailscale, SMART Tools, and TuneD.
- Copyparty service controls, user/folder management, optional thumbnails/SFTP, managed update and rollback.
- DNSMASQ local DNS records and wildcards, optional DHCP, lease display, and static-IP safety checks.
- Guided WireGuard pairing and optional site-to-site routes.
- Tailscale connection and subnet-routing management.
- DuckDNS timer-based dynamic DNS updates.
- Configurable dark/light theme, accent colour, and interface font size.
- ServerDeck application icon and unified visual design language.
- Optional TOTP MFA with recovery codes and root CLI recovery.

### Changed

- Consolidated ServerDeck styling into one internally served stylesheet shared by login and the main application.
- Standardised buttons, cards, inputs, tabs, chips, typography, status colours, spacing, radii, and accent handling across the application.
- Terminal controls moved into a full-width toolbar card, with the terminal and command-history panel aligned beneath it.
- Terminal sessions now start automatically when the Terminal page is opened.
- DNS and DHCP are presented together under the DNSMASQ add-on while preserving established on-disk paths for upgrade compatibility.
- Wi-Fi discovery now handles NetworkManager-managed and kernel-visible unmanaged wireless interfaces more safely.
- Docker networking provides guided MACVLAN/IPVLAN selection, including IPVLAN guidance for Wi-Fi hosts.

### Fixed

- Wi-Fi scan failures now surface actionable backend errors instead of generic or silent failures.
- NetworkManager `unmanaged` Wi-Fi devices can be scanned without blindly taking ownership; explicit connection workflows handle management safely.
- OpenCloud Quick App first-run storage/bootstrap handling.
- Terminal connection/reconnection and input handling issues found during RC testing.
- Numerous layout, card alignment, navigation, terminal, Docker, CopyParty, and consistency issues found during release-candidate testing.

### Removed

- Unused legacy helpers and stale internal compatibility aliases that were no longer referenced by the v2 application.
- Obsolete OpenCloud storage-permission repair code superseded by the current wrapper/bootstrap flow.
- Duplicate embedded style definitions replaced by the central ServerDeck stylesheet.

### Compatibility

- Requires Python 3.11+.
- Intended for Debian, Ubuntu, and Raspberry Pi OS with systemd.
- Internal `/etc/servermanager`, `/var/lib/servermanager`, `/opt/servermanager`, and `servermanager.service` names are retained so existing ServerDeck installations upgrade in place.

