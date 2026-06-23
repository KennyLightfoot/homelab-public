# Project 1: OPNsense Network Segmentation

![Status](https://img.shields.io/badge/Status-Complete-success)
![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)

## Overview

Designed and implemented a segmented lab network on a Proxmox hypervisor using OPNsense as the router and firewall. Three isolated network segments were created — LAN, DMZ, and Lab — each with its own Linux container as a host. Firewall rules enforce strict isolation between segments while allowing all segments to reach the internet.

This mirrors how enterprise networks and AWS VPCs are designed: different trust zones, controlled traffic flow between them, and a single routing appliance enforcing the policy.

---

## Goals

- Create isolated network segments for different trust levels
- Route traffic between segments through a firewall (not a flat switch)
- Prove isolation: DMZ and Lab cannot reach the LAN
- Prove controlled access: all segments can reach the internet
- Build a foundation for future IDS/IPS and SIEM projects

---

## Architecture

```mermaid
graph TD
    Internet((Internet))
    Router["Home Router\n192.168.1.254"]

    subgraph Proxmox["Proxmox VE 9.1.1 — pve-homelab (192.168.1.50)"]

        vmbr0["vmbr0 — WAN Bridge\n192.168.1.0/24\nbridged to physical NIC"]

        subgraph OPNsense["VM 101 — OPNsense 26.1.6 (2 vCPU / 2 GB RAM)"]
            WAN["vtnet0 — WAN"]
            LAN_IF["vtnet1 — LAN\ngateway 10.10.1.1"]
            DMZ_IF["vtnet2 — OPT1 / DMZ\ngateway 10.10.2.1"]
            LAB_IF["vtnet3 — OPT2 / Lab\ngateway 10.10.3.1"]
        end

        subgraph LAN["vmbr1 — LAN Segment (10.10.1.0/24)"]
            lan_box["CT200 lan-box\nDHCP"]
        end

        subgraph DMZ["vmbr2 — DMZ Segment (10.10.2.0/24)"]
            dmz_box["CT201 dmz-box\n10.10.2.10"]
        end

        subgraph Lab["vmbr3 — Lab Segment (10.10.3.0/24)"]
            lab_box["CT202 lab-box\n10.10.3.10"]
        end

    end

    Internet --> Router
    Router --> vmbr0
    vmbr0 --> WAN
    LAN_IF --> LAN
    DMZ_IF --> DMZ
    LAB_IF --> Lab
```

---

## Tech Stack

| Component | Version | Role |
|---|---|---|
| Proxmox VE | 9.1.1 | Hypervisor |
| OPNsense | 26.1.6_2 (amd64) | Router / Firewall |
| Debian LXC | — | Segment target hosts |

---

## Network Design

| Segment | Bridge | Subnet | Gateway | Trust Level |
|---|---|---|---|---|
| WAN | vmbr0 | 192.168.1.0/24 | 192.168.1.254 | External |
| LAN | vmbr1 | 10.10.1.0/24 | 10.10.1.1 | Trusted |
| DMZ | vmbr2 | 10.10.2.0/24 | 10.10.2.1 | Semi-trusted |
| Lab | vmbr3 | 10.10.3.0/24 | 10.10.3.1 | Untrusted |

### Why three segments?

**LAN** is the trusted zone — production services and management traffic live here. Nothing from DMZ or Lab should ever reach it.

**DMZ** (Demilitarized Zone) is for services that need internet access but shouldn't touch the LAN. In a real network this is where public-facing web servers live. Here it's a staging zone for future projects.

**Lab** is the untrusted zone for attack/defense exercises. Kali Linux will run here. It is the most restricted — blocked from both LAN and DMZ.

This is the same model used in AWS with public subnets (DMZ), private subnets (LAN), and isolated subnets (Lab).

---

## Implementation

### Step 1 — Create Proxmox bridges

Four bridges were configured in `/etc/network/interfaces` on the Proxmox host:

- **vmbr0** — bridged to the physical NIC (`nic0`), connects to the home network
- **vmbr1, vmbr2, vmbr3** — internal-only bridges with no physical port (`bridge-ports none`)

Internal bridges have no physical attachment — they are virtual switches that only carry traffic between VMs and containers on the Proxmox host. OPNsense is the only entity with a leg in all four, making it the sole routing and filtering point.

### Step 2 — Deploy OPNsense VM

OPNsense VM (ID 101) was created with four virtual NICs, one per bridge:

| NIC | Bridge | OPNsense Interface | Role |
|---|---|---|---|
| net0 (virtio) | vmbr0 | vtnet0 / WAN | Uplink to home network |
| net1 (virtio) | vmbr1 | vtnet1 / LAN | Trusted segment |
| net2 (virtio) | vmbr2 | vtnet2 / OPT1 | DMZ segment |
| net3 (virtio) | vmbr3 | vtnet3 / OPT2 | Lab segment |

OPNsense was installed from ISO and interfaces were assigned through the console menu. Each interface was given a static gateway IP (the `.1` address of its subnet).

### Step 3 — Configure firewall rules

OPNsense firewall rules are applied **inbound on each interface** (traffic entering OPNsense from that segment):

**LAN (vtnet1):** allow all — this is the trusted zone.

**DMZ / OPT1 (vtnet2):**
- Block traffic destined for the LAN subnet (10.10.1.0/24)
- Allow all other traffic (internet access passes through)

**Lab / OPT2 (vtnet3):**
- Block traffic destined for the LAN subnet (10.10.1.0/24)
- Block traffic destined for the DMZ subnet (10.10.2.0/24)
- Allow all other traffic (internet access passes through)

### Step 4 — Deploy LXC containers

Three Debian LXC containers were created as target hosts, one per segment:

| Container | ID | Bridge | IP Config |
|---|---|---|---|
| lan-box | CT200 | vmbr1 | DHCP (assigned by OPNsense) |
| dmz-box | CT201 | vmbr2 | 10.10.2.10/24 static |
| lab-box | CT202 | vmbr3 | 10.10.3.10/24 static |

Static IPs are set directly in the Proxmox container config (`pct config`) so they survive reboots without depending on DHCP.

---

## Isolation Verification

All tests run from the Proxmox host using `pct exec` to run commands inside each container.

### Blocked traffic (expected: 100% packet loss)

```bash
# DMZ cannot reach LAN gateway
pct exec 201 -- ping -c 2 -W 2 10.10.1.1
# 2 packets transmitted, 0 received, 100% packet loss ✓

# Lab cannot reach LAN gateway
pct exec 202 -- ping -c 2 -W 2 10.10.1.1
# 2 packets transmitted, 0 received, 100% packet loss ✓

# Lab cannot reach DMZ host
pct exec 202 -- ping -c 2 -W 2 10.10.2.10
# 2 packets transmitted, 0 received, 100% packet loss ✓
```

### Allowed traffic (expected: internet reachable)

```bash
# DMZ can reach internet
pct exec 201 -- ping -c 2 8.8.8.8
# 2 packets transmitted, 2 received, 0% packet loss ✓

# Lab can reach internet
pct exec 202 -- ping -c 2 8.8.8.8
# 2 packets transmitted, 2 received, 0% packet loss ✓
```

All five tests passed. Segment isolation is confirmed.

---

## Key Concepts Learned

- **Virtual switches vs routed networks** — bridges are layer 2 (like a physical switch); routing between them requires a layer 3 device (OPNsense)
- **Stateful firewall rules** — OPNsense tracks connection state, so return traffic is automatically allowed without explicit rules
- **Inbound rule direction** — rules are written from the perspective of traffic *entering* an interface, not leaving
- **Defense in depth** — layering multiple enforcement points (segment isolation + firewall rules) means no single misconfiguration exposes everything
- **AWS parallel** — Proxmox bridges = VPC subnets; OPNsense = VPC route table + security groups; LXC containers = EC2 instances

---

## What's Next

- **Project 5: Suricata IDS** — completed passive PCAP-based traffic inspection on OPNsense
- **Project 6: Wazuh SIEM** — ship firewall, IDS, and endpoint logs to a central Wazuh manager for alerting and correlation
- **Project 7: AWS VPC with Terraform** — reproduce this exact architecture in AWS as infrastructure-as-code

---

**Completed:** June 2026
