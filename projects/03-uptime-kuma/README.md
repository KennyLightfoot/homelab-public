# Project 3: Uptime Kuma Monitoring

![Status](https://img.shields.io/badge/Status-Complete-success)

## Overview

Deployed Uptime Kuma as a containerized availability-monitoring service. The monitor set covers a public HTTPS endpoint, internal application services, and infrastructure hosts so network, transport, and application-level failures can be distinguished quickly.

---

## Goals

- Monitor availability of external websites
- Monitor availability of internal lab services
- Detect outages within 60 seconds
- Establish a baseline for service health before adding more infrastructure

---

## Tech Stack

| Component | Role |
|---|---|
| Docker | Container runtime |
| Uptime Kuma | Uptime monitoring |
| Ubuntu services VM | Docker host on a private network |

---

## Evidence Status

The original dashboard capture contained live service labels and management details, so it is being replaced rather than published as durable technical evidence.

A future sanitized capture will use generic monitor names and will demonstrate:

- HTTP or HTTPS application monitoring
- TCP service-listener monitoring
- ICMP host reachability
- Historical outage and recovery visibility

---

## Deployment

Uptime Kuma runs as a Docker container with persistent storage kept outside the container lifecycle:

```bash
docker run -d \
  --name uptime-kuma \
  --restart always \
  -p <monitoring-port>:3001 \
  -v uptime-kuma-data:/app/data \
  louislam/uptime-kuma:<tested-version>
```

The administrative interface is restricted to the private management environment.

> The public example uses placeholders and a tested image tag rather than publishing live management details or relying on a floating image tag.

---

## Monitors Configured

### HTTP and HTTPS

Application monitors perform full requests and validate response behavior. These are used for customer-facing endpoints and services where an open port alone would not prove the application is healthy.

### TCP

TCP monitors verify that a selected service is accepting connections. They are useful for narrowing a failure to the transport or service-listener layer before deeper application troubleshooting.

### ICMP

Reachability checks determine whether infrastructure hosts respond at the network layer. They help separate host or routing failures from application-specific incidents.

The public portfolio intentionally omits live hostnames, ports, administrative endpoints, and internal service inventories.

---

## Monitor Types Explained

Uptime Kuma supports several monitor types — choosing the right one matters:

**HTTP(s)** — Makes a full HTTP request and checks the response code. Best for external sites where you want to verify the web server is actually responding, not just that the port is open. Also checks SSL certificate expiration.

**TCP Port** — Opens a TCP connection to a host:port and checks it succeeds. Best for internal services over plain HTTP where SSL isn't involved. Lighter weight than a full HTTP check.

**Ping** — Sends ICMP echo requests. Best for verifying a host is reachable at the network layer, regardless of what services are running.

---

## Validation Evidence

The monitor set was validated for:

- Successful HTTP and HTTPS requests
- Successful TCP connection checks
- Successful network-layer reachability checks
- Historical outage and recovery visibility
- Persistent monitor configuration after container restart
- Clear differentiation between host, port, and application failures

Point-in-time uptime percentages are intentionally omitted because they change continuously and do not provide durable portfolio evidence.

---

## Key Concepts Learned

- **Monitor type selection** — HTTP(s), TCP Port, and Ping serve different purposes; picking the wrong one gives misleading results
- **Port mapping** — container and host ports can differ, allowing services to coexist without publishing unnecessary live port details
- **Persistent volumes** — mounting `/app/data` to the host ensures monitor configs and history survive container restarts
- **Monitoring independence** — a monitor hosted on the same failure domain as its targets may become unavailable during the same incident, so independent monitoring is preferable for critical systems

---

## Operational Relevance

This project provides evidence for:

- Selecting the correct test for the suspected failure layer
- Separating network, transport, and application availability
- Recognizing that an open port does not guarantee application health
- Preserving outage history for incident investigation
- Designing monitoring with failure-domain limitations in mind
- Documenting validation criteria and expected results clearly

---

**Completed:** June 2026
