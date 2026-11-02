# Lessons Log
Things I learned from watching other people: posts, write-ups, and other people's fixes, and what I changed in my own lab because of them. My own troubleshooting goes in the [learning log](learning-log.md).

## 2026-10-30: Using AI agents as a security lookout (CVE-2026-98432)
- **What happened:** I saw a [post on X by @IvanPerezF_](https://x.com/ivanperezf_/status/2115900137016848684) (Oct 29, 2026) warning about a critical Linux kernel flaw, CVE-2026-98432. It's in IPv4/TCP connection handling: a remote attacker with no login and no user action needed could trigger it.
- **Verify before acting:** I had my AI bots check the claim against primary sources before trusting a social media post. It was real. The kernel's own security team (the CVE Numbering Authority) rates it CVSS 9.8, and Oracle gives it a preliminary 8.1 because it's a race condition that's hard to trigger. It was published Oct 15, 2026, isn't on CISA's Known Exploited list, and is fixed in kernels 5.10.272, 5.15.223, 6.1.190, 6.6.159, 6.12.113, 6.18.56, 7.2.10, and 7.3.
- **Response:** Seeing the post triggered immediate update checks on both of my Linux systems:
  - Leviathan (Ubuntu Server): `sudo apt update && sudo apt full-upgrade -y`, reboot, then `uname -r` to confirm the new kernel.
  - Hoshiryū (Bazzite): `rpm-ostree status`, and reboot if an update is staged.
  - My Windows PC wasn't affected.
- **Exposure:** Leviathan is LAN-only with no ports forwarded, so the risk was lower. That's one more reason the firewall comes first.
- **Making it routine:** I had the bots schedule a weekly security check across every platform that's actually running. Each week it looks for new vulnerabilities, breaches, and vendor trust issues, reports only what applies to my setup with fix steps, and keeps a running log.
- **Boundaries:** The bots never get my logins, passwords, or network details (IPs, configs). They watch and advise, and I apply every fix myself.
- **Lesson:** Treat social media as an early warning, not a source. Verify against the CVE record and vendor advisories, check your own exposure, then patch. Automating the watch means I don't depend on catching the right post.
