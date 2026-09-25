# AWS monitoring server — WGU coursework

I built a monitoring server in an AWS coursework environment to practice cloud deployment, restricted administrative access, and CPU/disk monitoring. The project combined an Ubuntu EC2 instance with Prometheus, Grafana, and node_exporter.

## Work completed

| Area | Implementation |
|---|---|
| Compute | EC2 running Ubuntu 24.04 |
| Storage | Encrypted EBS volume |
| Access | Security group limited to one administrative IP |
| Monitoring | Prometheus, Grafana, and node_exporter deployed on the server |
| Rules | Wrote and validated CPU and disk-space alert rules |

## Validation and limits

The recorded coursework result is validation of the CPU and disk-space alert rules. Rule validation is separate from notification delivery: this project does not establish an alert-routing or notification-delivery result.

This is a retrospective summary of WGU coursework, not production cloud administration or a claim that an EC2 instance remains running today. Raw lab output, screenshots, rule thresholds, and configuration files are not included in this public summary.

**Documentation date: September 24, 2026.** This is the write-up date, not a new cloud deployment or test date.

## Skills demonstrated

- Deploying a Linux monitoring server on EC2.
- Applying storage encryption and limiting administrative access.
- Writing and validating resource-monitoring rules.

[Back to portfolio](../../README.md)
