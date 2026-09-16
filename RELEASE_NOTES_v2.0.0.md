# ServerDeck 2.0.0 Release Notes

**Release date:** 16 September 2026

ServerDeck 2.0.0 is the first stable release of the ServerDeck v2 line. It packages the functionality validated through the v2 release-candidate cycle into the stable `2.0.0` build without introducing additional feature changes after RC32.

## What ServerDeck 2.0 is for

ServerDeck is a lightweight administration interface for Debian-family home servers and NAS systems. The design goal is to provide the management tasks normally needed on a headless server without requiring a heavyweight control panel or continuously running metrics stack.

The application remains a single Python file. The installer copies it into the established ServerDeck installation path, creates/updates the systemd service, and generates a self-signed HTTPS certificate if required.

## Major capabilities

### Server administration

- System overview, notifications, reboot-required state, and lightweight live activity.
- Processes and services.
- APT updates and scheduled unattended update jobs.
- Interactive privileged terminal with command history.
- File Browser and permissions/ownership management.
- Power controls and persistent Activity output.

### Storage and NAS

- Physical disk and partition inspection.
- Guided storage preparation and persistent `/etc/fstab` mounting.
- EXT4, Btrfs, and XFS guided storage preparation, with additional filesystem handling elsewhere in disk tools where supported.
- SMART health and temperature when smartmontools is installed.
- Samba shares.
- Linux users and groups.
- rsync backups with dry-run verification and systemd schedules.

### Networking

- NetworkManager, Netplan, and ifupdown awareness.
- DHCP/static IPv4 management with timed rollback protection.
- Wi-Fi network discovery and connection.
- Improved handling of wireless devices that the kernel sees but NetworkManager reports as unmanaged.
- Docker MACVLAN/IPVLAN workflows with guided own-IP networking.

### Applications and optional services

ServerDeck keeps the base installation small and exposes optional functionality through Add-ons:

- Docker
- Copyparty
- MiniDLNA
- DNSMASQ (DNS + DHCP)
- WireGuard
- DuckDNS
- Tailscale
- SMART Tools
- TuneD

The Docker Quick Apps catalogue includes Heimdall, Jellyfin, LibreSpeed, NGINX, NGINX Proxy Manager, Resilio Sync, Syncthing, Deluge, OpenCloud, IT Tools, Caddy, AdGuard Home, Crafty Controller, and Easy WireGuard.

## Final RC-to-stable promotion

The stable release is promoted directly from RC32. The application code differs from RC32 only in release-version metadata:

- `2.0.0-rc32` → `2.0.0`
- Release header updated from RC32 to the stable 2.0.0 label.

There are no intentional functional or CSS changes between RC32 and the stable release.

## Install

```bash
chmod +x serverdeck-v2.0.0.py
sudo ./serverdeck-v2.0.0.py --install
```

The default HTTPS port is **8443**, but the installer allows another port to be selected.

## Upgrade from an RC or earlier v2 build

Install the stable file over the existing installation:

```bash
sudo ./serverdeck-v2.0.0.py --install
```

Existing ServerDeck configuration and state are preserved. The historical internal `servermanager` paths and service name remain intentionally unchanged for compatibility.

## Security and deployment notes

ServerDeck is a privileged administration service. It can format disks, alter network configuration, manage users, execute shell commands, install packages, control systemd services, and perform other host-level operations.

For that reason:

- Use it on a trusted LAN or through a private VPN such as Tailscale or WireGuard.
- Do not expose the ServerDeck management port directly to the public Internet without an independently secured deployment design.
- Keep backups of important data before destructive storage operations.
- Network changes use rollback protection, but administrators should still have local/console access available when changing a remote server's primary interface.
- The default TLS certificate is self-signed and must be explicitly trusted if you want to remove browser certificate warnings.

## Known deployment considerations

- ServerDeck supports Debian-family systems; it is not intended as a generic cross-distribution control panel.
- Some features depend on optional packages and are hidden or gated until those packages are installed.
- Hardware-specific SMART information depends on drive/controller support.
- Docker own-IP networking depends on the host interface and LAN topology; ServerDeck guides MACVLAN/IPVLAN configuration but cannot change upstream switch/router restrictions.
- Wi-Fi behaviour ultimately depends on the distribution's network manager, wireless driver, rfkill state, and firmware.
- DHCP should only be enabled when ServerDeck is intended to be the DHCP server for that network segment; avoid running conflicting DHCP servers.

## Documentation

See [README.md](README.md) for installation, feature, security, add-on, and troubleshooting information. See [CHANGELOG.md](CHANGELOG.md) for the 2.0.0 change summary.

