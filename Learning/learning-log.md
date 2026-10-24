Learning Log

#starting with the nas, the first toe dip into server territory

**System:** ASUS M5A97 R2.0 + FX-6300  
**Hostname:** leviathan  
**Purpose:** Short-term disposable file dump / NAS while permanent i3 hardware is acquired.  
**Note:** Hardware is old and may fail; data is being organized off older drives.

---

## 2026-10-22 — BIOS Prep & Ubuntu Server Install

### Step: Entered BIOS and set fan controls
**Why:** The FX-6300 line runs hot. This box will eventually stay on more often for file access, so higher minimum fan duty reduces thermal risk on old hardware.

**Settings applied:**
- BIOS temperature scale confirmed as Celsius.
- Chassis and CPU max duty cycle set to 100%.
- Chassis upper temperature threshold lowered to 60–65 °C so fans ramp earlier.
- CPU minimum duty raised (target 70–80%+) so the cooler stays more aggressive at idle.
- Noted that this board’s Manual mode uses duty cycles rather than a simple DC/PWM toggle.

**Result:** Fan profile adjusted for a hot, aging CPU before first power-on with the new OS.

### Step: Set boot order
- USB first for installer.
- SSD second.
- HDDs lower or disabled so Windows drives would not interfere with install.

**Result:** Ready to boot Ubuntu Server installer cleanly.

### Step: Installed Ubuntu Server on the 120 GB SSD
**Decisions:**
- Skipped LVM encryption. Temporary box; passphrase on every boot adds friction with little benefit while the data lives on separate HDDs.
- Server name set.

**Result:** Ubuntu Server installed and reachable at the console. Login works.

### End of day
Stopped for the evening. Machine was left powered down. 
**Plan for next session:** Bring up network, identify drives, and begin accessing old data.

---

## 2026-10-23 — Network Bring-up & Drive Access

### Problem: Interface up but no IPv4 address
`ip a` showed `enp2s0` linked but only a link-local IPv6 address. No `inet` line.

**Troubleshooting:**
- Confirmed `dhclient` was not available (Ubuntu Server uses Netplan).
- Inspected `/etc/netplan/00-installer-config.yaml`.
- Found the file only matched the MAC and set the interface name — `dhcp4` was missing.

**Fix:**
Added `dhcp4: true` under `enp2s0`, then:
```bash
sudo netplan apply
```

**Result:** Interface received ip address. Network is working.

### Step: Updated the system and installed Samba + NTFS support
**Why:** Fresh Ubuntu Server install needs current packages. Samba is required so other devices (and the iPhone Files app) can reach the drives. `ntfs-3g` ensures the old Windows HDDs can be mounted cleanly.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ntfs-3g samba

**Result:** System updated. Samba and NTFS tools installed.

Samba was not yet fully configured (shares and password) — deferred until drives identified and mounted.
### Step: Identified drives
```bash
lsblk
```

| Device | Size     | Role              |
|--------|----------|-------------------|
| sda    | ~120 GB  | SSD (Ubuntu OS)   |
| sdb    | ~932 GB  | 1 TB HDD          |
| sdc    | ~112 GB  | 120 GB HDD        |

**Result:** Confirmed expected layout (OS SSD + two data HDDs).

### Problem: NTFS drives refused clean mount
Mount attempts reported unclean filesystem (Windows hibernation / Fast Startup). Linux fell back to read-only.

**Troubleshooting / Fix:**
```bash
sudo ntfsfix /dev/sdb2
sudo umount /mnt/1tb          # previous mount was still active
sudo mount /dev/sdb2 /mnt/1tb
```
Same process used for the second drive as needed.  
Note: the command is `umount` (no “n”).

**Result:** Both data drives mounted and browsable under `/mnt/1tb` and `/mnt/120gb`.

### Note on remote access
SSH password authentication is disabled by default. Enabling it (`PasswordAuthentication yes` in `sshd_config` + restart) was deferred. Samba not yet configured.

### End of day
Shut down cleanly:
```bash
sudo shutdown -h now
```

Mounts are **not** persistent. They must be remounted after reboot.  
OS, Netplan config, and drive contents are intact.

**Plan for next session:**
- Remount the two HDDs.
- Enable SSH password auth.
- Configure Samba for easy access from other devices / iPhone Files app.
- Begin organizing files off the old drives.
- Decide timing for Nextcloud (fresh install preferred over migration for this temporary box).

---

##

---
-END LOG-

