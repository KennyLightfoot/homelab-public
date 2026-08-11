# Recruiter Portfolio Surgery Design

## Goal

Make the public homelab portfolio effective for a recruiter spending roughly 60 seconds on the repository while preserving enough technical depth for an infrastructure, network, cloud, or support hiring manager to validate the work one click deeper.

## Positioning

Primary positioning: **Infrastructure / Network / Cloud Support**.

Supporting positioning: customer-facing technical support experience combined with hands-on infrastructure troubleshooting.

Do not position Kenneth as an experienced Cloud Engineer, DevOps Engineer, SRE, Security Engineer, or Senior Infrastructure Engineer.

## Homepage design

The root README is recruiter-first. It should quickly communicate:

1. who Kenneth is technically;
2. the roles the portfolio supports;
3. the strongest completed evidence;
4. the technical capabilities demonstrated; and
5. where a technical reviewer should click next.

Completed and validated projects appear before active investigations. The homepage should not lead with a list of skills Kenneth is still developing.

## Evidence hierarchy

### Featured validated work

1. Proxmox Backup and Recovery Validation
2. OPNsense Network Segmentation and Firewall Validation
3. Uptime Kuma Monitoring or Pi-hole DNS Filtering as supporting completed evidence

### Active investigation

Suricata Stability Remediation is retained because it demonstrates log-based troubleshooting, controlled change, retesting, and honest status reporting.

Grafana/Prometheus remains visible as active remediation but should not visually compete with completed flagship work.

## Copy rules

- Prefer evidence over claims.
- Remove repeated hiring-meta language where the technical work already proves the point.
- Remove self-assigned difficulty badges rather than replacing them with inflated ratings.
- Keep truthful project status labels.
- Keep useful operational relevance sections, but rename or trim them when they read like interview coaching.
- Remove explicit interview-framing blocks from public project pages.
- Correct Security+ from in-progress to completed everywhere touched by this change.
- Keep future work subordinate to current evidence.

## Security and accuracy

Preserve SECURITY.md and the publication checklist. Do not publish credentials, customer/payment data, exact management endpoints, raw captures, full logs, configuration backups, or sensitive screenshots. Do not rewrite Git history as part of this change.

Claims must distinguish homelab evidence from professional production-enterprise experience. Unfinished work must remain unfinished.

## Git workflow

All changes are made on `portfolio/recruiter-surgical-polish`. `main` remains untouched until explicit user approval after diff review.