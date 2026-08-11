# Project 2: Pi-hole DNS

![Status](https://img.shields.io/badge/Status-Complete-success)

## Overview

Deployed Pi-hole as a containerized DNS filtering service. It evaluates DNS requests against curated blocklists and prevents clients from resolving selected advertising, tracking, and malicious domains before an application establishes a connection.

---

## Goals

- Block ads and tracking domains network-wide at the DNS layer
- Add threat intelligence feeds (crypto mining, malicious domains)
- Understand how DNS works as a security control point
- Practice Docker deployment and persistent container configuration

---

## Tech Stack

| Component | Version | Role |
|---|---|---|
| Docker | — | Container runtime |
| Pi-hole | v6.4 | DNS server / ad blocker |
| Ubuntu services VM | — | Docker host on a private network |

---

## Evidence Status

The original administrative screenshots were removed because they exposed private management details and did not provide durable troubleshooting evidence.

Replacement sanitized evidence will demonstrate:

- Pi-hole service status
- One allowed DNS lookup
- One controlled blocked lookup
- Gravity database rebuild success
- Sanitized output with client and management details removed

---

## How Pi-hole Works

Every device on a network needs DNS to browse the internet — it translates human-readable names like `google.com` into IP addresses. Normally your router handles this by forwarding queries to your ISP or a public resolver like `8.8.8.8`.

Clients are configured to use Pi-hole as their DNS resolver. Pi-hole checks each query against its local policy database before either forwarding the request to an upstream resolver or returning a blocking response.

This lab demonstrates the transferable concepts behind DNS-layer filtering and policy enforcement. Managed enterprise platforms add centralized identity, reporting, threat intelligence, policy distribution, and high-availability capabilities beyond this self-hosted implementation.

---

## Deployment

Pi-hole runs as a Docker container with persistent configuration and data mounted outside the container lifecycle:

```bash
docker run -d \
  --name pihole \
  --restart always \
  -p 53:53/tcp \
  -p 53:53/udp \
  -p <admin-port>:80 \
  -e TZ=<local-timezone> \
  -v pihole-config:/etc/pihole \
  -v pihole-dnsmasq:/etc/dnsmasq.d \
  pihole/pihole:<tested-version>
```

The administrative interface is restricted to the private management environment. DNS is exposed only where required for approved clients.

> The public example uses placeholders and a tested image tag rather than publishing live management details or relying on the floating `latest` tag.

---

## Blocklist Management

Pi-hole uses **gravity**, a local database produced by downloading, validating, and merging configured domain lists. The database is rebuilt with `pihole -g`.

The active sources were selected to cover advertising, tracking, cryptocurrency-mining infrastructure, and known malicious domains. Source health is reviewed periodically because third-party lists may be retired, moved, or return errors.

Validation includes:

- Confirming each source downloads successfully
- Removing unavailable or redundant sources
- Rebuilding the gravity database
- Reviewing the resulting unique-domain count
- Testing both blocked and allowed lookups
- Allowlisting legitimate services when false positives occur

---

## Verification

Confirm Pi-hole is listening and blocking:

```bash
# Check service status
docker exec pihole pihole status

# Expected output:
# [✓] FTL is listening on port 53
# [✓] Pi-hole blocking is enabled
```

### Test a blocked domain from a client

```bash
# Should return 0.0.0.0 (blocked) instead of a real IP
nslookup doubleclick.net <pihole-dns-ip>

# Should return a real IP (not blocked)
nslookup google.com <pihole-dns-ip>
```

---

## Key Concepts Learned

- **DNS as a security layer** — blocking at DNS is efficient because it happens before any data is transferred; the connection is never made
- **Gravity database** — Pi-hole merges multiple blocklists into a single SQLite database for fast lookups at query time
- **Sinkholing** — returning a controlled blocking response instead of the requested destination prevents the client from connecting to the original domain
- **Allowlisting** — some legitimate services get caught by blocklists; Pi-hole supports exact domain, wildcard, and regex allowlisting
- **Cloud comparison** — managed DNS firewalls provide comparable policy outcomes while adding cloud-native integration, centralized governance, scaling, and managed availability

---

## Operational Relevance

This project provides evidence for:

- Separating DNS-resolution failures from broader network failures
- Comparing expected and observed resolver behavior
- Validating service state from both the container and client perspectives
- Investigating false positives and applying controlled allowlisting
- Managing persistent Docker configuration
- Documenting reproducible verification commands and expected results

---

**Completed:** June 2026
