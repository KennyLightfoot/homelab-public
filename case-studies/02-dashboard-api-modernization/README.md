# Case Study: Dashboard API Modernization and Safe Cutover

![Status](https://img.shields.io/badge/Status-In%20Progress-orange)
![Focus](https://img.shields.io/badge/Focus-Docker%20Application%20Operations-blue)

## Problem

A legacy Dockerized dashboard API was running successfully, but its deployment was difficult to reproduce and carried several operational risks:

- Application and database settings were mixed into deployment configuration.
- The runtime needed modernization.
- The relationship between the API, PostgreSQL, Homepage, monitoring, and other dependent services needed to be mapped before changes were made.
- A failed replacement could interrupt an otherwise healthy internal service.

The objective was not simply to rebuild a container. The objective was to create a controlled, testable replacement with a documented rollback path.

## Goal

Modernize the dashboard API to Node.js 24, externalize sensitive configuration, build a validated candidate image, and prepare a controlled production cutover without modifying the healthy live service until all prerequisites are satisfied.

## Environment

| Component | Role |
|---|---|
| Carlvis-Ubuntu VM | Docker application host |
| Docker Compose | Application deployment and service orchestration |
| Dashboard API | Internal backend application |
| PostgreSQL | Application database |
| Homepage | Internal dashboard consumer and health-check surface |
| cAdvisor and Grafana | Container visibility and post-change monitoring |

Private addresses, credentials, tunnel details, image fingerprints, and raw validation manifests are excluded from this public case study.

## Investigation

The existing deployment was reviewed before any replacement work began.

The investigation included:

- Locating the active Compose project and application source.
- Identifying the live container, image, port ownership, and dependent services.
- Mapping application and database environment variables.
- Separating actual secrets from non-secret runtime configuration.
- Preserving the working deployment and creating timestamped backups before promotion attempts.
- Confirming that the live service remained healthy while the replacement was staged.

## Implementation

1. Sanitized the application project and removed deployment-specific secret values.
2. Updated the application build to use Node.js 24.
3. Prepared a separate staging project instead of modifying the active project in place.
4. Built an immutable candidate image with a unique tag.
5. Created protected application and database environment-file boundaries outside the public project.
6. Validated the staged Compose configuration.
7. Tested the candidate image separately from the live service.
8. Created file checksums and image fingerprints for promotion validation.
9. Prepared a controlled promotion script with backups and rollback behavior.
10. Stopped the first promotion attempt when a permanent application environment file was not present.
11. Restored the active project and confirmed that the live service remained healthy.

## Safe Failure and Rollback

The first promotion attempt did not proceed because a required protected environment file had not yet been installed in its permanent location.

That failure was useful validation of the change process:

- The prerequisite check detected an incomplete deployment state.
- The active project was restored from its pre-promotion backup.
- The production container and host port remained unchanged.
- The already validated candidate image did not need to be rebuilt.
- The next attempt could resume from the validated staging artifacts rather than starting over.

This demonstrated that rollback planning was part of the implementation rather than an undocumented emergency response.

## Current State

The following work is complete:

- Legacy deployment investigation
- Dependency mapping
- Project sanitization
- Node.js 24 candidate build
- Staged Compose validation
- Candidate-container testing
- Checksum and image-fingerprint validation
- Pre-promotion backup creation
- Safe rollback from an incomplete promotion attempt

The following work remains:

- Install the permanent protected application environment file.
- Revalidate the active Compose configuration.
- Promote the validated staged project files.
- Perform the controlled production cutover.
- Verify application, PostgreSQL, Homepage, and monitoring behavior.
- Confirm that unnecessary host exposure is reduced.
- Preserve sanitized post-cutover evidence.

## Validation Strategy

The cutover is considered successful only when all of the following are true:

```text
[ ] The promoted project matches the validated staging checksums
[ ] The running container uses the expected candidate image
[ ] The application health endpoint responds successfully
[ ] PostgreSQL connectivity is confirmed
[ ] Homepage health checks continue to work
[ ] cAdvisor and Grafana show the replacement container
[ ] The old container and image are retained until rollback risk is acceptable
[ ] No secrets or private configuration are added to the public repository
```

## Skills Demonstrated

- Docker and Docker Compose operations
- Node.js runtime modernization
- PostgreSQL dependency analysis
- Secret externalization
- Immutable image tagging
- Staged deployment validation
- Change control and maintenance planning
- Backup-before-change discipline
- Health checks and service dependency testing
- Rollback design and recovery validation
- Sanitized technical documentation

## Interview Framing

> I inherited a working but difficult-to-reproduce Docker application deployment. Before changing it, I mapped its PostgreSQL and service dependencies, separated secrets from deployment configuration, modernized the runtime to Node.js 24, built and tested an immutable candidate image, and created a staged promotion process with checksums and rollback. When a required environment file was missing, the process stopped and restored the original project without interrupting the healthy service. The remaining step is the controlled production cutover and post-change validation.

## Lessons Learned

- A working service should not be changed in place when a separately validated candidate can be built.
- Promotion checks should fail closed when protected configuration is missing.
- A rollback process should be tested before the production cutover.
- Image tags, checksums, and container fingerprints provide stronger evidence than file names alone.
- Secret migration and application modernization are related but should be validated independently.
- Operational honesty is more valuable than marking a partially completed deployment as finished.

## Completion Criteria

This case study will be marked **Complete** after the production cutover, dependency validation, monitoring confirmation, and sanitized evidence update are finished.
