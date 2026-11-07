# Learning Log

## Starting with the NAS, the first toe dip into server territory

**System:** ASUS M5A97 R2.0 + FX-6300  
**Device name:** leviathan  
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
- Made some deliberate trade-offs on this temporary box.
- Server name set.

**Result:** Ubuntu Server installed and reachable at the console. Login works.

### End of day
Stopped for the evening. Machine was left powered down. 
**Plan for next session:** Bring up network, identify drives, and begin accessing old data.

---

## 2026-10-23 — Network Bring-up & Drive Access

### Problem: Interface up but no IPv4 address
`ip a` showed the network interface linked but only a link-local IPv6 address. No `inet` line.

**Troubleshooting:**
- Confirmed `dhclient` was not available (Ubuntu Server uses Netplan).
- Inspected the installer's Netplan config.
- Found the file only matched the MAC and set the interface name — `dhcp4` was missing.

**Fix:**
Added `dhcp4: true` under the network interface, then:
```bash
sudo netplan apply
```

**Result:** Interface received ip address. Network is working.

### Step: Updated the system and installed Samba + NTFS support
**Why:** Fresh Ubuntu Server install needs current packages. Samba is required so other devices (and the iPhone Files app) can reach the drives. `ntfs-3g` ensures the old Windows HDDs can be mounted cleanly.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ntfs-3g samba
```

**Result:** System updated. Samba and NTFS tools installed.

Samba was not yet fully configured (shares and password) — deferred until drives identified and mounted.
### Step: Identified drives
```bash
lsblk
```
[layout redacted]

**Result:** Confirmed expected layout (OS SSD + two data HDDs).

### Problem: NTFS drives refused clean mount
Mount attempts reported unclean filesystem (Windows hibernation / Fast Startup). Linux fell back to read-only.

**Troubleshooting / Fix:**
```bash
sudo ntfsfix /dev/***
sudo umount /mnt/***          # previous mount was still active
sudo mount /dev/*** /mnt/***
```
Same process used for the second drive as needed.  
Note: the command is `umount` (no “n”).

**Result:** Both data drives mounted and browsable under `/mnt/***` and `/mnt/***`.

### Note on remote access
Remote access setup deferred. Samba not yet configured.

### End of day

Shut down cleanly:
```bash
sudo shutdown -h now
```

Mounts are **not** persistent. They must be remounted after reboot.  
OS, Netplan config, and drive contents are intact.

**Plan for next session(s):**
- Remount the two HDDs.
- Finalize remote access setup.
- Configure Samba for easy access from other devices / iPhone Files app.
- Begin organizing files off the old drives.
- Decide timing for Nextcloud (fresh install preferred over migration for this temporary box).

---

## 2026-10-23 follow-up: Why the NAS was built in stages
- **What happened:** I installed Ubuntu Server without a working network connection, so SSH wasn't set up during the install. Networking got fixed the next day (the missing DHCP setting in Netplan).
- **SSH:** I held off on remote login until the network was sorted. When I did set it up, I went straight to key-based login instead of passwords.
- **Lesson:** Building in stages is fine as long as each stage ends in a safe state. Deferring remote access was safer than opening it up just to get it working.

---

## 2026-10-24 - Samba shares and remote access testing
- **Goal:** Get the old data drives reachable over the network and check remote access.
- **Samba:** Set up file shares for both data drives. Tested from my phone's Files app and from my desktop. Shares first showed up empty because the drives weren't mounted; mounting them fixed it. Mounts still weren't persistent across reboots.
- **Remote access:** Checked the SSH config and found a drop-in file overriding the defaults. SSH was socket-activated, so the service shows as inactive until a connection comes in, which is normal. Held off on finishing remote login setup.
- **Result:** Network file access worked. I was able to organize files on the server straight from my desktop.
- **Lesson:** An empty share usually means the drive underneath isn't mounted, not a Samba problem. Check the effective SSH config (`sshd -T`), not just the main config file, since drop-ins can override it.
- **Next:** Make mounts persistent, finish remote access setup.

---

## 2026-10-27: Hoshiryū GPU swap and Bazzite rebase
*Retrospective entry written November 7, 2026.*

### Change and reasoning
I replaced the temporary GTX 1080 with a PowerColor Hellhound RX 6650 XT. The GTX 1080 made the initial build possible, but the legacy NVIDIA setup kept this console-style PC on Bazzite’s desktop experience rather than the gaming-mode setup she wanted.

With the AMD card installed, I rebased to the `bazzite-deck` image rather than reinstalling the operating system. The swap and rebase completed without troubleshooting. 

### Learning
A GPU change can affect the operating-system image and available desktop/gaming experience, not just graphics performance. Rebasing provided a path to the intended setup without starting over.

### Planned follow-up: fan-curve tuning
I plan to test custom GPU fan curves to improve cooling and, where temperature limits performance, sustain higher clocks. I will compare GPU and hotspot temperatures, sustained clocks, and noise under comparable loads before and after changes.


Fan-curve tuning is planned, not completed.

---
## 2026-10-30: Lyonheart's first blue screen (LiveKernelEvent 144)
- **What happened:** Lyonheart (Windows) restarted on its own while idle, with only background apps running. It was the first crash this PC had ever had.
- **Investigation:** Opened Reliability Monitor (`perfmon /rel`). It showed a "Windows was not properly shut down" entry, a hardware error logged as LiveKernelEvent 144 (normally a USB fault), a failed Windows/Microsoft Store update, and a failed Xbox Service install at the same moment as the crash.
- **Diagnosis:** Researched it and found that a failed update or service install like this can trigger the 144 USB code even when the USB hardware is fine. Looked into the failed update and traced it to the Xbox app and Gaming Services.
- **Fix:** Uninstalled the Xbox app because I don't need it right now. I could have repaired the update instead, but I chose to remove the app. Reinstall it if I want Game Pass later.
- **Lesson:** Check Reliability Monitor before blaming hardware. Line up the timestamps: an update or install failing at the same moment usually explains the crash better than the stop code alone.
- **Lesson 2:** Write down what the hardware was doing right before the crash. Mine was idle, which pointed away from heavy-load causes like a GPU under stress. An idle crash still leaves the usual suspects open (an aging PSU, a driver, unstable RAM at XMP/EXPO speed), so ruling things in and out by load state narrows the search.

---

## Nov 2 - Security review before going public
- **What happened:** Before making this repo public, I had an AI agent review it for sensitive information I'd accidentally exposed in my own write-ups.
- **What it found:** My early logs included details that map out my setup: system hostnames, network interface names, config file paths, mount points, drive layouts, and notes on which security features I'd skipped or deferred. None of it was a password, but together it gives an attacker a head start.
- **What I did:** Reworded those entries to describe what I did without the exact details, and planned a history rewrite so the old versions don't stay in past commits.
- **Lesson:** Log the problem, the fix, and the lesson, not the exact config. Write every entry as if it's already public. Deferred security steps especially shouldn't be announced.
- **Takeaway:** AI is useful as a second set of eyes. I still make the calls and the changes myself, but the audit caught things I'd stopped noticing. My rule going forward: keep things local where I can and don't hand real configs to AI, but for something that's going public anyway, a review is far less risky than not knowing what I exposed.

---

## 2026-11-07 — Checking an older iPad before dashboard reuse
*Retrospective on the November 2 security review.*

**Plan:** Reuse an older iPad as a Homarr dashboard client.

**What I learned:** The security review flagged CVE-2026-86950, a CoreGraphics vulnerability involving malicious files. Apple lists fixes in releases this older hardware cannot install, with no listed fix for its legacy branches. That is different from saying it cannot receive any security updates.

**Response:** Keep the intended role narrow, without personal accounts or general browsing, and isolate it from trusted devices. Dashboard configuration is still planned, not completed.

**Wider lesson:** This reminded me that smart devices should be migrated to, or kept on, the guest network with appropriate separation from trusted PCs. It is a design requirement, not a statement that the migration is complete.

**Takeaway:** Check support and vulnerability coverage before repurposing old hardware. A lightweight dashboard role does not remove an operating-system vulnerability; isolation reduces exposure rather than repairing it.

Source: [Apple security advisory](https://support.apple.com/en-us/149225).

---

-END LOG-

