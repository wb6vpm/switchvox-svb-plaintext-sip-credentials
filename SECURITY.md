# Security Policy and Evidence Handling

This public repository documents CVE-2026-45362. Sangoma lists Switchvox 8.4 as the patched release.

## Documentation Corrections

Open a GitHub issue for a factual correction, broken link, affected-version detail, or other non-sensitive documentation problem.

## Sensitive Evidence

Do not post live SIP credentials, customer backups, unredacted logs, packet captures, call-detail records, account identifiers, or personal information in a public issue.

For sensitive evidence related to the incident documented here, use GitHub's private vulnerability-reporting option if it is available on the repository's Security tab. Otherwise, open a public issue containing only a non-sensitive description and request a private exchange method. Do not attach the evidence to that issue.

## New Product Vulnerabilities

Report a new or distinct Switchvox vulnerability privately to Sangoma. Do not use this repository as the first public disclosure channel for an unpatched issue.

The vendor advisory for this vulnerability is [GHSA-mfm3-g35x-c9w8](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8).

## Previously Created Backups

Upgrading does not protect `.svb` files created by affected versions. If the chain of custody for an older backup is uncertain, protect or remove the file and rotate every credential stored in it.
