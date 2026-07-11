# Homelab Documentation

> **Support, Networking, Cloud & Security Portfolio**  
> Hands-on infrastructure projects built on a self-hosted Proxmox environment, focused on troubleshooting, network operations, monitoring, recovery, automation, and defensive security.

> **Public-repo note:** Private management addresses, credentials, tunnel details, raw captures, and admin-specific access information are redacted or generalized.

## About

I am building this lab while completing WGU's B.S. in Cloud and Network Engineering — AWS track.

The portfolio is designed to support entry-level roles in:

- Cloud Support Engineering
- Network Support / NOC
- Technical Support / Application Support
- Junior Cloud Engineering
- SOC and cloud-security-adjacent work

**Certifications:** CompTIA A+, Linux Essentials, ITIL 4

---

## Current Progress

| Area | Status | Evidence |
|---|---|---|
| Network segmentation | ✅ Complete | OPNsense routing and firewall validation across LAN, DMZ, and Lab zones |
| DNS filtering | ✅ Complete | Pi-hole DNS sinkhole with query and blocklist validation |
| Uptime monitoring | ✅ Complete | Uptime Kuma monitoring internal and external services |
| Metrics and observability | ✅ Complete | Prometheus, Grafana, cAdvisor, Node Exporter, and InfluxDB |
| Backup and recovery | ✅ Complete | Dedicated Proxmox backup datastore plus successful LXC restore and live firewall snapshot tests |
| Network IDS | ⚠️ Remediation | Suricata deployed; stability and ruleset tuning are in progress after memory-pressure failures |
| Linux and Docker hardening | 🚧 In progress | Network remediation, port inventory, secret migration, and attack-surface reduction |
| Physical VLAN / SDN lab | 🚧 Next | Managed switch, second network interface, and Raspberry Pi infrastructure node |
| SIEM / centralized logging | 🚧 Planned | Wazuh manager and endpoint/log ingestion |
| AWS / Terraform | 🚧 Planned | AWS VPC architecture modeled from the homelab |
| Hybrid networking | 🚧 Planned | Encrypted home-lab-to-AWS connectivity |

See the current implementation handoff: **[Current-State Handoff and Pre-Hardware Runbook](./docs/current-state-handoff.md)**

---

## Architecture

```text
Household router / existing home network
        |
        +-- Fedora administration laptop
        +-- Carlvis-Ubuntu services VM
        +-- Proxmox VE host
                |
                +-- OPNsense lab firewall
                |       +-- LAN segment
                |       +-- DMZ segment
                |       +-- Security-lab segment
                |
                +-- LXC test systems
                +-- External backup datastore
```

The household router remains the real edge router. OPNsense currently protects the isolated lab environment so firewall and routing changes do not interrupt the household network.

---

## Hardware

| Machine / Device | Role |
|---|---|
| **Mini PC (`pve-homelab`)** | Proxmox VE host, Ryzen 7 class CPU, ~32 GB RAM, 1 TB internal NVMe |
| **External 2 TB SSD** | Proxmox VM/container backups, configuration archives, and recovery evidence |
| **Fedora laptop** | Administration cockpit for SSH, Git, Python, Terraform, and analysis |
| **Main PC** | Personal workstation and burst compute |
| **Old Lenovo** | Planned monitored endpoint and study system |

**Current networking:** OPNsense VM routes between isolated Proxmox bridges.  
**Next networking phase:** managed VLAN switch plus a second Linux-compatible network interface.

---

## VM and Container Inventory

| ID | Name | Type | RAM | Purpose |
|---|---|---|---:|---|
| 100 | `Carlvis-Ubuntu` | VM | 12 GB | Docker services, DNS, monitoring, dashboards, automation, and local AI services |
| 101 | `opnsense` | VM | 4 GB | Lab router, firewall, and IDS host |
| 200 | `lan-box` | LXC | 512 MB | Internal LAN test system |
| 201 | `dmz-box` | LXC | 512 MB | DMZ test system |
| 202 | `lab-box` | LXC | 512 MB | Security-lab test system |

### Major Carlvis Services

| Service | Purpose |
|---|---|
| Pi-hole | Network-wide DNS filtering |
| Uptime Kuma | Availability monitoring |
| Grafana | Dashboards and visualization |
| Prometheus | Metrics collection |
| InfluxDB | Time-series storage |
| cAdvisor | Container metrics |
| Node Exporter | Host metrics |
| n8n | Workflow automation |
| Portainer | Docker administration |
| Open WebUI | Local AI interface |
| SearXNG | Private search |
| Ollama | Local model runtime |
| Qdrant | Vector database |
| Homepage | Internal service dashboard |

---

## Fastest Proof for Recruiters

Start with these examples:

1. **[OPNsense Network Segmentation](./projects/01-opnsense-segmentation/)** — subnetting, trust zones, firewall policy, routing, and validation
2. **[Proxmox Backup and Recovery](./case-studies/01-proxmox-backup-recovery/)** — storage administration, scheduled backups, full restore testing, and operational evidence
3. **[Grafana Monitoring Stack](./projects/04-grafana-stack/)** — observability, exporters, dashboards, and troubleshooting data

---

## Projects

### ✅ [Project 1: OPNsense Network Segmentation](./projects/01-opnsense-segmentation/)

Built a segmented lab using OPNsense and separate LAN, DMZ, and Lab bridges. Lower-trust zones retain internet access while internal access is restricted and validated.

**Skills:** Proxmox, OPNsense, subnetting, firewall rules, routing, LXC

### ✅ [Project 2: Pi-hole DNS](./projects/02-pihole/)

Deployed Pi-hole as a DNS sinkhole and documented blocklists, queries, DNS behavior, and validation of blocked versus allowed domains.

**Skills:** DNS, Docker, Linux, ad/tracker blocking, network troubleshooting

### ✅ [Project 3: Uptime Kuma Monitoring](./projects/03-uptime-kuma/)

Monitors internal services, infrastructure hosts, ports, and a public endpoint through HTTP, TCP, and ICMP checks.

**Skills:** service monitoring, alerting, Docker, availability validation

### ✅ [Project 4: Grafana Monitoring Stack](./projects/04-grafana-stack/)

Built a monitoring stack using Grafana, Prometheus, cAdvisor, Node Exporter, and InfluxDB for host and container visibility.

**Skills:** observability, metrics, Docker, Prometheus, dashboards

### ⚠️ [Project 5: Suricata IDS](./projects/05-suricata-ids/)

Suricata was deployed on OPNsense in passive PCAP mode with Emerging Threats and abuse.ch rules. Historical alerting and EVE JSON output were validated. The current phase is operational remediation: the service exits under memory pressure, so the deployment is being tuned before it is marked stable again.

**Skills:** IDS, rulesets, packet inspection, log analysis, troubleshooting, SIEM preparation

### 🚧 Project 6: Wazuh SIEM

Planned centralized logging, endpoint monitoring, file-integrity monitoring, and Suricata/OPNsense event ingestion.

### 🚧 Project 7: AWS VPC with Terraform

Planned AWS network architecture modeled after the homelab and deployed through infrastructure as code.

### 🚧 Project 8: AWS Security Layer

Planned CloudTrail, GuardDuty, AWS Config, KMS, Secrets Manager, and alerting controls.

### 🚧 Project 9: Home Lab to AWS VPN

Planned encrypted hybrid-networking capstone connecting the local lab to AWS.

---

## Operational Case Studies

### ✅ [Proxmox Backup and Recovery Validation](./case-studies/01-proxmox-backup-recovery/)

Created physically separate backup storage, scheduled jobs, restored an LXC workload under a temporary ID, validated DHCP/routing/internet/DNS, tested a live OPNsense snapshot, and preserved recovery evidence.

### 🚧 Carlvis Linux and Docker Hardening

Current work includes duplicate-IP remediation, service and port inventory, migration of secrets out of Compose files, Cloudflare Tunnel review, and least-privilege reduction of backend and administrative port exposure.

---

## Current Roadmap

1. Finish Carlvis credential rotation and reduce unnecessary LAN exposure.
2. Validate the large scheduled VM backup and external-disk remount after reboot.
3. Add the managed switch, second network interface, and Raspberry Pi as a bench lab.
4. Extend virtual segments into physical VLANs.
5. Stabilize Suricata and generate a controlled alert.
6. Deploy Wazuh and onboard endpoints and network logs.
7. Add Kali and one intentionally vulnerable target.
8. Complete an attack → detect → investigate → remediate → retest exercise.
9. Build the AWS/Terraform and hybrid-networking phases alongside WGU coursework.

---

## Interview Positioning

The strongest framing is not "I installed a lot of tools." It is:

- Built segmented networks rather than a flat Docker host
- Monitored real services and used metrics to troubleshoot them
- Identified operational risks and completed a tested recovery process
- Investigated Linux networking problems and validated the repair
- Reduced exposed services and moved secrets out of configuration files
- Deployed IDS tooling, collected failure evidence, and planned remediation
- Documented what changed, why it mattered, and how the result was verified

---

## Connect

**LinkedIn:** https://www.linkedin.com/in/kenneth-lightfoot/  
**WGU Program:** Cloud and Network Engineering — AWS (In Progress)
