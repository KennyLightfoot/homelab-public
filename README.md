# Homelab Documentation

> **Cloud Engineering & Security Portfolio**
> Hands-on infrastructure projects built on a self-hosted Proxmox environment, documenting real-world networking, security, and cloud engineering skills.

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
| **Mini PC (`pve-homelab`)** | Proxmox VE host — `192.168.1.50`, ~32 GB RAM. Always-on lab fabric. |
| **Fedora Laptop** | Cockpit — drives everything via SSH. Git, Terraform, AWS CLI live here. |
| **Main PC** | Burst compute, local AI (Ollama). |
| **Old Lenovo** | Wazuh-monitored endpoint + study box. |

**Hypervisor:** Proxmox VE
**Networking:** OPNsense VM (`192.168.1.50:8006`) routing between segmented internal bridges

---

## VM & Container Inventory

| ID | Name | Type | RAM | Purpose |
|---|---|---|---|---|
| 100 | `Carlvis-Ubuntu` | VM | 12 GB | AI/business infra — Docker host for all services |
| 101 | `opnsense` | VM | 2 GB | Lab router/firewall |
| 200 | `lan-box` | LXC | ~0.75 GB | Target host on LAN segment |
| 201 | `dmz-box` | LXC | ~0.75 GB | Target host on DMZ segment |
| 202 | `lab-box` | LXC | ~0.75 GB | Target host on Lab segment |

### Services Running on Carlvis (VM 100)

| Service | Port | Purpose |
|---|---|---|
| Pi-hole | `:53` / `:8080` | Network-wide DNS ad blocking |
| Uptime Kuma | `:3002` | Uptime monitoring for hosted services |
| Portainer | `:9000` / `:9443` | Docker container management UI |
| Grafana | `:3000` | Metrics dashboards |
| Prometheus | `:9090` | Metrics collection |
| InfluxDB | `:8086` | Time-series database |
| cAdvisor | `:8085` | Container resource metrics |
| Node Exporter | `:9101` | Host-level metrics |
| n8n | `:5678` | Workflow automation |
| Open WebUI | `:3010` | LLM chat interface |
| SearXNG | `:8888` | Private search engine |
| Ollama | — | Local LLM backend |
| Qdrant | `:6333` | Vector database (RAG) |
| Homepage | `:8082` | Internal service dashboard |

---

## Projects

### ✅ [Project 1: OPNsense Network Segmentation](./projects/01-opnsense-segmentation/)
**Status:** Complete
**Skills:** Proxmox, OPNsense, Network Segmentation, Firewall Rules, Linux Containers

Built a segmented lab network using three Proxmox bridges (LAN, DMZ, Lab) and an OPNsense VM as the router/firewall. LXC containers deployed in each segment. Isolation proven: DMZ and Lab cannot reach LAN; both reach internet.

**Tech Stack:** Proxmox VE, OPNsense, LXC

---

### ✅ [Project 2: Pi-hole DNS](./projects/02-pihole/)
**Status:** Complete
**Skills:** DNS, Docker, Network Security, Ad Blocking

Deployed Pi-hole on Carlvis as a network-wide DNS sinkhole. Documented Docker deployment, blocklists, gravity database behavior, and DNS-layer security concepts.

**Tech Stack:** Docker, Pi-hole, Carlvis-Ubuntu

---

### ✅ [Project 3: Uptime Kuma Monitoring](./projects/03-uptime-kuma/)
**Status:** Complete
**Skills:** Docker, Infrastructure Monitoring, Alerting

Deployed Uptime Kuma on Carlvis to monitor external web availability, internal service ports, and core infrastructure hosts.

**Tech Stack:** Docker, Uptime Kuma, Carlvis-Ubuntu

---

### ✅ [Project 4: Grafana Monitoring Stack](./projects/04-grafana-stack/)
**Status:** Complete
**Skills:** Observability, Metrics, Docker, Prometheus, InfluxDB

Full observability stack on Carlvis: Prometheus scraping node-exporter and cAdvisor, Grafana dashboards, InfluxDB for time-series data.

**Tech Stack:** Docker, Grafana, Prometheus, InfluxDB, cAdvisor, Node Exporter

---

### ✅ [Project 5: Suricata IDS](./projects/05-suricata-ids/)
**Status:** Complete
**Skills:** Intrusion Detection, Network Monitoring, Security Logging, SIEM Prep

Deployed Suricata on OPNsense in passive PCAP IDS mode on the WAN interface. Enabled Emerging Threats and abuse.ch rulesets, confirmed alert logging, and prepared EVE JSON output for future Wazuh SIEM integration.

**Tech Stack:** OPNsense, Suricata, Emerging Threats Open, abuse.ch

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

## Connect

**LinkedIn:** https://www.linkedin.com/in/kenneth-lightfoot/
**WGU Program:** Cloud and Network Engineering — AWS (In Progress)
