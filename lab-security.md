# Lab Security
This page stays high-level on purpose. It describes the approach and why, not the exact measures.

## Approach
- **Nothing exposed by default.** Services are local-only. Outside access only comes after the firewall is in place, and only through an encrypted tunnel.
- **Firewall first.** The lab's internet edge will run through a dedicated firewall, with the lab kept on its own isolated network segment.
- **Test before trusting.** Changes go to an isolated lab segment first and always have a rollback path.
- **Separate admin.** Day-to-day use and administration are kept apart, and admin access uses strong authentication (hardware keys are planned).
- **Backups off the machine.** 3-2-1: primary copy on the NAS, a separate backup, and an archive.
- **Retired drives get destroyed**, not reused or sold.
- **Patch quickly.** A weekly security check covers every platform I actually run, and I verify alerts against official sources before acting.
- **AI agents advise; I act.** My agents never get logins, passwords, or network details. I apply every change myself.

## Status
| Area | Status |
|-----------------------------|-------------|
| Firewall                    | Planned     |
| Network segmentation        | Planned     |
| Remote access               | Planned     |
| Hardware security keys      | Planned     |
| Backups                     | In progress |
| Patching and security watch | Active      |

## Lab experiments
The lab exists to learn, so some machines (like the temporary NAS) make deliberate trade-offs that a permanent build won't. Those get fixed before anything holds real data or faces the internet.

### Planned networking and security lab

Inspired by CyberFlow’s [The FUN Way to Learn Networking](https://youtu.be/saSPsB-Afuw), I plan to use a small virtual lab for hands-on networking and security practice: configuring services, inspecting traffic, troubleshooting mistakes, and documenting what I learn.

Tools and resources to explore:
- Wireshark — packet capture and traffic analysis.
- Nmap — host discovery and service scanning.
- Scapy — packet structure and controlled packet crafting.
- Burp Suite and mitmproxy — inspecting test web traffic.
- VirtualBox — a possible platform for practice VMs.
- Metasploitable — an intentionally vulnerable practice target.
- TryHackMe and Root-Me — guided security exercises.
- Pi-hole and PiVPN — learning DNS filtering and VPN concepts.

Testing will stay within my own or explicitly authorized systems. Intentionally vulnerable targets will use an isolated virtual network, without bridged networking or internet exposure.

These are planned learning resources, not installed services or a commitment to deploy everything. AdGuard Home remains Starrlight’s chosen DNS-filtering service; the learning examples do not change the infrastructure plan.