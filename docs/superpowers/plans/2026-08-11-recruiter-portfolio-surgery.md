# Recruiter Portfolio Surgery Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Reframe the public homelab repository so recruiters see strong validated infrastructure/network/cloud-support evidence immediately, while technical reviewers retain detailed project depth.

**Architecture:** Keep the existing repository structure and project URLs stable. Concentrate structural changes in the root README, then make small consistency edits to project pages: remove self-rated difficulty badges, reduce hiring-meta language, and preserve truthful status/evidence.

**Tech Stack:** GitHub Markdown, Mermaid diagrams, GitHub repository branch workflow.

## Global Constraints

- Work only on `portfolio/recruiter-surgical-polish`; do not edit `main`.
- Preserve technical facts and project URLs.
- Do not invent metrics, incidents, technologies, outcomes, or professional experience.
- Do not expose credentials, private management details, customer data, raw captures, complete logs, or configuration exports.
- Keep incomplete projects explicitly incomplete.
- Security+ must be shown as completed.

---

### Task 1: Rebuild the root README hierarchy

**Files:**
- Modify: `README.md`

- [ ] Replace the recruiter-meta-heavy opening with a concise infrastructure/network/cloud support portfolio introduction.
- [ ] Put validated completed work first with Proxmox recovery and OPNsense segmentation as the two flagship examples.
- [ ] Add supporting completed evidence for Uptime Kuma and Pi-hole.
- [ ] Move Suricata and Grafana/Prometheus into a clearly separate active-investigations section.
- [ ] Remove the long list of future API/auth/webhook/SQL gaps from the homepage.
- [ ] Correct CompTIA Security+ to completed.
- [ ] Preserve publication/security boundary and LinkedIn connection information.

### Task 2: Remove self-rated difficulty badges

**Files:**
- Modify: `projects/01-opnsense-segmentation/README.md`
- Modify: `projects/02-pihole/README.md`
- Modify: `projects/03-uptime-kuma/README.md`
- Modify: `projects/04-grafana-stack/README.md`
- Modify: `projects/05-suricata-ids/README.md`

- [ ] Remove `Difficulty` badges without replacing them with Advanced/Expert labels.
- [ ] Preserve status badges and all technical implementation evidence.

### Task 3: Reduce hiring-meta language on project pages

**Files:**
- Modify: `case-studies/01-proxmox-backup-recovery/README.md`
- Modify: `projects/05-suricata-ids/README.md`
- Modify selected other project pages only where wording is explicitly recruiter-facing.

- [ ] Remove public `Interview Framing` sections.
- [ ] Rename `Support Engineering Relevance` to `Operational Relevance` where useful rather than deleting the evidence translation.
- [ ] Replace phrases such as `recruiter-facing evidence` with neutral terms such as `sanitized validation evidence` where they occur in edited files.
- [ ] Keep technical facts, troubleshooting sequence, completion criteria, and lessons learned intact.

### Task 4: Verify the branch

- [ ] Compare `main` to `portfolio/recruiter-surgical-polish` and inspect every changed file.
- [ ] Verify README links point to existing project paths.
- [ ] Search changed/current content for `Security+ — in progress`, `Difficulty`, `Interview Framing`, and unnecessary `recruiter-facing` wording.
- [ ] Confirm active investigations remain clearly incomplete.
- [ ] Confirm no claim converts homelab work into professional enterprise production experience.
- [ ] Report the branch and changes to the user without merging.