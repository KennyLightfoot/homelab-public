# Project 3: Uptime Kuma Monitoring

![Status](https://img.shields.io/badge/Status-Complete-success)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green)

## Overview

Deployed Uptime Kuma as a self-hosted uptime monitoring solution running in Docker on the Carlvis VM. Monitors cover both external public-facing websites and internal Carlvis services, giving a single dashboard view of the lab's health.

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
| Carlvis-Ubuntu (VM 100) | Host — `192.168.1.234` |

---

## Screenshots

![Uptime Kuma Dashboard](./screenshots/dashboard.png)
*Dashboard showing 9 active monitors — all currently up, with historical outage events still visible*

---

## Deployment

Uptime Kuma runs as a Docker container on Carlvis with persistent storage mounted to the host:

```bash
docker run -d \
  --name uptime-kuma \
  --restart always \
  -p 3002:3001 \
  -v /home/big-dog/uptime-kuma-data:/app/data \
  louislam/uptime-kuma:latest
```

**Access:** `http://192.168.1.234:3002`

> Port 3002 is used on the host (instead of the default 3001) to avoid conflicts with other services.

---

## Monitors Configured

### External Websites — HTTP(s)

| Name | URL | Interval |
|---|---|---|
| Public website | Redacted public HTTPS endpoint | 60s |

### Internal Services — TCP Port

TCP Port monitors check that the service port is accepting connections. Used for internal HTTP services since they don't require SSL validation.

| Name | Host | Port | Interval |
|---|---|---|---|
| Grafana | 192.168.1.234 | 3000 | 60s |
| Pi-hole | 192.168.1.234 | 8080 | 60s |
| Portainer | 192.168.1.234 | 9000 | 60s |
| n8n | 192.168.1.234 | 5678 | 60s |
| Open WebUI | 192.168.1.234 | 3010 | 60s |
| Homepage | 192.168.1.234 | 8082 | 60s |

### Infrastructure — Ping

| Name | Host | Interval |
|---|---|---|
| Carlvis VM | 192.168.1.234 | 60s |
| Proxmox Host | 192.168.1.50 | 60s |

---

## Monitor Types Explained

Uptime Kuma supports several monitor types — choosing the right one matters:

**HTTP(s)** — Makes a full HTTP request and checks the response code. Best for external sites where you want to verify the web server is actually responding, not just that the port is open. Also checks SSL certificate expiration.

**TCP Port** — Opens a TCP connection to a host:port and checks it succeeds. Best for internal services over plain HTTP where SSL isn't involved. Lighter weight than a full HTTP check.

**Ping** — Sends ICMP echo requests. Best for verifying a host is reachable at the network layer, regardless of what services are running.

---

## Results

All 9 monitors were up at the time of capture. Uptime percentages reflect available history, so earlier outages remain visible even after a service recovers.

| Monitor | Status | Uptime (30-day) |
|---|---|---|
| Carlvis VM | ✅ Up | 100% |
| Grafana | ✅ Up | 100% |
| Homepage | ✅ Up | 100% |
| Public website | ✅ Up | 99.93% |
| n8n | ✅ Up | 100% |
| Open WebUI | ✅ Up | 56.69% |
| Pi-hole | ✅ Up | 100% |
| Portainer | ✅ Up | 100% |
| Proxmox Host | ✅ Up | 100% |

---

## Key Concepts Learned

- **Monitor type selection** — HTTP(s), TCP Port, and Ping serve different purposes; picking the wrong one gives misleading results
- **Port mapping** — Uptime Kuma runs on container port 3001 but is exposed on host port 3002, avoiding conflicts with other services
- **Persistent volumes** — mounting `/app/data` to the host ensures monitor configs and history survive container restarts
- **Self-hosted vs cloud monitoring** — running your own monitoring means no external dependency, but it also means the monitor goes down if the host goes down (a limitation to solve with future redundancy)

---

## What's Next

- Set up notification channels (email or webhook) so outages alert without checking the dashboard
- Add a public Status Page for external services
- As more lab infrastructure is added (Wazuh, Suricata), add monitors here first

---

**Completed:** June 2026
