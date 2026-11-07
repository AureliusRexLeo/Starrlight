# iPad — Dashboard
Last updated: 2026-11-07

## Purpose
Repurpose an older 12.9-inch iPad Pro as a Homarr dashboard client, not a server.

## Current status
- Reset and initial setup completed without a personal account.
- Dashboard configuration remains planned.
- Exact generation and installed iPadOS version to confirm before deployment.

## Security
The security review identified CVE-2026-86950, a CoreGraphics vulnerability that can allow code execution when processing a malicious file. Apple reports targeted exploitation.

The first- and second-generation models are limited to iPadOS 16 and 17 respectively. Apple lists fixes in newer releases unavailable to these models, with no listed fix on their legacy branches. Other legacy security updates may still be available.

The planned dashboard role requires separation from trusted devices, restricted dashboard access, and no personal accounts or general browsing. Isolation reduces exposure; it does not patch the vulnerability.

## Next steps
- Confirm the model and apply available updates.
- Configure and test network separation and only the access needed for Homarr.
- Configure Guided Access and a restricted dashboard view.
- Check battery condition and charging arrangements before extended display use.

## References
- [Apple security advisory](https://support.apple.com/en-us/149225)
- [Apple security releases](https://support.apple.com/en-us/100100)