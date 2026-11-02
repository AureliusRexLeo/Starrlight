# Software
Last updated: 2026-10-30

## Leviathan (NAS)
- Running: Ubuntu Server, Samba file shares (SSH with key-based login).
- Planned: Nextcloud (next step), Jellyfin later. Unraid/ZFS considered for the permanent build. CasaOS as an experiment.

## Sovereign (lab server)
- Planned: Proxmox VE, GNS3/EVE-NG, OPNsense firewall VM, Homarr dashboard, the *arr stack (moved from LyonHeart).

## LyonHeart
- Ready to start: *arr stack (including Bazarr, Seerr, Homarr) and Jellyfin server in Docker. Library on Leviathan's share, downloads on the 2 TB HDD behind a VPN kill switch, Jellyfin transcoding on Intel QuickSync. Configs move to Sovereign later.
- Local AI: ACE-Step 1.5; other local models kept private.

## Warspite
- Planned: Linux with Kodi/mpv for direct play. Jellyfin client only.

## Hoshiryū
- Bazzite (bazzite-deck), EmuDeck.

## Vanguard
- Store-bought NVR on vendor firmware.

## Argus
- TBD.

## Network and privacy (full stack of services that may lead to vunerablities not listed)
- OPNsense firewall (VM on Sovereign).
- Proton VPN for downloads and privacy.

## Planned for testing and learning
- Remote access: Tailscale, Headscale, WireGuard, or Cloudflare Tunnel. Nothing gets outside access until the firewall is up.
- Split DNS for local service names, after the firewall.
Full apps and services list in progress.
