# Budget

High-level expected / willing-to-pay figures for Starrlight.  
Line items are estimates and may change.  
Sections note the source where relevant.

## Network
- MikroTik CRS326-24G-2S+RM: $200
- CyberPower 1000 VA pure-sine UPS: $180
- ARRIS SB8200 modem (certified refurbished - new): $80–140
- TP-Link Archer AX55 (Wi-Fi 6, AP mode): $75–90
- Homelab subtotal (pre-tax, low–high): $980–1,055
- Homelab with 8% tax (low–high): $1,058–1,139

## Server (Sovereign) — full planned build
*Not the base starting build. Goal is all-new parts. CPU planned to be sourced through a friend.
Other components open to used/alternative where it makes sense, but new is preferred for this machine.*

- CPU: AMD Ryzen 9 5900XT 3.3 GHz 16-Core — $289.99 (price is new. plan is sourced via friend)
- CPU Cooler: Thermalright Peerless Assassin 120 SE — $31.41
- Motherboard: ASRock B550M Steel Legend Micro ATX — $99.99
- Memory: TEAMGROUP Elite 32 GB (2 x 16 GB) DDR4-2666 — $219.99 (will add another 32gb when cost is reasonable)
- Storage: Kingston NV3 1 TB M.2 NVMe — $158.99 (OS/ Cache) (cheaper alternatives are available/owned)
- Case: Phanteks Eclipse G370A ATX Mid Tower — $43.98 (design irrelevant, airflow and fit key)
- Power Supply: ASRock PRO-650G 650 W 80+ Gold — $49.99 (considering 750w for additional headroom)

**Total (PCPartPicker, 2026-10-22):** $894.34  
Prices include shipping, taxes, and discounts when available.  
[PCPartPicker list](https://pcpartpicker.com/list/ygRZXf)

## NAS (Leviathan) — target platform

### New parts
- CPU: Intel Core i3-14100 3.5 GHz Quad-Core — $134.99
- Motherboard: ASRock H610M-HDV/M.2 R2.0 Micro ATX — $63.99
- Memory: Kingston KCP426NS6/4 4 GB DDR4-2666 (×2) — $39.47 × 2
- Storage: Kingston NV3 500 GB M.2 NVMe — $109.00
- Power Supply: ASRock PRO-650G or equivalent — $49.99 
  
**New-parts total (PCPartPicker, 2026-10-22):** $436.91  
[PCPartPicker list](https://pcpartpicker.com/list/6MJjR4)

### Used alternative
Minimum of an i3-12000-series non-F or higher (T low-power version ideal) bundled with board and RAM when possible.  
Target: 16 GB RAM + 500 GB SSD.  

- Do not spend more than ~$220 on an 8 GB RAM bundle.  
- ~$250 on a 16 GB bundle is acceptable.  
- Example found: bundle with everything needed except PSU and SSD for $250 → roughly $400 total once those are added.  

Exact used pricing varies; stay inside the caps above.

### Interim storage (used SAS array)
Used SAS array to bridge until IronWolf (or equivalent NAS-class) pricing improves — $160–170.

## Media PC (Warspite)

- Micro Center bundle (CPU + board + 16 GB RAM): Ryzen 5 5600X3D + Gigabyte B550M DS3H AC R2 + G.Skill Ripjaws V 16 GB DDR4-3200 — $300
- CPU Cooler: Thermalright Assassin X 120 Refined SE — $17.98
- Power Supply: ASRock PRO-650G 650 W 80+ Gold (or equivalent Gold-rated) — ~$50
- Case: Lian Li A3 mATX (or similar) — $70–80
- OS drive: 500 GB SATA SSD (WD Blue or equivalent) — ~$40–50
- Additional storage (goal, not needed to start): Kingston NV3 1 TB M.2 NVMe — ~$150
- Internal optical: ASUS BW-16D1HT (or LG WH16NS40) — M-DISC + BDXL, UHD-capable after LibreDrive — ~200 (will settle for regular bd writer to start).  
  Optional USB enclosure if needed later — ~$15–25

**Rough total (new parts):** ~$630–650  
Exact pricing varies by vendor and whether the NVMe/ uhd writer is added immediately.

## AI server (Argus)

Platform is intentionally simple; GPU is the main variable.

- CPU: Ryzen 5 5600 / 5600X / 5700X — $80–160
- Motherboard: X570 with two full x16 slots — $70–150
- Memory: 32 GB minimum DDR4, 64 GB preferred — up to $500
- Power Supply: 850 W 80+ Gold with two native 8-pin (or 8+8) cables; higher if the chosen GPU requires it — up to $150
- Case, cooler, fans, and extras — up to $150
- GPU (first card): target $1,500, hard max $1,700  
  (Options under consideration: 3090, V100, RX 7900-class, or Arc workstation card — chosen by tok/s vs. price vs. age vs. what’s coming. Second card planned later. Tensor use and engine optimization factored in.)

**Platform subtotal (excluding GPU):** roughly $370–1,110 depending on RAM and exact parts.  
GPU is the dominant cost and will be finalized closer to build time.

### Future platform upgrade (far future)
Eventual goal is to move to an ECC-capable platform when needed or budget allows.  
Practical target: X870E or X670E board with documented x8/x8 bifurcation and UDIMM ECC support (ProArt / Creator / Taichi / Aorus Elite class).  
Full RDIMM ECC + three GPUs would be workstation territory and is only considered if a third card is actually planned but full server build is not ruled out for eventual progress.

## NVR (Vanguard)
Planned: $3,000+ to start.
Details not published.

## Other notes
- LyonHeart (Main PC) target PSU: Corsair RM750e.
- Hoshiryū (Wife PC) GPU end goal: 9070 GRE primary; 9060 XT only if it returns to ≤$400.
- UPS: shared pure-sine unit for the two personal PCs (same room). Full homelab stack targets ~1500 VA pure-sine class (e.g. CyberPower CP1500PFCLCD or equivalent) for graceful shutdown headroom.
- Figures are planning estimates, not firm quotes. Updated as hardware decisions lock in.