# Security and Responsible Disclosure

This repository is a sanitized technical portfolio. It is not the source of truth for live infrastructure, credentials, customer data, or production configuration.

## Publication boundary

The public repository must not contain:

- passwords, API keys, tokens, cookies, private keys, recovery codes, or connection strings
- customer, payment, personal, or business-confidential data
- exact private management addresses, administrative URLs, tunnel identifiers, or unnecessary service inventories
- raw packet captures, HAR files, complete logs, database exports, firewall exports, or unredacted configuration backups
- screenshots showing browser address bars, unrelated tabs, private labels, account identifiers, or operational weaknesses that have not been remediated

Representative diagrams, generalized labels, and sanitized command output may be used when they are necessary to explain the technical lesson.

## Reporting a suspected exposure

Do not open a public issue containing a suspected credential or sensitive value. Contact the repository owner privately through the LinkedIn profile listed in the root README and include only the affected path and a high-level description.

## Response process

When a suspected exposure is confirmed:

1. Rotate or revoke the affected credential immediately.
2. Remove the value from the current branch.
3. Review dependent systems and access logs.
4. Determine whether Git history contains the value.
5. Rewrite history only when necessary, then invalidate old clones and verify the replacement state.
6. Document the incident privately and publish only a sanitized retrospective if it provides portfolio value.

Deleting a file does not make a previously published secret safe. Rotation is required whenever a real credential may have been exposed.