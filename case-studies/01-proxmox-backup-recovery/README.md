# Case Study: Proxmox Backup and Recovery Validation

![Status](https://img.shields.io/badge/Status-Complete-success)
![Focus](https://img.shields.io/badge/Focus-Backup%20%26%20Recovery-blue)

## Problem

The homelab had only limited local backups stored on the same physical drive as the active virtual machines. There were no dependable scheduled jobs and no documented proof that a backup could actually be restored.

That created two risks:

- A host-drive failure could remove both the live systems and their local backup files.
- A backup could appear successful without being usable during recovery.

## Goal

Create physically separate Proxmox backup storage, schedule infrastructure backups, and complete a real restore test that validates the recovered system from the operating system through network services.

## Environment

| Component | Role |
|---|---|
| Proxmox VE host | Hypervisor and backup scheduler |
| External 2 TB SSD | Physically separate backup datastore |
| OPNsense VM | Lab firewall and routing validation target |
| Debian LXC container | Small restore-test workload |

## Implementation

1. Connected the external SSD through a USB enclosure.
2. Inspected the existing partitions and old data read-only.
3. Ran SMART health checks, including an extended self-test.
4. Erased the retired operating-system installation after confirming the data was no longer needed.
5. Created a new ext4 filesystem and permanent mount.
6. Registered the mount as Proxmox directory storage restricted to backup content.
7. Enabled mount-point checking so Proxmox would not silently write to the host root filesystem if the external disk were absent.
8. Created separate backup schedules for core infrastructure and the larger services VM.

## Recovery Test

A stopped LXC workload was backed up with Zstandard compression and restored under a temporary container ID.

The restored instance was validated for:

- Successful boot
- Restored Proxmox configuration
- DHCP address assignment
- Default gateway installation
- Reachability to the OPNsense LAN interface
- Internet connectivity
- DNS resolution

After successful validation, the temporary recovery instance was stopped and removed while the original workload and backup archive were preserved.

## Running VM Snapshot Test

The running OPNsense firewall VM was backed up in snapshot mode. The VM resumed immediately and remained operational after the backup completed.

## Validation Results

| Test | Result |
|---|---|
| External SSD health | Extended SMART test completed without error |
| Proxmox storage status | Active |
| LXC backup | Successful |
| LXC restore | Successful |
| DHCP after restore | Successful |
| Gateway test | Successful |
| Internet test | Successful |
| DNS test | Successful |
| Running OPNsense snapshot | Successful |
| Temporary restore cleanup | Successful |

## Skills Demonstrated

- Proxmox VE storage administration
- Linux block-device and filesystem inspection
- SMART drive-health testing
- Backup scheduling and retention
- LXC backup and restore
- QEMU snapshot backup
- DHCP, routing, and DNS validation
- Disaster-recovery testing
- Evidence collection and operational documentation

## Operational Relevance

This case study provides evidence for:

- Identifying operational risk before an outage occurs
- Distinguishing backup completion from proven recoverability
- Planning a controlled recovery test
- Validating operating-system, network, and service behavior after restoration
- Preserving the original workload while testing under a temporary identity
- Documenting verification results and cleanup steps
- Communicating recovery readiness clearly to stakeholders

---

## Lessons Learned

- A completed backup job is not proof of recoverability.
- Recovery tests should validate application and network behavior, not only whether a guest boots.
- Backup storage should be physically separate from active VM storage.
- Mount-point validation helps prevent failed external storage from redirecting backups onto the hypervisor root disk.
- Larger workloads should use different schedules and retention policies than small infrastructure guests.

## Follow-up Work

- Confirm scheduled backups after a full maintenance cycle.
- Validate that the backup datastore remounts after a controlled host reboot.
- Add automated failure notifications.
- Periodically repeat restore testing with a documented recovery objective.
