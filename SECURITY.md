# Security Policy and Evidence Handling

This public repository documents CVE-2026-45362. Sangoma lists Switchvox 8.4 as the patched release.

## Documentation Corrections

Open a GitHub issue for a factual correction, broken link, affected-version detail, or other non-sensitive documentation problem.

## Sensitive Evidence

Do not post live SIP credentials, customer backups, unredacted logs, packet captures, call-detail records, account identifiers, or personal information in a public issue.

GitHub private vulnerability reporting is not currently enabled for this repository. Do not attach sensitive evidence to a public issue. Use the [Sensitive evidence contact request](https://github.com/wb6vpm/switchvox-svb-plaintext-sip-credentials/issues/new/choose) form without describing the evidence, or use an existing authorized private disclosure channel.

Questions or corrections concerning CISA's SSVC assessment should be sent to `vulnerability@cisa.dhs.gov` with `SSVC` in the subject line, as directed by [CISA's SSVC page](https://www.cisa.gov/resources-tools/resources/stakeholder-specific-vulnerability-categorization-ssvc).

## New Product Vulnerabilities

Report a new or distinct Switchvox vulnerability through [Sangoma's private vulnerability-reporting form](https://github.com/sangoma/security-switchvox/security/advisories/new). Do not use this repository as the first public disclosure channel for an unpatched issue.

The vendor advisory for this vulnerability is [GHSA-mfm3-g35x-c9w8](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8).

## Previously Created Backups

Upgrading does not protect `.svb` files created by affected versions. If the chain of custody for an older backup is uncertain, protect or remove the file and rotate every credential stored in it.
