# Incident 001: Prometheus Target Health Review

## Symptom

Prometheus target health review showed active scrape targets working correctly, but also revealed stale or broken scrape targets from older monitoring configuration.

## Scope

Affected monitoring components:

- Prometheus running on Carlvis-Ubuntu
- Grafana dashboards backed by Prometheus data
- Node Exporter host metrics
- cAdvisor container metrics
- Stale scrape targets from old or incorrect configuration entries

## Initial Theory

Possible causes included retired services, renamed containers, incorrect hostnames, old pfSense/exporter configuration, changed ports, or stale entries left in `prometheus.yml`.

## Tests Performed

- Reviewed Grafana dashboards for host and container metrics
- Checked Prometheus target health
- Confirmed active exporters were being scraped successfully
- Identified DOWN targets that did not match current services
- Compared current running services against Prometheus scrape configuration
- Documented stale scrape targets for cleanup

## Commands Used

```bash
docker ps
docker logs prometheus
curl http://<prometheus-host>:9090/targets
curl http://<node-exporter-host>:9100/metrics
curl http://<cadvisor-host>:8080/metrics
```

## Root Cause

The monitoring stack was successfully collecting metrics from active exporters, but the Prometheus configuration still included stale scrape targets from old or incorrect service entries.

## Fix

Stale scrape targets should be removed or corrected in `prometheus.yml` so Prometheus target health reflects the current lab environment.

Active monitoring targets should remain:

- cAdvisor
- Node Exporter
- Prometheus self-monitoring

## Verification

Confirmed the following:

- Grafana dashboards rendered host and container metrics
- Prometheus was scraping active exporters
- Node Exporter metrics were visible
- cAdvisor container metrics were visible
- Stale DOWN targets were identified as configuration cleanup items

## Prevention / Follow-Up

- Review Prometheus targets after adding, removing, or renaming services
- Keep `prometheus.yml` aligned with current Docker service names and ports
- Document monitoring changes when services are retired
- Add alerting for exporter availability after stale targets are cleaned up
