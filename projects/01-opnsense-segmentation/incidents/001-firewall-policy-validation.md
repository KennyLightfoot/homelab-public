# Incident 001: Firewall Policy Validation

## Symptom

A host on the DMZ or Lab network could reach the internet but could not reach higher-trust internal networks.

## Scope

Affected traffic paths:

- DMZ to LAN
- Lab to LAN
- Lab to DMZ

## Initial Theory

Possible causes included incorrect IP configuration, missing default gateway, routing issues, missing firewall rules, or intentionally blocked traffic due to segmentation policy.

## Tests Performed

- Checked container IP configuration
- Verified default gateway behavior
- Tested internet connectivity
- Tested cross-segment connectivity
- Reviewed OPNsense interface assignments
- Reviewed firewall rules
- Compared expected traffic policy against actual behavior

## Root Cause

The firewall was blocking traffic from lower-trust segments to higher-trust segments as intended.

## Fix

No fix was required. The blocked traffic matched the intended segmentation policy.

## Verification

Confirmed the following:

- DMZ could reach the internet
- Lab could reach the internet
- DMZ could not reach LAN
- Lab could not reach LAN
- Lab could not reach DMZ

## Prevention / Follow-Up

Documented expected firewall behavior and validation tests so future firewall changes can be checked against the intended policy.
