# Homelab Documentation

> **Cloud Engineering & Security Portfolio**
> Hands-on infrastructure projects built on a self-hosted Proxmox environment, documenting real-world networking, security, and cloud engineering skills.

> **Note:** Private RFC1918 addresses, management URLs, and admin-specific access details are redacted or generalized in this public repo. The focus here is architecture, validation, and operational reasoning.

## About

Working toward an entry-level cloud engineering / cloud security role.
Building this lab as part of my WGU B.S. in Cloud & Network Engineering (AWS track).

**Certs:** CompTIA A+, Linux Essentials, ITIL 4

---

## Current Portfolio Progress

| Area | Status | Evidence |
|---|---|---|
| Network segmentation | ✅ Complete | OPNsense firewall routing between LAN, DMZ, and Lab segments |
| DNS security | ✅ Complete | Pi-hole DNS sinkhole with curated blocklists |
| Uptime monitoring | ✅ Complete | Uptime Kuma monitoring for internal services and a public web endpoint |
| Metrics and observability | ✅ Complete | Prometheus, Grafana, cAdvisor, Node Exporter, and InfluxDB stack |
| Network IDS | ✅ Complete | Suricata running in passive PCAP IDS mode on OPNsense |
| SIEM / centralized logs | 🚧 Next | Wazuh manager and endpoint/log ingestion |
| AWS / Terraform | 🚧 Planned | AWS VPC architecture mirrored from the homelab |
| Hybrid networking | 🚧 Planned | Home lab to AWS WireGuard VPN capstone |

---

## Target Roles

This portfolio is aimed at entry-level roles in:

- Cloud Support Engineering
- Network Support / NOC
- Junior Cloud Engineering
- Cloud Security / SOC-adjacent roles
- Technical Support / Application Support with infrastructure exposure

---

## Hardware

| Machine | Role |
|---|---|
| **Mini PC (`pve-homelab`)** | Proxmox VE host, ~32 GB RAM. Always-on lab fabric. |
| **Fedora Laptop** | Cockpit — drives everything via SSH. Git, Terraform, AWS CLI live here. |
| **Main PC** | Burst compute, local AI (Ollama). |
| **Old Lenovo** | Wazuh-monitored endpoint + study box. |

**Hypervisor:** Proxmox VE management on a private subnet
**Networking:** OPNsense VM routing between segmented internal bridges

---

## VM & Container Inventory

| ID | Name | Type | RAM | Purpose |
|---|---|---|---|---|
| 100 | `Carlvis-Ubuntu` | VM | 12 GB | Docker services host for monitoring, DNS, dashboards, and lab services |
| 101 | `opnsense` | VM | 2 GB | Lab router/firewall |
| 200 | `lan-box` | LXC | ~0.75 GB | Target host on LAN segment |
| 201 | `dmz-box` | LXC | ~0.75 GB | Target host on DMZ segment |
| 202 | `lab-box` | LXC | ~0.75 GB | Target host on Lab segment |

### Services Running on Carlvis (VM 100)

| Service | Purpose |
|---|---|
| Pi-hole | Network-wide DNS ad blocking |
| Uptime Kuma | Uptime monitoring for hosted services |
| Portainer | Docker container management UI |
| Grafana | Metrics dashboards |
| Prometheus | Metrics collection |
| InfluxDB | Time-series database |
| cAdvisor | Container resource metrics |
| Node Exporter | Host-level metrics |
| n8n | Workflow automation |
| Open WebUI | LLM chat interface |
| SearXNG | Private search engine |
| Ollama | Local LLM backend |
| Qdrant | Vector database (RAG) |
| Homepage | Internal service dashboard |

---

## Projects

### Fastest Proof for Recruiters

If you only open three projects, start here:

1. **OPNsense Network Segmentation** — proves subnetting, firewalling, trust zones, and validation
2. **Grafana Monitoring Stack** — proves observability, exporters, Prometheus, and dashboard reasoning
3. **Suricata IDS** — proves security tooling, rulesets, detection strategy, and SIEM prep

### ✅ [Project 1: OPNsense Network Segmentation](./projects/01-opnsense-segmentation/)
**Status:** Complete
**Skills:** Proxmox, OPNsense, Network Segmentation, Firewall Rules, Linux Containers

Built a segmented lab network using three Proxmox bridges (LAN, DMZ, Lab) and an OPNsense VM as the router/firewall. LXC containers deployed in each segment. Isolation proven: DMZ and Lab cannot reach LAN; both reach internet.

**Tech Stack:** Proxmox VE, OPNsense, LXC

**Validation Evidence:** documented bridge layout, routing path through OPNsense, and proof that lower-trust segments cannot reach LAN while retaining internet access

---

### ✅ [Project 2: Pi-hole DNS](./projects/02-pihole/)
**Status:** Complete
**Skills:** DNS, Docker, Network Security, Ad Blocking

Deployed Pi-hole on Carlvis as a network-wide DNS sinkhole. Documented Docker deployment, blocklists, gravity database behavior, and DNS-layer security concepts.

**Tech Stack:** Docker, Pi-hole, Carlvis-Ubuntu

**Validation Evidence:** dashboard screenshots, live query log, gravity/blocklist counts, and DNS lookup tests for blocked vs allowed domains

---

### ✅ [Project 3: Uptime Kuma Monitoring](./projects/03-uptime-kuma/)
**Status:** Complete
**Skills:** Docker, Infrastructure Monitoring, Alerting

Deployed Uptime Kuma on Carlvis to monitor external web availability, internal service ports, and core infrastructure hosts.

**Tech Stack:** Docker, Uptime Kuma, Carlvis-Ubuntu

**Validation Evidence:** uptime dashboard screenshot, 9 active monitors, and mixed HTTP/TCP/ICMP checks across public and internal targets

---

### ✅ [Project 4: Grafana Monitoring Stack](./projects/04-grafana-stack/)
**Status:** Complete
**Skills:** Observability, Metrics, Docker, Prometheus, InfluxDB

Full observability stack on Carlvis: Prometheus scraping node-exporter and cAdvisor, Grafana dashboards, InfluxDB for time-series data.

**Tech Stack:** Docker, Grafana, Prometheus, InfluxDB, cAdvisor, Node Exporter

**Validation Evidence:** dashboard screenshots, active Prometheus targets, host metrics, and per-container visibility

---

### ✅ [Project 5: Suricata IDS](./projects/05-suricata-ids/)
**Status:** Complete
**Skills:** Intrusion Detection, Network Monitoring, Security Logging, SIEM Prep

Deployed Suricata on OPNsense in passive PCAP IDS mode on the WAN interface. Enabled Emerging Threats and abuse.ch rulesets, confirmed alert logging, and prepared EVE JSON output for future Wazuh SIEM integration.

**Tech Stack:** OPNsense, Suricata, Emerging Threats Open, abuse.ch

**Validation Evidence:** IDS settings screenshot, live alerts screenshot, enabled rulesets, and EVE JSON output prepared for SIEM ingestion

---

### 🚧 Project 6: Wazuh SIEM
**Status:** Next
**Skills:** Security Monitoring, Log Analysis, Endpoint Detection

Wazuh manager VM on Proxmox with agents on the Lenovo laptop and Fedora cockpit.

---

### 🚧 Project 7: AWS VPC with Terraform
**Status:** Planned
**Skills:** AWS, Terraform, Infrastructure as Code

Mirror the home lab network segments as AWS VPC infrastructure in Terraform — same architecture, as code.

---

### 🚧 Project 8: Cloud Security Layer on AWS
**Status:** Planned
**Skills:** GuardDuty, CloudTrail, AWS Config, KMS, Secrets Manager

---

### 🚧 Project 9: Site-to-Site WireGuard VPN (Home ↔ AWS)
**Status:** Planned
**Skills:** WireGuard, VPN, AWS, Network Architecture

Capstone — bridge the home lab and AWS VPC over an encrypted tunnel.

---

## Interview Positioning

This repo is strongest when positioned as:

- **Support + infrastructure troubleshooting proof** for Help Desk / Technical Support / Application Support
- **Monitoring + network fundamentals proof** for NOC / Cloud Support
- **Security exposure proof** for junior SOC / cloud security-adjacent roles

Best framing in interviews:

- Built segmented networks, not just flat Docker labs
- Monitored real services with Prometheus, Grafana, and Uptime Kuma
- Added DNS-layer filtering and passive IDS to understand detection before prevention
- Documented what was deployed, why it mattered, and how it was verified

---

## Connect

**LinkedIn:** https://www.linkedin.com/in/kenneth-lightfoot/
**WGU Program:** Cloud and Network Engineering — AWS (In Progress)
