# Homelab Current-State Handoff

**Updated:** July 2026  
**Purpose:** Current status, known risks, and the exact work required before adding the managed switch, second network interface, and Raspberry Pi infrastructure node.

> This public handoff is sanitized. Private management addresses, credentials, API keys, tunnel tokens, and raw packet captures are intentionally excluded.

## What This Document Is

This is a **current-state handoff and implementation runbook**. It is designed to answer four questions:

1. What is already working?
2. What is currently broken or risky?
3. What should be completed before the next hardware change?
4. What happens after the new equipment arrives?

## Current Architecture

```text
Home router / household network
        |
        +-- Fedora administration laptop
        +-- Carlvis-Ubuntu services VM
        +-- Proxmox host
                |
                +-- OPNsense lab firewall
                |       +-- LAN segment
                |       +-- DMZ segment
                |       +-- Security-lab segment
                |
                +-- Lightweight LXC test systems
                +-- External SSD backup storage
```

The household router remains the real edge router. OPNsense currently protects the isolated lab networks, not the entire house.

## Completed Foundation Work

### Proxmox backup and recovery

- Health-tested a dedicated external SSD.
- Created a permanent ext4-backed Proxmox backup datastore.
- Added scheduled backup jobs for infrastructure systems and the larger services VM.
- Backed up an LXC container.
- Restored the container under a temporary ID.
- Verified DHCP, routing, internet connectivity, and DNS after restoration.
- Removed the temporary recovery instance.
- Backed up the running OPNsense VM in snapshot mode.
- Preserved validation evidence outside the public repository.

### Carlvis network remediation

- Identified simultaneous static and DHCP addressing on the same interface.
- Backed up the Netplan configuration.
- Disabled the stale DHCP configuration.
- Retained one static address and one default route.
- Verified gateway, internet, DNS, Pi-hole, Uptime Kuma, Docker, and monitoring-service health.

### Docker and service inventory

- Inventoried running containers and Compose project locations.
- Inventoried host listening ports.
- Confirmed the host firewall currently allows inbound traffic by default.
- Identified backend and administrative services published to the entire home LAN.
- Moved hardcoded monitoring credentials into a protected environment file.
- Added repository ignore rules for secrets, private configurations, backups, and raw packet captures.

## Known Issues and Risks

### Must address before hardware integration

- Rotate the exposed n8n API key.
- Rotate the exposed PostgreSQL password after mapping every dependent application.
- Restrict n8n to loopback while preserving its Cloudflare Tunnel and internal monitoring access.
- Review and reduce direct LAN exposure for PostgreSQL, Qdrant, LiteLLM, Portainer, Prometheus, InfluxDB, cAdvisor, Node Exporter, Syncthing administration, and CUPS.
- Confirm the public n8n route presents authentication and does not expose the workflow editor anonymously.
- Confirm the first scheduled backup of the large Carlvis VM completes successfully.
- Confirm the external backup disk remounts correctly after a controlled Proxmox reboot.

### Important but not blocking the store trip

- Suricata starts and then exits under memory pressure; rule and service tuning is required.
- Proxmox package refreshes fail because the enterprise repository is enabled without a subscription.
- Thin-provisioned virtual disk totals exceed the physical thin pool; actual usage is still moderate but must be monitored.
- Several Docker images use floating `latest` tags and should later be pinned to tested versions.
- Cloudflared is behind the current release and uses `--no-autoupdate`.

## Pre-Micro Center Runbook

Complete these tasks before connecting new networking hardware.

### 1. Finish the n8n security change

- Rotate the n8n API key.
- Create a shared Docker network for n8n, Dashboard, and Homepage.
- Change internal monitoring calls to use the service name and container port.
- Bind the host port to `127.0.0.1` rather than every interface.
- Recreate only the affected containers.
- Verify the Cloudflare Tunnel, Dashboard, Homepage health check, and n8n login still work.
- Prove a separate LAN device can no longer connect directly to the n8n host port.

### 2. Map and rotate the database credential

- Identify every container or script using the PostgreSQL role.
- Update all consumers to read the password from a protected environment file.
- Rotate the PostgreSQL role password during one controlled maintenance window.
- Recreate or restart only dependent services.
- Verify application and database health.

### 3. Reduce unnecessary service exposure

Classify every listening service as one of:

- LAN-facing user service
- Localhost-only backend
- Tailscale-only administration
- Future management-VLAN service
- Unused and removable

Prioritize PostgreSQL, Qdrant, LiteLLM, Portainer, monitoring collectors, Syncthing GUI, and CUPS.

### 4. Validate operations

- Confirm Carlvis scheduled backup completion.
- Reboot Proxmox during a planned window.
- Verify the external backup datastore remounts and reports active.
- Verify OPNsense, Carlvis, Pi-hole, Uptime Kuma, and core monitoring services restart correctly.
- Save sanitized validation results.

### 5. Update documentation

- Current architecture diagram
- VM and container inventory
- Docker service inventory
- Known-issues list
- Backup and restore procedure
- Planned physical switch and VLAN topology

## Ready-to-Shop Exit Criteria

The lab is ready for the Micro Center trip when all of the following are true:

```text
[ ] Exposed n8n API key rotated
[ ] n8n no longer directly reachable from the home LAN
[ ] PostgreSQL credential rotated or a documented maintenance plan is ready
[ ] Most dangerous unnecessary LAN ports reduced
[ ] Carlvis backup completed successfully
[ ] External backup datastore survived a reboot
[ ] Current architecture and inventory are documented
[ ] Household router and real home devices remain unchanged
```

## Planned Hardware Integration

The initial hardware expansion is a **bench lab**, not an immediate household-network migration.

```text
Existing household router
        |
Proxmox built-in network interface
        |
OPNsense lab firewall
        |
Second Linux-compatible network interface
        |
Managed VLAN switch
        +-- Infrastructure/test port
        +-- DMZ test port
        +-- Security-lab test port
        +-- Raspberry Pi infrastructure node
        +-- Spare troubleshooting port
```

The first Raspberry Pi is planned as an independent infrastructure node for secondary filtered DNS, recursive DNS, Tailscale, monitoring, notifications, and configuration backups.

## After the Store Trip

1. Bench-configure switch management and export its configuration.
2. Test access and tagged ports with noncritical devices.
3. Connect the second mini-PC network interface to the switch.
4. Extend the virtual lab segments into physical VLANs.
5. Deploy the Raspberry Pi infrastructure node.
6. Prove routing, isolation, DNS, and rollback procedures.
7. Tune Suricata and generate a controlled alert.
8. Deploy Wazuh and onboard log sources.
9. Add Kali and one intentionally vulnerable target.
10. Complete one documented attack, detection, investigation, remediation, and retest exercise.

## Portfolio Direction

The public portfolio remains focused on support, NOC, cloud support, and security-adjacent work. Every case study should answer:

- What problem or risk existed?
- How was it investigated?
- What change was made?
- How was the result validated?
- Which job responsibility does the work demonstrate?
