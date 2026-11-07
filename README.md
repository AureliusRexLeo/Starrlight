# Starrlight 

Starrlight is my home lab. It's spelled with two r's on purpose: my wife's last name is Starr, and the lab is named for her.

It started because I got tired of paying for subscriptions and watching digital purchases disappear. 
I want my own files, media, and services on hardware I control, with no cloud account required.
An injury gave me a few months to plan it properly, and the lab also allows me to gain experience for a possible career change into IT. 
I've been building PCs for 14 years and tinkering with hardware for 22, and now I'm learning networking from the ground up.

Lab machines are named after naval ships. Personal PCs have their own names.

## Goals
- Learn networking from the configuration up: VLANs, trunking, STP, and console recovery, done by hand on real hardware.
- Pair a physical managed switch (MikroTik CRS326) with virtual topologies (Proxmox plus GNS3/EVE-NG) on a dedicated lab server.
- Earn Network+ as my first certification, alongside hands-on lab work.
- Keep a public record of what I build, what breaks, and how I fix it.
- Locally managed, offline-capable gear. No subscriptions, no cloud-managed equipment.

Every change gets tested in an isolated lab VLAN first and has a rollback path (RouterOS Safe Mode on the switch, VM snapshots on the firewall).


## Devices
| Device | Role | Status | Page |
|-----------|--------------------------------------------------------------------|---------|-----------------------------------------------------|
| Leviathan | NAS: primary copy for family files and media (3-2-1 backups)       | Functional (temporary) | [nas-leviathan.md](nas-leviathan.md) |
| Sovereign | Lab server (Proxmox, GNS3/EVE-NG, OPNsense firewall VM)            | Planned | [server-sovereign.md](server-sovereign.md)          | 
| Warspite  | Living-room media PC: direct-play 4K HDR playback, ripping, burner | Planned | [media-warspite.md](media-warspite.md)              |
| Argus     | Local AI server for models and agents                              | Planned | [ai-argus.md](ai-argus.md)                          | 
| Vanguard  | NVR for cameras and sensors, separate from the NAS                 | Planned | [nvr-vanguard.md](nvr-vanguard.md)                  |
| Kraken    | Locally managed network: MikroTik switch, UPS, own modem and AP    | Planned | [network-kraken.md](network-kraken.md)              |
| LyonHeart | My main PC: gaming, editing, local AI                              | Running | [pc-lyonheart.md](pc-lyonheart.md)                  |
| Hoshiryū  | Wife's PC (console-style)                                          | Running | [pc-hoshiryu.md](pc-hoshiryu.md)                    |
| iPad | Homarr dashboard client | Planned; setup incomplete | [ipad.md](ipad.md) |

## Other pages
| Page | What's in it |
|------------------------------------------------------|------------------------------------------------|
| [budget.md](budget.md)                               | Line-item budgets per device                   |
| [parts.md](parts.md)                                 | Where every part is and where it's going       |
| [software.md](software.md)                           | What runs where, and what's planned            |
| [lab-security.md](lab-security.md)                   | Security approach (kept high-level on purpose) |
| [notes-customization.md](notes-customization.md)     | The longer backstory                           |
| [Learning/learning-log.md](Learning/learning-log.md) | My build and troubleshooting log               |
| [Learning/lessons-log.md](Learning/lessons-log.md)   | Lessons from other's that's changed my lab     |

