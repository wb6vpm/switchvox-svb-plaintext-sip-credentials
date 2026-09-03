# Evidence Manifest

This directory indexes evidence relevant to CVE-2026-45362 and the incident that led to its discovery. It does not contain production credentials, customer backups, or unredacted customer records.

## Public Record

| Item | Status | Location |
| --- | --- | --- |
| CVE record | Public | [CVE-2026-45362](https://www.cve.org/CVERecord?id=CVE-2026-45362) |
| Sangoma advisory | Public | [GHSA-mfm3-g35x-c9w8](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8) |
| Switchvox 8.4 release notes | Public | [Sangoma documentation](https://sangomakb.atlassian.net/wiki/spaces/Switchvox/pages/1521647617) |
| Synthetic test narrative | Public | [Technical Analysis](../docs/technical-analysis.md#synthetic-test-case) |
| Sanitized incident narrative | Public | [Observed Production Incident](../docs/observed-incident.md) |
| Disclosure chronology | Public | [Disclosure Timeline](../docs/disclosure-timeline.md) |

## Incident Evidence Inventory

| Artifact | Evidentiary purpose | Status / next action |
| --- | --- | --- |
| Carrier fraud alerts and support correspondence | Establishes fraud reports, affected account scope, repeated traffic notices, and provider response | Recovered in retained mailbox threads. Preserve source exports and prepare only minimized or redacted copies for external review. |
| Carrier call-detail records | Establishes unauthorized destinations, timing, duration, and subaccounts | Partial. Dated call examples and an Industry Traceback Group request are retained in carrier correspondence. Request a complete carrier export if required. |
| SIP authentication records | Establishes use of valid carrier credentials and source details | No standalone raw export has been recovered. Request the carrier's retained authentication and registration records if required. |
| Credential-rotation records | Establishes when replacement credentials became active | Partial. Contemporaneous response messages document credential replacement and conversion of affected subaccounts to source-IP authentication. Portal audit data should still be requested if available. |
| Switchvox backup audit entries | Establishes backup creation and download after rotation | Observed during incident response; original raw logs not presently located |
| Service-suspension correspondence | Establishes the scope and approximate 48-hour operational disruption | Recovered. The December 17 suspension notice, signed carrier warning letter, and December 19 restoration message are retained. |
| Test backups from affected versions | Reproduces plaintext credential storage | Retain or regenerate privately; publish only synthetic extracts or hashes |
| Switchvox 8.4 test backup | Confirms changed backup format after remediation | Retain or regenerate privately; publish only non-sensitive format observations or hashes |

## Recovered Correspondence Index

The retained messages remain in the researcher's controlled mailbox. This public index omits customer names, account identifiers, telephone numbers, email addresses, and carrier ticket numbers.

| Date | Artifact | What it supports | Limitation |
| --- | --- | --- | --- |
| 2025-12-03 | Initial carrier fraud notice | Opens the carrier fraud case and identifies traffic attributed to the affected carrier account | Does not identify the credential-acquisition path |
| 2025-12-04 through 2025-12-12 | Repeated carrier fraud notices | Confirms that reported unauthorized traffic continued and supplies dated call examples | Embedded examples are not a complete raw CDR export |
| 2025-12-17 | Suspension message and signed warning letter | Confirms that the carrier characterized the traffic as ongoing illegal impersonation and suspended outbound termination | Describes carrier enforcement, not the initial-access method |
| 2025-12-17 through 2025-12-23 | Incident-response correspondence | Documents credential replacement and migration of affected subaccounts to source-IP authentication | Does not replace the missing Switchvox audit log |
| 2025-12-19 | Restoration message | Confirms removal of the outbound suspension after mitigation | Does not establish that every affected artifact was remediated |
| 2025-12-23 | Industry Traceback Group request | Characterizes a reported call as financial impersonation involving evidence of fraud, brand impersonation, and spoofing | Establishes the reported call and traceback, not the backup acquisition path |

## Evidence Handling

Before sharing an artifact publicly:

1. Remove live credentials, authentication tokens, customer names, complete account identifiers, telephone numbers, public IP addresses tied to customers, and unrelated personal information.
2. Preserve an unmodified private copy.
3. Record the file hash, source, collection date, and redaction method.
4. Publish a sanitized excerpt only when it adds information not already documented.
5. Use a private submission channel for CISA, MITRE, Sangoma, the carrier, or another authorized reviewer when full evidence is necessary.

Do not commit an unredacted `.svb` backup, packet capture, provider export, or support bundle to this repository.
