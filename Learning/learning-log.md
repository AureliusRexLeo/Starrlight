# Learning Log
What I've learned building Starrlight, grouped by area. Newest entries go at the bottom of each section.

---

## Personal PCs

### 2026-08 — LyonHeart: Running ACE-Step 1.5 locally with an AMD GPU
*Exact install date not recorded; written up from my notes on 2026-11-08.*

**System:** LyonHeart (Intel i5-13500, AMD RX 9060 XT 16 GB, 16 GB DDR5, Windows)

**Goal:** Run the ACE-Step 1.5 music-generation model locally on my own AMD GPU instead of a cloud service.

**Context:** This was my first real time using PowerShell. I wrote down everything I ran as I went, in case I broke something and had to undo it.

#### What made it harder than a normal install
- **AMD instead of NVIDIA.** Most local AI tools assume NVIDIA's CUDA. On AMD I had to use ROCm, AMD's GPU compute platform, through AMD's Windows builds of PyTorch.
- **The Python version mattered.** The ROCm packages only worked with Python 3.12, so I set up a separate virtual environment just for this.
- **Windows path limits.** Python warned about file paths longer than 260 characters. I turned on long-path support from an administrator PowerShell window.
- **PowerShell isn't Command Prompt.** Script activation was blocked until I allowed scripts for that session only. Multi-line commands failed until I learned PowerShell continues lines with a backtick, not `^`.

#### Troubleshooting I actually ran into
- **"python was not found":** I used the `py -3.12` launcher and opened a fresh terminal after installing Python.
- **Startup crash (`cannot import name 'group' from 'torch.distributed'`):** The ROCm Windows build doesn't include all of PyTorch's distributed features, and one dependency expected them. I edited that dependency's import so it falls back safely when they're missing. That's a local fix, so updating or reinstalling the package will undo it.
- **"ffmpeg executable not found":** I installed FFmpeg and restarted the terminal so it showed up on my PATH.
- **UI glitches (an option disappearing, maxed-out waveforms with no audio):** Refreshing the page or restarting the server cleared them.
- **Slow generation and CPU-offloading messages:** Normal with 16 GB of VRAM. The first generation is always the slowest.
- **Warnings I learned to ignore:** a missing optional fine-tuning library, attention fallback messages on ROCm, and pip PATH warnings inside the virtual environment.

#### What I figured out once it was running
- The 2B Turbo model was the most stable starting point on 16 GB of VRAM.
- Instrumentals and short clips (30–60 seconds) generated faster and more reliably.
- Negative prompts helped keep out unwanted styles.
- Repaint mode let me fix one section without regenerating the whole track.

#### Takeaways
- Check hardware and version compatibility before following a generic install guide.
- Keep each project in its own virtual environment so its dependencies stay separate from the rest of the system.
- Write down every command when you're learning. It makes problems much easier to trace and undo.
- Read error messages carefully. Most of my fixes came from one specific error line, not from reinstalling everything.

---

### 2026-10-27 — Hoshiryū: GPU swap and Bazzite rebase
*Retrospective entry written November 7, 2026.*

#### Change and reasoning
I replaced the temporary GTX 1080 with a PowerColor Hellhound RX 6650 XT. The GTX 1080 made the initial build possible, but the legacy NVIDIA setup kept this console-style PC on Bazzite’s desktop experience rather than the gaming-mode setup she wanted.

With the AMD card installed, I rebased to the `bazzite-deck` image rather than reinstalling the operating system. The swap and rebase completed without troubleshooting.

#### Learning
A GPU change can affect the operating-system image and available desktop/gaming experience, not just graphics performance. Rebasing provided a path to the intended setup without starting over.

#### Planned follow-up: fan-curve tuning
I plan to test custom GPU fan curves to improve cooling and, where temperature limits performance, sustain higher clocks. I will compare GPU and hotspot temperatures, sustained clocks, and noise under comparable loads before and after changes.

Fan-curve tuning is planned, not completed.

---

### 2026-10-30 — LyonHeart: First blue screen (LiveKernelEvent 144)
- **What happened:** LyonHeart (Windows) restarted on its own while idle, with only background apps running. It was the first crash this PC had ever had.
- **Investigation:** Opened Reliability Monitor (`perfmon /rel`). It showed a "Windows was not properly shut down" entry, a hardware error logged as LiveKernelEvent 144 (normally a USB fault), a failed Windows/Microsoft Store update, and a failed Xbox Service install at the same moment as the crash.
- **Diagnosis:** Researched it and found that a failed update or service install like this can trigger the 144 USB code even when the USB hardware is fine. Looked into the failed update and traced it to the Xbox app and Gaming Services.
- **Fix:** Uninstalled the Xbox app because I don't need it right now. I could have repaired the update instead, but I chose to remove the app. Reinstall it if I want Game Pass later.
- **Lesson:** Check Reliability Monitor before blaming hardware. Line up the timestamps: an update or install failing at the same moment usually explains the crash better than the stop code alone.
- **Lesson 2:** Write down what the hardware was doing right before the crash. Mine was idle, which pointed away from heavy-load causes like a GPU under stress. An idle crash still leaves the usual suspects open (an aging PSU, a driver, unstable RAM at XMP/EXPO speed), so ruling things in and out by load state narrows the search.

---

## Leviathan (NAS): the first toe dip into server territory

**System:** ASUS M5A97 R2.0 + FX-6300  
**Device name:** leviathan  
**Purpose:** Short-term disposable file dump / NAS while permanent i3 hardware is acquired.  
**Note:** Hardware is old and may fail; data is being organized off older drives.

---

### 2026-10-22 — BIOS Prep & Ubuntu Server Install

#### Step: Entered BIOS and set fan controls
**Why:** The FX-6300 line runs hot. This box will eventually stay on more often for file access, so higher minimum fan duty reduces thermal risk on old hardware.

**Settings applied:**
- BIOS temperature scale confirmed as Celsius.
- Chassis and CPU max duty cycle set to 100%.
- Chassis upper temperature threshold lowered to 60–65 °C so fans ramp earlier.
- CPU minimum duty raised (target 70–80%+) so the cooler stays more aggressive at idle.
- Noted that this board’s Manual mode uses duty cycles rather than a simple DC/PWM toggle.

**Result:** Fan profile adjusted for a hot, aging CPU before first power-on with the new OS.

#### Step: Set boot order
- USB first for installer.
- SSD second.
- HDDs lower or disabled so Windows drives would not interfere with install.

**Result:** Ready to boot Ubuntu Server installer cleanly.

#### Step: Installed Ubuntu Server on the 120 GB SSD
**Decisions:**
- Made some deliberate trade-offs on this temporary box.
- Server name set.

**Result:** Ubuntu Server installed and reachable at the console. Login works.

#### End of day
Stopped for the evening. Machine was left powered down.  
**Plan for next session:** Bring up network, identify drives, and begin accessing old data.

---

### 2026-10-23 — Network Bring-up & Drive Access

#### Problem: Interface up but no IPv4 address
`ip a` showed the network interface linked but only a link-local IPv6 address. No `inet` line.

**Troubleshooting:**
- Confirmed `dhclient` was not available (Ubuntu Server uses Netplan).
- Inspected the installer's Netplan config.
- Found the file only matched the MAC and set the interface name — `dhcp4` was missing.

**Fix:**
Added `dhcp4: true` under the network interface, then:
~~~bash
sudo netplan apply
~~~

**Result:** Interface received ip address. Network is working.

#### Step: Updated the system and installed Samba + NTFS support
**Why:** Fresh Ubuntu Server install needs current packages. Samba is required so other devices (and the iPhone Files app) can reach the drives. `ntfs-3g` ensures the old Windows HDDs can be mounted cleanly.

~~~bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ntfs-3g samba
~~~

**Result:** System updated. Samba and NTFS tools installed.

Samba was not yet fully configured (shares and password) — deferred until drives identified and mounted.

#### Step: Identified drives
~~~bash
lsblk
~~~
[layout redacted]

**Result:** Confirmed expected layout (OS SSD + two data HDDs).

#### Problem: NTFS drives refused clean mount
Mount attempts reported unclean filesystem (Windows hibernation / Fast Startup). Linux fell back to read-only.

**Troubleshooting / Fix:**
~~~bash
sudo ntfsfix /dev/***
sudo umount /mnt/***          # previous mount was still active
sudo mount /dev/*** /mnt/***
~~~
Same process used for the second drive as needed.  
Note: the command is `umount` (no “n”).

**Result:** Both data drives mounted and browsable under `/mnt/***` and `/mnt/***`.

#### Note on remote access
Remote access setup deferred. Samba not yet configured.

#### End of day

Shut down cleanly:
~~~bash
sudo shutdown -h now
~~~

Mounts are **not** persistent. They must be remounted after reboot.  
OS, Netplan config, and drive contents are intact.

**Plan for next session(s):**
- Remount the two HDDs.
- Finalize remote access setup.
- Configure Samba for easy access from other devices / iPhone Files app.
- Begin organizing files off the old drives.
- Decide timing for Nextcloud (fresh install preferred over migration for this temporary box).

---

### 2026-10-23 follow-up — Why the NAS was built in stages
- **What happened:** I installed Ubuntu Server without a working network connection, so SSH wasn't set up during the install. Networking got fixed the next day (the missing DHCP setting in Netplan).
- **SSH:** I held off on remote login until the network was sorted. When I did set it up, I went straight to key-based login instead of passwords.
- **Lesson:** Building in stages is fine as long as each stage ends in a safe state. Deferring remote access was safer than opening it up just to get it working.

---

### 2026-10-24 — Samba shares and remote access testing
- **Goal:** Get the old data drives reachable over the network and check remote access.
- **Samba:** Set up file shares for both data drives. Tested from my phone's Files app and from my desktop. Shares first showed up empty because the drives weren't mounted; mounting them fixed it. Mounts still weren't persistent across reboots.
- **Remote access:** Checked the SSH config and found a drop-in file overriding the defaults. SSH was socket-activated, so the service shows as inactive until a connection comes in, which is normal. Held off on finishing remote login setup.
- **Result:** Network file access worked. I was able to organize files on the server straight from my desktop.
- **Lesson:** An empty share usually means the drive underneath isn't mounted, not a Samba problem. Check the effective SSH config (`sshd -T`), not just the main config file, since drop-ins can override it.
- **Next:** Make mounts persistent, finish remote access setup.

---

### 2026-11-11 — Leviathan first update, drive triage, key-only SSH, Samba rebuild

#### Summary
Leviathan, the Starrlight NAS (Ubuntu 26.04 for now; Unraid long-term), got its first full update since setup. Along the way, one data drive turned out to be dead and was retired, the remaining data drives were set to mount automatically at boot, SSH was locked down to key-only logins, and the Samba shares were rebuilt around a non-admin share account.

#### 1. Patching
- Ran a full upgrade: 47 packages, then a reboot.
- Kernel is now `7.0.0-43-generic`, which fixes **CVE-2026-98432** (IPv4 TCP use-after-free, remote root).
- APT is now `3.2.1ubuntu0.1`, which fixes **CVE-2026-109518**. (`apt --version` hides the package suffix; `dpkg -s apt | grep Version` shows it.)
- Automatic security updates (unattended-upgrades) confirmed on.
- The security gate on the kernel CVE is cleared for Leviathan. It stays LAN-only with no port forwards; other Linux boxes still need their own check.

#### 2. Drive Health
SMART checks don't need a drive to be mounted, so every drive was checked straight from the device list.

| Drive | Role | Result |
|---|---|---|
| Boot SSD | OS | About 30% life left, 0 reallocated sectors. Worn but healthy. |
| 1 TB Seagate 3.5" | Media | Good. |
| 500 GB WD 2.5" | Files | Good. |
| 500 GB Seagate 3.5" | — | **Failed. Retired.** |

The failed Seagate spun up but never identified itself to the system. The kernel log showed the link coming up but an unknown device class and failed resets. The fault followed the drive across a new data cable, a different SATA port, and different power, which points to the drive's own controller. It was removed from the build.

#### 3. Unexpected Lockup
After the side panel went back on, the keyboard stopped responding and the power button wouldn't shut the system down, so it was powered off at the PSU switch. On the next boots the keyboard was dead at the BIOS screen. With the case opened, cables reseated, and the failed drive removed, the board showed the AMI "press F1 to recover" prompt and booted normally from the SSD via the boot menu. Likely cause: a loose connection. It hasn't repeated, so it's being watched rather than fixed.

#### 4. Formatting and Automount
- The WD drive still carried an old NTFS signature. It was wiped and formatted ext4 with the label `files`.
- Both data drives are in `/etc/fstab` with the `nofail` option, so a dead or missing drive won't stop Leviathan from booting.
- The WD is mounted by its label instead of its UUID, which is easier to read and type.

#### 5. Key-Only SSH
- A check with `sudo sshd -T | grep -i passwordauthentication` showed password logins were still allowed.
- An ed25519 key was generated on LyonHeart (the main PC), copied to Leviathan, and tested before anything was turned off.
- Password and keyboard-interactive logins were then disabled in a drop-in file under `/etc/ssh/sshd_config.d/`. The `00-` prefix makes it load first, so it overrides any later file that turns passwords back on.
- Config checked with `sshd -t`, SSH reloaded, a second login tested while the first session stayed open, and `sshd -T` now reports `passwordauthentication no`.
- New devices get access by having their public key added from a device that already works. The local keyboard and monitor remain the fallback.

#### 6. Samba Rebuild
- The old config shared a drive that no longer existed and didn't share the WD at all. It also forced every file to be owned by the admin account.
- New setup:
  - A `nas` group and a dedicated share-only account with no shell login and no home folder.
  - The admin account is in the group but isn't used for shares, in line with the plan to not use the admin account day to day.
  - Two shares, `media` (1 TB) and `files` (WD), limited to the `nas` group, no guest access.
  - Folders use setgid permissions (`2775`) with `force group = nas`, so new files stay group-owned. A future service account for the media stack only needs to join the `nas` group.
- LyonHeart connects to both shares.

#### 7. Lessons Learned
- Check every character in a config file, especially the file name. A file that doesn't end in exactly `.conf` gets silently ignored.
- A terminal that seems frozen is usually waiting at a prompt.
- Test key-based login *before* turning off password logins.
- `nofail` in fstab keeps one bad drive from taking the whole box down at boot.
- A fault that follows a drive across cables, ports, and power is the drive, not the system.

#### 8. Still Open
- Back up `/etc` (system config) to the 1 TB drive.
- Router DHCP reservation so Leviathan's address doesn't change.
- Replacement boot SSD on the parts list, plus a weekly check on SSD wear.
- Service account for the media stack, when the stack goes up.
- Optional: test the failed Seagate in another PC to confirm.

---

### 2026-11-13 — Leviathan: config backup, mount fix, and clock fix

#### Summary
A short maintenance session on Leviathan, done from LyonHeart (the main PC) over SSH. Security updates were confirmed current, the system config was backed up before anything changed, an old mount entry was fixed, and the server clock was set to local time.

#### 1. Security Updates
- Confirmed the latest security updates from recent advisories were already installed. Nothing new to apply.

#### 2. Back Up First
- Before touching any config, I backed up the whole system configuration folder (`/etc`) into a compressed archive.
- **Where it went, and why:** the archive went to the Linux-formatted (ext4) data drive, not the NTFS media drive. The config holds sensitive system files, and ext4 enforces Linux file permissions, so the archive can be locked to root-only. An NTFS drive mounted for easy sharing doesn't give that protection.
- Locked it down with `chmod 600` and confirmed the permissions.
- `tar` printed "Removing leading `/` from member names." That's normal: it stores paths without the leading slash, so restoring the archive later can't overwrite the live system by accident.

#### 3. The Mount Fix
- Reviewing `/etc/fstab` turned up one data drive still listed by its device name (`/dev/sdX`) instead of a permanent ID, plus a typo in its options and a duplicated option.
- **Why it matters:** Linux assigns device names in the order it finds drives at boot. Add or remove a drive and the names can shuffle, so the wrong drive (or no drive) gets mounted.
- **Fix:** replaced the line with one that uses the drive's UUID, a permanent ID that never changes, and corrected the options.
- **Tested without rebooting:** `sudo systemctl daemon-reload && sudo mount -a`, then confirmed the files were there.
- **Backed up again** after the fix, then confirmed the new backup so it includes the corrected config.

#### 4. Clock Fix
- File timestamps showed the server was running on UTC instead of local time. Set it to local time with `timedatectl` so logs and file times line up with when things actually happened.

#### Lessons Learned
- **Check what's there before changing it, and back up first.** Reading the existing config is how the typo turned up at all.
- **Double- and triple-check every typed line.** The typo reinforces my rule. It worked anyway, for now, but "works for now" isn't the standard. Going line by line has to become a habit.
- **Windows habits don't carry over.** Working from the main PC, right-click paste into the terminal is a big time saver, but it also tempts Windows reflexes. In `nano`, I highlighted a line and hit Delete, and then reflexively hit Ctrl+Z, which *suspends* nano instead of undoing. The fixes: `fg` brings nano back, Alt+U is undo, Ctrl+K cuts a whole line, and Ctrl+X then N exits without saving anything. Nothing was harmed because nano doesn't save until you tell it to, but I'm studying up on nano's commands.

---

## Security and devices

### 2026-11-02 — Security review before going public
- **What happened:** Before making this repo public, I had an AI agent review it for sensitive information I'd accidentally exposed in my own write-ups.
- **What it found:** My early logs included details that map out my setup: system hostnames, network interface names, config file paths, mount points, drive layouts, and notes on which security features I'd skipped or deferred. None of it was a password, but together it gives an attacker a head start.
- **What I did:** Reworded those entries to describe what I did without the exact details, and planned a history rewrite so the old versions don't stay in past commits.
- **Lesson:** Log the problem, the fix, and the lesson, not the exact config. Write every entry as if it's already public. Deferred security steps especially shouldn't be announced.
- **Takeaway:** AI is useful as a second set of eyes. I still make the calls and the changes myself, but the audit caught things I'd stopped noticing. My rule going forward: keep things local where I can and don't hand real configs to AI, but for something that's going public anyway, a review is far less risky than not knowing what I exposed.

---

### 2026-11-07 — Checking an older iPad before dashboard reuse
*Retrospective on the November 2 security review.*

**Plan:** Reuse an older iPad as a Homarr dashboard client.

**What I learned:** The security review flagged CVE-2026-86950, a CoreGraphics vulnerability involving malicious files. Apple lists fixes in releases this older hardware cannot install, with no listed fix for its legacy branches. That is different from saying it cannot receive any security updates.

**Response:** Keep the intended role narrow, without personal accounts or general browsing, and isolate it from trusted devices. Dashboard configuration is still planned, not completed.

**Wider lesson:** This reminded me that smart devices should be migrated to, or kept on, the guest network with appropriate separation from trusted PCs. It is a design requirement, not a statement that the migration is complete.

**Takeaway:** Check support and vulnerability coverage before repurposing old hardware. A lightweight dashboard role does not remove an operating-system vulnerability; isolation reduces exposure rather than repairing it.

Source: [Apple security advisory](https://support.apple.com/en-us/149225).

---

## Network

### 2026-11-12 — ISP gateway: factory reset and rebuild

#### Summary
The ISP-provided gateway stopped accepting my admin login, even though the network itself (including the guest network) kept working. Instead of fighting it, I factory-reset the gateway and rebuilt it from my own notes. No call to ISP support was needed.

#### 1. The Problem
- The admin page rejected the login that had been working.
- Everything else was fine: devices stayed online, and the guest network was still up.
- With no way into the settings, there was no way to confirm the security settings were still in place.

#### 2. The Decision
A factory reset wipes everything back to defaults, which sounds worse than it is when the setup is documented. My notes already listed every setting I'd changed and why, so a reset was the fastest path back to a known-good, verified configuration.

#### 3. The Rebuild
- **Admin access:** set a new, strong admin login right away, before anything else.
- **Guest network:** brought it back for untrusted devices, with the same name and password so those devices reconnected without being touched.
- **Security settings:** reapplied them and confirmed each one saved, including remote management staying off.
- **Fixed addresses:** gave the main PC and the NAS DHCP reservations, so they always get the same address on the network. 

#### 4. Verification
- The file shares between the main PC and the NAS came back on their own, because the NAS kept the same address.
- The gateway now shows the NAS with a fixed address.

#### 5. Why This Matters for the Lab
Fixed addresses are the groundwork for what comes next: the *arr stack, Nextcloud, and later the firewall and VPN. Nothing is exposed to the internet: no open ports, and remote access will only come through a VPN once the firewall is in place.

#### Lesson Learned
My organized documentation and checklists turned what could have been a daunting reset into a 10-minute redo. It reinforces documenting every setting you change, and why, while you're changing it.

---

-END LOG-