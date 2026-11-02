# Leviathan (NAS)
Last updated: 2026-10-30

## Why
Leviathan holds the primary copy in my 3-2-1 backup plan: per-person media shares, family files, and later a quarterly optical archive (the burner will probably live on Warspite).

## Target
- Platform: Intel 12th-14th gen i3 (i3-12100 or i3-14100 class), 16 GB DDR4, 500 GB NVMe for OS/cache. All-new budget is in [budget.md](budget.md); a used 12th-14th gen board and CPU is also an option.
- Storage: 1 TB for media, 2x 500 GB for files to start; mixed disks, then used SAS, then NAS-class drives.
- Software: Unraid (temporary) moving to ZFS later is on the table; Ubuntu Server for now. Nextcloud and Jellyfin planned. CasaOS as an experiment.
- Permanent spot with a Cat6 run, running headless 24/7.

## Now (temporary build)
- CPU and board: AMD FX-6300 on ASUS M5A97 R2.0, Zalman case.
- OS: Ubuntu Server on a 120 GB SSD (about 30% life left; to be replaced after the config is backed up).
- Cooler and PSU: replaced and working. Cooler is a no-name aftermarket unit (the old one was a Corsair H60).
- Planned upgrade: a Thermaltake Toughpower GF1 850W (fully modular, lots of SATA), case has no room for unused cables.
- Data drives:
  - 1 TB HDD: installed, media.
  - 500 GB HDD #1: installed. Has old data; reviewing it, saving family pics, then wiping and health-testing it.
  - 500 GB 2.5" HDD #2: in the case, unplugged on purpose until I need it.
- Optical: ASUS DVD-RW. GPU: R7 260X spare, not installed.
- Still to buy: 3-pin fan Y-splitter, SATA power extension (or Molex-to-SATA).
- Services: Samba file shares working on the local network.
- The data on it right now is a copy; originals are on my external backup drive and elsewhere.

## Log
- 2026-10-18: Temporary platform documented.
- 2026-10-22: Raised BIOS fan curves for the hot FX-6300, set boot order, installed Ubuntu Server.
- 2026-10-23: Fixed no-IPv4 after install (Netplan config was missing DHCP). Installed Samba and NTFS support. Fixed read-only NTFS drives (Windows Fast Startup) with ntfsfix. Details in the [learning log](Learning/learning-log.md).
- 2026-10-24: Operational. Samba shares set up and reached from my phone. Empty shares turned out to be unmounted drives.
- 2026-10-25: Remounted with client-friendly permissions, sorted files from Windows over the network, cleared Windows-locked folders from the server side. Made the media drive mount at boot with `nofail`. Retired the old 120 GB HDD and physically destroyed it (drilled, then saltwater). Thats the standard for any retired drive.
- Later: moved SSH to key-based login.
- 2026-10-28: 500 GB #1 installed. 500 GB #2 placed but left unpowered. Picked the Thermaltake GF1 850W as the new targeted PSU.

## Next
1. Confirm mounts, then back up the OS/config files first.
2. Health-check (SMART) every drive.
3. Switch the fstab entry to a UUID so adding drives can't reshuffle names.
4. Configure shares and permissions, then set up Nextcloud.
5. Backups before real use. Permanent spot, then headless operation.
