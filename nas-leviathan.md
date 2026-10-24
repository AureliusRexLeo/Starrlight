# Leviathan (NAS)
Status: running - temporary

## Why

Leviathan is the central storage and media server for the house and a small circle of family/friends.  
It holds the primary copy of important data (the “3” in the 3-2-1 backup plan) and serves media (not general file storage).  
Each person gets their own encrypted share for the media they care about.  

It is also the source for the cold archival leg: optical discs are burned quarterly (incremental of the most important material) from data stored here.
The burner will live either in this machine or, more likely, on the media PC and pull the needed files over the network.  

The current temporary hardware exists only to get data and shares online while the proper platform is assembled.
The long-term goal is a stable, low-power NAS that can grow with mixed drives and later move to better storage.

## Target
i3-12100 non-F, UHD 730
16 GB DDR4
500gb Nmve os/cache drive.
Unraid on USB (Unraid is temporary. Long-term target is ZFS (or equivalent) once the drive layout is settled).
Jellyfin + file shares
Mixed disks now. Plan is to move to SAS drives as an interim step, then to proper NAS-class drives when affordable.

## Now
Temporary NAS (Zalman):
- CPU: AMD FX-6300 (AM3+)
- Board: ASUS M5A97 R2.0 (970 + SB950, 6× SATA 6Gb/s, 4× DDR3, Realtek 8111F GbE, no onboard video)
- Case: Zalman ATX (5× 3.5", 5× 5.25", 1 hidden tray)
- Optical: ASUS DVD-RW in one bay
- GPU: R7 260X available, not currently installed
- Cooler: old Corsair H60 already removed — needs replacement
- OS/temp drive: 120 GB SATA SSD (~30% life, transfer only)
- OS: Ubuntu Server
- Status: running. Holds network data until the i3-12100 platform is ready. Backups required.

## Log
- 2026-10-18  Temporary FX-6300 platform documented. Cooler already removed.
- 2026-10-24  system operational. troubleshooting/configuring in progress. Network configured. Progress taking place when time allows. Learning more linux terminal commands.