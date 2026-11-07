# Software
Last updated: 2026-11-07

High-level map of what runs where and what is planned. Planned services are not installed just because they appear here. Configurations, network details, and deployment instructions stay private.

## Leviathan (NAS)
- Running: Ubuntu Server and Samba file shares.
- Planned next: Nextcloud for files and sharing.
- Long-term OS: Unraid. CasaOS remains a small experiment, not the main management platform.
- After the i3 upgrade, Leviathan may temporarily host the needed service stack if Sovereign is not ready. Once Sovereign takes over that stack, Leviathan returns to its NAS role.

## Sovereign (lab server)
- Not built. Planned OS: Proxmox VE.
- Planned roles: OPNsense firewall VM, GNS3/EVE-NG networking labs, and hosting the main service stack in suitable VMs or containers.
- Eventual home for the media automation services, Homarr, and supporting monitoring and management tools.

## LyonHeart (daily PC — temporary stack host)
- Running: Windows, with local AI experiments including ACE-Step 1.5.
- Planned temporary setup: Docker Desktop for the needed media services and Homarr; Jellyfin runs natively on Windows using Intel QuickSync.
- Media libraries stay on the NAS; downloads use a dedicated disk and a VPN-protected download client.
- The stack migrates to Sovereign later, with i3-upgraded Leviathan as an interim host if needed.
- Portainer is not required for this temporary Windows setup. Linux containers can use QuickSync on compatible Intel hardware with GPU access configured.

## Warspite
- Not built; gifted CPU and motherboard received.
- Planned: Kubuntu with Kodi/mpv for direct play, disc ripping, and light emulation.
- Jellyfin client only; server-side transcoding happens elsewhere.

## Hoshiryū
- Running: Bazzite (bazzite-deck) and EmuDeck.

## Vanguard
- Planned: store-bought NVR running vendor firmware.

## Argus
- Planned: headless Ubuntu LTS for local models and agents.

## Network and privacy (planned)
- Firewall first: outside access waits until the dedicated firewall and intended network separation are in place.
- Headscale provides self-hosted coordination for mesh remote access using compatible Tailscale client apps, rather than exposing individual application ports.
- Proton VPN provides download privacy; Gluetun provides the container VPN connection and kill-switch boundary for the download client. This is separate from Headscale's remote-access role.
- AdGuard Home provides network-level DNS filtering on existing hardware.
- Caddy provides the later reverse proxy and HTTPS path for services.
- restic provides encrypted backups, paired with restore testing.

## Apps and services (planned)

### Core and media
- Nextcloud — files and sharing; preferred over Syncthing for on-demand access and sync.
- Homarr — the only dashboard, for the lab and a later family-board view.
- Jellyfin — media server and playback library.
- Sonarr, Radarr, and Prowlarr — media library automation and indexer management.
- Bazarr — subtitle management; Seerr — media requests.
- qBittorrent — download client behind the VPN boundary.
- FlareSolverr — only if a selected service actually requires it.
- Navidrome — music streaming; Lidarr joins it for music-library organization.

### Management, monitoring, and utilities
- Portainer — Docker management on the permanent Linux Docker host, not the temporary Windows host.
- Uptime Kuma — service availability checks.
- Beszel — host resource monitoring.
- Stirling PDF — PDF utilities, added when useful.

### Documentation and identity
- BookStack — private runbooks, SOPs, and structured documentation.
- Authentik — planned identity provider for shared sign-in to Nextcloud and BookStack. Other applications can join where their authentication support allows; native SSO is not assumed for every app.
- Nextcloud holds files; BookStack holds structured documentation. Both are clients of Authentik, rather than Nextcloud acting as the identity provider.
- Start with links between files and documentation. Displaying BookStack inside Nextcloud is optional later work, not a day-one requirement.
- Vaultwarden — a later password-manager deployment after the core hosting and access arrangements are ready. It does not depend on completing SSO.

### Later / staged
- Immich — photo-library migration away from iCloud.
- Home Assistant — home automation.
- Kiwix — offline reference material.
- Add-ons are staged as capacity and need allow, rather than deployed all at once.

## Portfolio vs private documentation
- This GitHub repo is the scrubbed public lab story and selected write-ups.
- BookStack is the planned private documentation home. For employment or portfolio use, share a curated export or limited read-only view, not the entire internal wiki.

## Learning resources
Planned isolated-VM exercises, learning resources, and source credits are in [Lab Security](lab-security.md).