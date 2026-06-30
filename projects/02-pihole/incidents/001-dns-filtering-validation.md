# Incident 001: DNS Filtering Validation

## Symptom

A client needs DNS filtering validated to confirm that known ad/tracking domains are blocked while normal domains still resolve successfully.

## Scope

Affected service:

- Pi-hole DNS container running on Carlvis-Ubuntu
- DNS service listening on port 53
- Client DNS queries sent directly to Pi-hole

## Initial Theory

If Pi-hole is configured correctly, blocked domains should return a sinkhole response while allowed domains should return normal DNS records. If DNS resolution fails for all domains, possible causes include container failure, port conflict, upstream DNS failure, or incorrect client DNS configuration.

## Tests Performed

- Checked Pi-hole service status inside the Docker container
- Confirmed FTL was listening on port 53
- Confirmed Pi-hole blocking was enabled
- Tested a known blocked domain with `nslookup`
- Tested a known allowed domain with `nslookup`
- Reviewed the Pi-hole query log for live DNS activity
- Reviewed active blocklists and gravity database status

## Commands Used

```bash
docker exec pihole pihole status
nslookup doubleclick.net 192.168.1.234
nslookup google.com 192.168.1.234
```

## Root Cause

No outage was present. The validation confirmed that Pi-hole was functioning as intended: blocked domains were sinkholed and normal domains resolved successfully.

## Fix

No fix was required for the core DNS filtering service.

A separate blocklist maintenance item was identified: one configured adlist source was returning `410 Gone` and should be removed from Pi-hole's adlist configuration.

## Verification

Confirmed the following:

- Pi-hole DNS service was running
- Blocking was enabled
- Known blocked domains returned a blocked/sinkhole response
- Known allowed domains returned valid DNS records
- Query log showed active client DNS lookups

## Prevention / Follow-Up

- Remove dead or deprecated blocklist sources
- Rebuild gravity after removing invalid lists
- Periodically review query logs and blocklist health
- Document known-good DNS test commands for future troubleshooting
