# Public Portfolio Publication Checklist

Use this checklist before publishing or updating any recruiter-facing artifact.

## Security and privacy

- [ ] No passwords, API keys, tokens, cookies, private keys, recovery codes, connection strings, or unredacted environment files
- [ ] No customer, payment, personal, or business-confidential data
- [ ] No exact management addresses, administrative URLs, tunnel identifiers, account IDs, or unnecessary service inventories
- [ ] No raw packet captures, HAR files, complete logs, database exports, firewall exports, or configuration backups
- [ ] No screenshots showing browser address bars, unrelated tabs, private labels, account identifiers, QR codes, or unresolved operational weaknesses
- [ ] Commands and output use placeholders where exact values are not essential
- [ ] Any credential that may have been published has been rotated or revoked; deletion alone is not treated as remediation

## Technical accuracy

- [ ] Status labels match the current verified state
- [ ] Counts, versions, diagrams, and validation results agree with the text
- [ ] Claims distinguish deployment from successful long-running operation
- [ ] Cloud or enterprise comparisons are described as conceptual parallels, not exact equivalents
- [ ] Each result can be supported by a reproducible test or sanitized evidence
- [ ] Planned work is clearly separated from completed work

## Recruiter value

- [ ] The page begins with the problem, impact, or risk—not a tool list
- [ ] Investigation steps explain what each test was intended to prove
- [ ] The resolution or current remediation state is explicit
- [ ] Verification shows how recovery or expected behavior was confirmed
- [ ] The write-up identifies the Support Engineer competency demonstrated
- [ ] The page can be understood without access to the private environment
- [ ] Screenshots add evidence rather than decoration

## Final review

- [ ] Rendered Markdown and links were checked on GitHub
- [ ] Image names and captions match what the images actually show
- [ ] A current-file secret scan was run
- [ ] Git history was considered when sensitive content was removed
- [ ] The public repository remains a portfolio, not a live CMDB or operations runbook
