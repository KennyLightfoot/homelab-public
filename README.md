# Home Lab Portfolio — Infrastructure, Networking, and Monitoring

Case studies from a personal Proxmox lab: what I built, what I checked, what changed, and how I verified the result. Lab work is separate from employment and business operations. Uptime Kuma also monitors the public website of my own business; that monitoring target does not make the rest of this environment a production network.

**Documentation reconciled: September 24, 2026.** Status below reflects recorded evidence and known open items. This date is a documentation review, not a new live health check. Earlier test results remain historical until repeated.

Sanitized evidence excludes credentials, customer data, public IPs, MAC addresses, and private management details. See [SECURITY.md](./SECURITY.md) and the [publication checklist](./docs/publication-checklist.md).

## Environment and recorded status

| Component | Recorded configuration | Status at documentation review |
|---|---|---|
| Proxmox host | Ryzen 7 6800H mini PC, 32 GB RAM, 1 TB NVMe, Proxmox VE 9.1 | Existing lab host; no new health check performed for this review |
| Guests | Ubuntu services VM, OPNsense VM, Windows Server 2025 DC, Windows 11 client, LAN/DMZ/lab LXC test boxes | Windows client rebuild pending; test LXCs were stopped at the last recorded inventory |
| Physical network | UniFi USW-Flex-2.5G-5, 2.5 GbE uplink to Proxmox | Flat/default physical network; OPNsense is a virtual lab router, not the home network's inline gateway |
| Virtual networking | Proxmox bridges and OPNsense firewall policy for LAN, DMZ, and lab zones | Five isolation/reachability tests passed in the recorded exercise; no new retest in this review |
| Monitoring | Docker on Ubuntu, managed over SSH; Uptime Kuma, Prometheus, Grafana, node_exporter, cAdvisor | Nine recorded Uptime Kuma monitors: eight lab services and one production website; no notification delivery configured |
| Backups | PNY CS900 2 TB SSD over USB, Proxmox snapshot-mode backups with retention | A guest restore was validated; scheduled coverage is incomplete |
| Ticketing | osTicket deployed with Docker Compose | Used for lab incidents; the next AD validation ticket is still pending |

## Case studies and projects

| Project | Recorded evidence | Limit or next verification |
|---|---|---|
| [Proxmox backup and recovery](./case-studies/01-proxmox-backup-recovery/) | Restored a guest, then checked boot, DHCP, routing, and DNS | A successful test does not establish complete guest backup coverage |
| [OPNsense virtual network segmentation](./projects/01-opnsense-segmentation/) | LAN, DMZ, and lab zones with firewall policy; five planned tests passed | Virtual lab exercise; stopped test guests need starting and rechecking before a live demonstration |
| [Uptime Kuma monitoring](./projects/03-uptime-kuma/) | Nine monitors; production-site history includes HTTP timeouts and a DNS failure | Availability history does not establish alert delivery |
| [Pi-hole DNS filtering](./projects/02-pihole/) | Containerized DNS filtering; allowed/blocked lookups documented | Historical validation; repeat the relevant checks when demonstrating |
| [Prometheus and Grafana](./projects/04-grafana-stack/) | Documented host/container metrics stack | Stale targets and notification delivery remain open; do not describe the pipeline as fully validated today |
| [Windows AD support lab](https://github.com/KennyLightfoot/windows-ad-support-homelab) | Windows Server 2025, AD DS, DNS, OUs, users/groups, file share, GPO | Windows 11 client rebuild in progress; current domain sign-in is not validated |

## Known limitations and next work

- **Windows client:** complete the domain-controller health check, rebuild the client, then document one AD support ticket with verification.
- **Backup coverage:** recorded scheduled jobs cover OPNsense, the LAN/DMZ/lab test containers, and the main Ubuntu services VM. Staging, rehearsal, and both Windows VMs are outside those recorded schedules. Check any one-off backups individually before relying on them.
- **Monitoring:** no home-lab notification delivery is configured. Grafana stale-target cleanup remains open. Validated CPU/disk rules from my separate AWS coursework are not evidence of delivered lab notifications.
- **[Suricata IDS](./projects/05-suricata-ids/):** parked; prior documentation records memory-pressure failures. It is not presented as an operational security service.

## Certifications and education

CompTIA A+ · Network+ · Security+ · AWS Certified Cloud Practitioner · ITIL 4 Foundation · LPI Linux Essentials

B.S. Cloud & Network Engineering (AWS track), Western Governors University — expected December 2026; 83 of 112 credits complete.

## Connect

[LinkedIn](https://www.linkedin.com/in/kenneth-lightfoot/) · [GitHub profile](https://github.com/KennyLightfoot)
