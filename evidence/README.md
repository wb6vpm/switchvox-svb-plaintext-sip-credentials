# Evidence Manifest

This directory indexes evidence relevant to CVE-2026-45362 and the incident that led to its discovery. It does not contain production credentials, customer backups, or unredacted customer records.

## Public Record

| Item | Status | Location |
| --- | --- | --- |
| CVE record | Public | [CVE-2026-45362](https://www.cve.org/CVERecord?id=CVE-2026-45362) |
| Sangoma advisory | Public | [GHSA-mfm3-g35x-c9w8](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8) |
| Switchvox 8.4 release notes | Public | [Sangoma documentation](https://sangomakb.atlassian.net/wiki/spaces/Switchvox/pages/1521647617) |
| Synthetic credential reproduction | Public | [Technical Analysis](../docs/technical-analysis.md#synthetic-test-case) |
| Sanitized incident account | Public | [Observed Production Incident](../docs/observed-incident.md) |
| Disclosure chronology | Public | [Disclosure Timeline](../docs/disclosure-timeline.md) |

## Incident Evidence Inventory

| Artifact | Evidentiary purpose | Status / next action |
| --- | --- | --- |
| Carrier fraud alerts and support correspondence | Establishes fraud reports, affected account scope, and provider response | Locate retained copies, preserve originals, and complete redaction review before publication |
| Carrier call-detail records | Establishes unauthorized destinations, timing, duration, and subaccounts | Locate retained exports or request records from the carrier if necessary |
| SIP authentication records | Establishes use of valid carrier credentials and source details | Locate retained copies or request records from the carrier if necessary |
| Credential-rotation records | Establishes when replacement credentials became active | Locate retained records and correlate them with the incident timeline |
| Switchvox backup audit entries | Establishes backup creation and download after rotation | Observed during incident response; original raw logs not presently located |
| Service-suspension correspondence | Establishes the scope and approximate 48-hour operational disruption | Locate retained copies, preserve originals, and complete redaction review before publication |
| Test backups from affected versions | Reproduces plaintext credential storage | Retain or regenerate privately; publish only synthetic extracts or hashes |
| Switchvox 8.4 test backup | Confirms changed backup format after remediation | Retain or regenerate privately; publish only non-sensitive format observations or hashes |

## Evidence Handling

Before sharing an artifact publicly:

1. Remove live credentials, authentication tokens, customer names, complete account identifiers, telephone numbers, public IP addresses tied to customers, and unrelated personal information.
2. Preserve an unmodified private copy.
3. Record the file hash, source, collection date, and redaction method.
4. Publish a sanitized excerpt only when it adds information not already documented.
5. Use a private submission channel for CISA, MITRE, Sangoma, the carrier, or another authorized reviewer when full evidence is necessary.

Do not commit an unredacted `.svb` backup, packet capture, provider export, or support bundle to this repository.
