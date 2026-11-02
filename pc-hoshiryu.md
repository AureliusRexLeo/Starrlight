# Hoshiryū
Last updated: 2026-10-30

## Why
A Steam Machine-style console PC for wife.

## Target
- GPU end goal: RX 9070 GRE (RX 9060 XT only if it's $400 or less).
- Storage: 1 TB+ TLC NVMe with DRAM.

## Now
- CPU: AMD Ryzen 5 7600X3D. Board: ASRock B650M Pro lightning WiFi 6E
- RAM: 16 GB DDR5-5600 (2x8 Patriot Viper).
- GPU: PowerColor Hellhound RX 6650 XT (since Oct 27).
- Cooler: Assassin X 120R ARGB. Case: Cooler Master Q300L. Fans: 2x MF140 front + 120 mm rear.
- PSU: EVGA 650 W Bronze. Storage: 500 GB WD Blue SATA SSD.
- OS: Bazzite, bazzite-deck image (Game Mode).
- BIOS: Resizable BAR on. Secure Boot off, per the Bazzite guide.

## Log
- 2026-10-18: Built around a donated GTX 1080. SteamOS wouldn't work with the NVIDIA card, so I went with Bazzite. The NVIDIA Deck image didn't work either, so it ran desktop mode. Stable the whole time; the 1080 just couldn't do 4K60.
- 2026-10-27: Swapped the GTX 1080 for an RX 6650 XT and rebased to the bazzite-deck image. No reinstall needed; the rebase kept home, Flatpaks, Steam library, and settings. The 1080 goes to Warspite.
- 2026-10-30: BIOS settings confirmed (Resizable BAR on, Secure Boot off).

## To do
- EmuDeck: copy the rest of the ROMs, re-configure for Game Mode and the new card, re-run Steam ROM Manager.
- Galaxy-colors RGB mode and fan curves.
- Optional: turn Secure Boot back on after enrolling the Bazzite key.
