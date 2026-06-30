# Incident 001: Monitoring Alert Triage Workflow

## Symptom

A monitored service appears down, degraded, or has historical outage events visible in the Uptime Kuma dashboard.

## Scope

Affected monitoring environment:

- Uptime Kuma running in Docker on Carlvis-Ubuntu
- External HTTP(s) monitor for a public website
- Internal TCP port monitors for lab services
- Ping monitors for core infrastructure hosts

## Initial Theory

Possible causes include a stopped service container, closed port, failed host, network connectivity issue, incorrect monitor type, DNS issue, or temporary application outage.

## Tests Performed

- Reviewed the Uptime Kuma dashboard for affected monitors
- Confirmed whether the issue affected one service or multiple services
- Checked monitor type: HTTP(s), TCP Port, or Ping
- Verified whether the target host was reachable
- Verified whether the target port was accepting connections
- Checked whether the related Docker container or service was running
- Compared current status against historical uptime events

## Commands Used

```bash
docker ps
docker logs <container-name>
curl -I http://<service-host>:<port>
nc -vz <service-host> <port>
ping <host>
```

## Root Cause

No active outage was present at the time of documentation. The dashboard confirmed that monitoring was active and historical outage visibility was preserved for future troubleshooting.

In a live incident, root cause would be determined by checking service status, container logs, port availability, host reachability, and monitor configuration.

## Fix

No fix was required during this validation.

For a live outage, the fix would depend on the confirmed cause, such as restarting a failed container, correcting a port mapping, fixing service configuration, or resolving a host/network issue.

## Verification

Confirmed the following:

- Uptime Kuma was running
- 9 monitors were configured
- HTTP(s), TCP Port, and Ping monitor types were in use
- Internal services and infrastructure hosts were visible on the dashboard
- Historical outage information remained available for troubleshooting context

## Prevention / Follow-Up

- Add notification channels so outages generate alerts automatically
- Document common checks for each monitored service
- Add incident notes when future outages occur
- Consider external monitoring for public services so outages can still be detected if the home lab is down
