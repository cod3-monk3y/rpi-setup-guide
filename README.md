# Raspberry Pi — Hardened Headless Setup Guide

Step-by-step headless setup for Raspberry Pi 4/5 running Raspberry Pi OS Trixie (Debian 13, 64-bit Lite).

Covers SD format, cloud-init configuration, SSH hardening, static IP, MAC address spoofing, WireGuard VPN, and system hardening — designed for headless operation with minimal attack surface.

## Contents

- **[rpi-setup-guide.md](rpi-setup-guide.md)** — Full markdown source (read on GitHub)
- **[rpi-setup-guide.pdf](rpi-setup-guide.pdf)** — Rendered PDF (16 pages, printable)

## What's covered

| Phase | Topic |
|-------|-------|
| 1 | Format SD card to FAT32 |
| 2 | Flash Raspberry Pi OS image |
| 3 | Mount partitions |
| 4 | Pre-boot config: `user-data`, `network-config`, `cmdline.txt`, `nmconnection`, rfkill |
| 5 | First boot — find the Pi on the network |
| 6 | Verify static IP assignment |
| 7 | SSH key auth + drop-in hardening (port 2222, key-only) |
| 8 | MAC address spoofing verification |
| 9 | WireGuard VPN server for remote access |
| 10 | Final system hardening |

## Key technical decisions

- **Cloud-init** for full pre-boot configuration (no manual first-boot steps)
- **NTP sync before package install** — Pi has no RTC, prevents sqv signature failures
- **MAC spoofing via systemd-networkd `.link` file** — applies at udev level before NetworkManager
- **SSH drop-in config** at `/etc/ssh/sshd_config.d/99-hardening.conf` — survives package upgrades
- **`ufw` firewall** chosen over `iptables-persistent` (they conflict)
- **`/etc/sysctl.d/99-wireguard.conf`** for IP forwarding — Trixie deprecates `/etc/sysctl.conf`
- **Manual WireGuard** instead of Tailscale — no third-party identity provider, no control plane dependency

## Quick start

Download the PDF directly:

```bash
curl -LO https://github.com/YOUR-USERNAME/YOUR-REPO/raw/main/rpi-setup-guide.pdf
```

Or clone the full repo:

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
```

## Target Audience

- Anyone building a hardened home VPN / remote access endpoint
- Defense / opsec contexts where commercial VPN services aren't acceptable
- Learners who want to understand WireGuard, cloud-init, and Linux hardening at the protocol level

## Requirements

- Raspberry Pi 4 or 5
- MicroSD card (16GB+ recommended)
- Linux or macOS workstation
- Ethernet cable (recommended for first boot) OR WiFi credentials

## License

MIT — use freely, attribution appreciated.

## Notes

This guide reflects working configurations validated on:
- Raspberry Pi OS Trixie (`2026-04-21-raspios-trixie-arm64-lite.img`)
- Raspberry Pi Imager v2.0.6+
- WireGuard 1.0.x

Tested on RPi 5. Should work on RPi 4 without modification.
