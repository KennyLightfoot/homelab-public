# Home Lab Portfolio — Infrastructure, Networking, and Monitoring

Case studies from a Proxmox home lab, written the way I would write an incident or change record: what the problem was, what I checked, what I changed, and how I verified it. This is a personal lab, not production or employment experience — with one exception noted below.

> **Sanitized:** credentials, customer data, public IPs, MAC addresses, and private management details are excluded. See [SECURITY.md](./SECURITY.md) and the [publication checklist](./docs/publication-checklist.md).

## Environment

| Piece | What it is |
|---|---|
| Host | Mini PC, AMD Ryzen 7 6800H, 32 GB RAM, 1 TB NVMe, Proxmox VE 9.1 |
| Guests | Ubuntu VM running the Docker services below; OPNsense VM; Windows Server 2025 DC and Windows 11 client ([separate repo](https://github.com/KennyLightfoot/windows-ad-support-homelab)); LXC test boxes for the LAN, DMZ, and Lab zones |
| Network | UniFi USW-Flex-2.5G-5 managed switch, 2.5 GbE to the Proxmox host. The physical network is one flat VLAN; all segmentation in this repo is virtual, done with Proxmox bridges and the OPNsense VM |
| Backups | PNY CS900 2 TB SSD over USB, Proxmox storage `pve-backup`; scheduled snapshot-mode ZSTD jobs with retention, restore tested |
| Docker services | Uptime Kuma, Prometheus, Grafana, node_exporter, cAdvisor, InfluxDB, Pi-hole, osTicket, Portainer |

## Case studies and projects

| | Evidence | What it shows |
|---|---|---|
| **[Proxmox backup and recovery validation](./case-studies/01-proxmox-backup-recovery/)** | Separate backup storage, scheduled jobs, a real LXC restore, then boot / DHCP / routing / internet / DNS checks on the restored guest | A backup is not a backup until a restore has been tested |
| **[OPNsense virtual network segmentation](./projects/01-opnsense-segmentation/)** | LAN, DMZ, and Lab zones on Proxmox bridges, routed through OPNsense with firewall policy; all five planned isolation and reachability tests verified | Trust zones, firewall rules, controlled testing |
| **[Uptime Kuma monitoring](./projects/03-uptime-kuma/)** | HTTP/HTTPS, TCP, and ICMP monitors that separate application, transport, and network-layer failures. Also watches my business's public website; its history has caught real HTTP timeouts and a DNS resolution failure | Availability monitoring, failure isolation |
| **[Pi-hole DNS filtering](./projects/02-pihole/)** | Containerized DNS filtering with persistent config; allowed and blocked lookups validated | DNS troubleshooting, Docker operations |
| **[Prometheus and Grafana](./projects/04-grafana-stack/)** | Host and container metrics (node_exporter, cAdvisor) scraped by Prometheus and dashboarded in Grafana | Metrics pipeline, PromQL |

The Uptime Kuma line is the one place this lab touches production: it monitors the real website of the business I run. No alerting is configured yet — see Backlog.

## Backlog (honest status)

- **Alerting.** Uptime Kuma and Grafana have no notification channel configured. Next step: one Grafana alert rule fired under a controlled test, delivered to a contact point, and closed through an osTicket ticket. Until that is done, this lab has dashboards, not alerts.
- **Grafana stale targets.** Stale scrape targets from a hosting migration still need cleanup.
- **[Suricata IDS](./projects/05-suricata-ids/)** — parked. Deployment and EVE JSON output were validated, but the service exits under memory pressure on this host. The write-up documents the diagnosis and what remains.
- **Backup coverage.** The scheduled jobs cover OPNsense, the LXC boxes, and the Ubuntu services VM. The Windows lab VMs and the staging VM have one-off backups only and are not yet on a schedule.

## Certifications and education

CompTIA A+ · Network+ · Security+ · AWS Certified Cloud Practitioner · ITIL 4 Foundation · LPI Linux Essentials
B.S. Cloud and Network Engineering (AWS track), Western Governors University — expected December 2026

## Connect

[linkedin.com/in/kenneth-lightfoot](https://www.linkedin.com/in/kenneth-lightfoot/) · [github.com/KennyLightfoot](https://github.com/KennyLightfoot)
