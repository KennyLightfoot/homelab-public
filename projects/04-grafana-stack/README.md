# Project 4: Grafana and Prometheus Monitoring

![Status](https://img.shields.io/badge/Status-Remediation%20in%20Progress-orange)

## Overview

Deployed a containerized observability stack using Prometheus, Node Exporter, cAdvisor, Grafana, and InfluxDB.

The stack collects host and container metrics, visualizes system behavior, and provides evidence for troubleshooting resource, availability, and service-dependency problems.

The core metrics pipeline is operational. The project remains in remediation while stale scrape targets are removed and custom alerting evidence is completed.

---

## Goals

- Collect host-level CPU, memory, filesystem, disk, load, and network metrics
- Collect resource metrics for Docker containers
- Store and query time-series data with Prometheus
- Visualize metrics with Grafana
- Distinguish exporter, scrape-target, query, and dashboard failures
- Build custom panels and alerts that support incident investigation

---

## Evidence Status

The original screenshots demonstrate that host and container metrics render successfully, but they rely primarily on imported community dashboards.

The project will return to complete status after adding validation evidence for:

- A custom Grafana panel built from a documented PromQL query
- A working alert rule triggered under a controlled test condition
- A closed troubleshooting ticket showing how metrics narrowed an incident
- A sanitized Prometheus targets view containing no stale jobs

---

## Architecture

```text
Ubuntu services VM
        |
        +-- Node Exporter
        |       |
        |       +-- Host CPU, memory, filesystem, disk,
        |           load, and network metrics
        |
        +-- cAdvisor
        |       |
        |       +-- Docker container resource metrics
        |
        +-- Prometheus
        |       |
        |       +-- Scrapes exporters
        |       +-- Stores time-series data
        |       +-- Evaluates PromQL queries
        |
        +-- Grafana
        |       |
        |       +-- Dashboards
        |       +-- Custom panels
        |       +-- Alert rules
        |
        +-- InfluxDB
                |
                +-- Separate time-series data source
```

Administrative endpoints, live hostnames, ports, and internal workload names are intentionally excluded from the public portfolio.

---

## Tech Stack

| Component | Role |
|---|---|
| Prometheus | Metrics collection, storage, and query engine |
| Node Exporter | Linux host metrics |
| cAdvisor | Docker container metrics |
| Grafana | Dashboards, visualization, and alerting |
| InfluxDB | Secondary time-series data source |
| Docker | Container runtime |

---

## How the Metrics Pipeline Works

### Exporters

Exporters translate operating-system or application state into the Prometheus exposition format.

- **Node Exporter** reads Linux statistics including CPU time, memory, filesystem capacity, disk I/O, load, and network activity.
- **cAdvisor** reads Docker runtime statistics and exposes labeled metrics for individual containers.

### Prometheus

Prometheus periodically requests each approved `/metrics` endpoint, parses the returned samples, and stores them in its time-series database.

Prometheus is responsible for:

- Scrape scheduling
- Target-health tracking
- Metric storage
- Label-based filtering
- PromQL query evaluation
- Alert-rule evaluation

### Grafana

Grafana connects to Prometheus as a data source and uses PromQL queries to construct panels and alerts.

Imported community dashboards were used initially to validate the pipeline. Custom queries and panels are now being added to demonstrate deeper operational understanding.

---

## Deployment

All components run as Docker containers on a private services VM.

Prometheus uses a `prometheus.yml` configuration file to define intentional scrape jobs and target endpoints.

Grafana, Prometheus, and related administrative interfaces are restricted to the private management environment.

---

## Dashboard Coverage

The initial dashboard set provides visibility into:

### Host metrics

- CPU utilization and load
- Available and used memory
- Filesystem capacity
- Disk activity
- Network traffic
- System uptime

### Container metrics

- CPU usage
- Memory consumption
- Filesystem activity
- Network activity
- Container lifecycle behavior

Point-in-time values and live workload names are intentionally omitted because they change continuously and expose unnecessary environment details.

---

## Prometheus Targets and Validation

The core pipeline was validated successfully:

- Node Exporter exposes host metrics
- cAdvisor exposes container metrics
- Prometheus scrapes both exporters
- Prometheus collects its own health metrics
- Grafana queries Prometheus successfully
- Dashboard panels render host and container data
- Metrics remain available across normal container restarts

A later review identified stale scrape jobs referencing retired exporters and incorrect service names.

The valid targets remained operational, but the project status was changed from complete to remediation in progress because a production-quality configuration should contain only intentional and healthy jobs.

The active remediation work is to:

1. Remove retired scrape jobs
2. Correct invalid service names
3. Reload the Prometheus configuration safely
4. Confirm every remaining target is intentional
5. Verify every target reports healthy
6. Preserve the investigation as a closed support ticket

---

## Key Concepts Learned

- **Pull-based collection** — Prometheus controls when targets are scraped rather than waiting for agents to push metrics
- **Exporter health versus target health** — a running exporter does not guarantee that Prometheus can resolve or reach it
- **Labels** — key-value labels such as job and instance support filtering and aggregation
- **PromQL** — queries select, aggregate, and transform metrics for dashboards and alerts
- **Configuration drift** — retired services and renamed containers can leave stale targets behind
- **Metrics versus logs** — metrics show system behavior and trends, while logs provide event-level detail
- **Cloud comparison** — managed observability platforms provide comparable collection, storage, visualization, and alerting outcomes while handling scaling, retention, integration, and availability differently

---

## Completion Criteria

This project returns to **Complete** when:

```text
[ ] Every configured Prometheus target is intentional
[ ] Every remaining target reports healthy
[ ] Stale exporters and incorrect service names are removed
[ ] One custom Grafana panel uses a documented PromQL query
[ ] One alert rule is triggered under a controlled test condition
[ ] The alert is acknowledged and resolved through a closed support ticket
[ ] Published screenshots are sanitized
```

---

## Operational Relevance

This project provides evidence for:

- Distinguishing collection failures from visualization failures
- Validating exporter and scrape-target health
- Investigating stale configuration and service-name problems
- Using labels and PromQL to narrow resource issues
- Creating alert thresholds tied to operational impact
- Verifying recovery after configuration changes
- Reporting incomplete work honestly instead of hiding unhealthy targets

---

**Initial deployment:** June 2026

**Current status:** Remediation and alert-validation work in progress
