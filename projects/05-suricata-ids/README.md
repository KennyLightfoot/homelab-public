# Project 5: Suricata IDS

![Status](https://img.shields.io/badge/Status-Stability%20Remediation-orange)
![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)

## Overview

Suricata was deployed as a passive network intrusion detection system on OPNsense. It uses PCAP mode to inspect traffic without dropping packets, with Emerging Threats Open and abuse.ch rules providing signatures and threat-intelligence coverage.

The deployment successfully produced alerts and EVE JSON output, but later operational testing revealed that the service starts and then exits under memory pressure. The project is therefore **deployed but not currently considered stable**.

This page now documents both the successful deployment and the active remediation work.

---

## Goals

- Deploy passive IDS monitoring on the lab firewall
- Understand IDS versus IPS operation
- Load selected community rulesets
- Produce structured EVE JSON output for future SIEM ingestion
- Establish a repeatable process for diagnosing and tuning an unstable security service

---

## Architecture

```text
Internet / upstream network
        |
        v
OPNsense WAN interface
        |
        +-- Suricata in passive PCAP mode
        |       |
        |       +-- alerts and EVE JSON logs
        |
        +-- LAN segment
        +-- DMZ segment
        +-- Security-lab segment
```

Suricata observes traffic at the OPNsense WAN interface. The firewall still handles packet forwarding and policy enforcement; Suricata detects and logs activity without blocking it.

---

## Tech Stack

| Component | Role |
|---|---|
| OPNsense | Router and firewall host |
| Suricata | Network IDS engine |
| Emerging Threats Open | Community detection rules |
| abuse.ch feeds | Malware and command-and-control intelligence |
| EVE JSON | Structured event output for SIEM integration |

---

## IDS vs IPS Decision

**IDS mode** was selected deliberately.

- IDS observes traffic and generates alerts.
- IPS runs inline and may block traffic.

The lab begins with passive detection so rules can be understood and tuned before any security tool is allowed to drop legitimate traffic.

---

## Initial Deployment Results

The original deployment validated:

- PCAP live mode on the selected interface
- Downloaded and enabled rulesets
- Live alert generation
- EVE JSON event output
- OPNsense integration and service controls

Selected rules focused on scanning, exploitation, malware, botnet activity, command-and-control infrastructure, shellcode, and malicious URLs.

## Evidence Status

The original administrative screenshots were removed from the public portfolio because they exposed unnecessary configuration details and did not clearly demonstrate the current remediation state.

Replacement recruiter-facing evidence will include:

- A controlled benign alert
- The matching event in EVE JSON
- Service status after an extended stability test
- Memory and process checks confirming the service remains healthy
- Sanitized output with management details removed

---

## Operational Issue Discovered

Later review showed that Suricata was not remaining active.

Observed behavior:

1. The start command returned successfully.
2. The Suricata process launched.
3. System logs reported that the process was killed after memory could not be reclaimed.
4. A related Python process was also terminated during the same pressure event.
5. Increasing the OPNsense VM from 2 GB to 4 GB did not fully resolve the failure.

Because of this evidence, the service is intentionally left stopped until tuning is completed.

---

## Remediation Performed

- Created a fresh Proxmox snapshot backup of the running OPNsense VM.
- Confirmed disk capacity was not the immediate constraint.
- Confirmed there was no configured swap inside OPNsense.
- Increased OPNsense memory from 2 GB to 4 GB.
- Restarted the service and collected new failure evidence.
- Chose not to blindly allocate more memory before reviewing the overall lab resource plan.

This changed the project from a simple deployment exercise into an operational troubleshooting case study.

---

## Current Remediation Plan

1. Review enabled rules and remove low-value or unnecessarily broad categories.
2. Record the loaded rule count and startup memory behavior.
3. Confirm the intended capture interface and logging configuration.
4. Start Suricata and validate that it remains active for an extended period.
5. Generate one controlled benign test alert.
6. Locate the alert in OPNsense and EVE JSON.
7. Recheck memory consumption and system logs.
8. Document the stable configuration before integrating Wazuh.

A dedicated Suricata or Zeek sensor VM may be evaluated later, but only after the simpler firewall-hosted design is understood and tuned.

---

## Validation Commands

Check status:

```sh
configctl ids status
```

Check for a running process:

```sh
pgrep -laf suricata
```

Review recent service logs:

```sh
tail -n 100 /var/log/suricata/suricata_*.log
```

Search system logs for memory-related termination:

```sh
grep -nE 'suricata|failed to reclaim memory|killed' /var/log/system/* | tail -n 50
```

---

## Skills Demonstrated

- IDS architecture and passive packet inspection
- Signature and ruleset selection
- EVE JSON logging
- OPNsense service administration
- FreeBSD command-line troubleshooting
- Resource-capacity analysis
- Log-based root-cause investigation
- Change control and backup-before-remediation discipline
- Honest operational status reporting

---

## Support Engineering Relevance

This project provides evidence for:

- Investigating a service that starts successfully but fails shortly afterward
- Using process state and system logs instead of relying only on a successful start command
- Separating disk, memory, configuration, and application-level hypotheses
- Backing up a production-like system before remediation
- Retesting after a resource change rather than assuming the change solved the problem
- Maintaining honest service status and explicit completion criteria
- Converting technical evidence into a concise customer or stakeholder explanation

---

## Interview Framing

> I deployed Suricata successfully and validated alerts and structured logs, but later discovered that the process was being terminated under memory pressure. I backed up the firewall, collected system evidence, increased resources, retested, and decided to tune the rules and architecture rather than hiding the failure or continuously adding RAM. The project is now being completed as a stability and capacity-planning exercise.

---

## Completion Criteria

This project returns to **Complete** when:

```text
[ ] Suricata remains running reliably
[ ] Rulesets are intentionally selected and documented
[ ] One controlled test alert is generated
[ ] The alert is found in the GUI and EVE JSON
[ ] Memory and log checks show no repeated process termination
[ ] The final configuration is ready for Wazuh ingestion
```
