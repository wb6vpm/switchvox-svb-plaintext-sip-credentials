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

This inventory is organized by artifact type, not chronology. The incident sequence is documented in [Observed Production Incident](../docs/observed-incident.md).

| Artifact | Evidentiary purpose | Status |
| --- | --- | --- |
| Carrier fraud alerts and support correspondence | Establishes fraud reports, affected account scope, repeated traffic notices, the separate SIP subaccounts, and provider response | Recovered in retained mailbox threads; not included in the public repository. |
| Carrier call-detail records | Establishes unauthorized destinations, timing, duration, and affected SIP subaccounts | Partial. Dated call examples and an Industry Traceback Group request are retained in carrier correspondence. |
| Affected-system topology | Establishes that each affected SIP subaccount was configured on a separate Switchvox PBX appliance | First-hand knowledge from administering the affected systems during incident response. |
| SIP authentication records | Establishes use of valid carrier credentials and source details | No standalone raw export has been recovered. |
| Credential-rotation records | Establishes when replacement credentials assigned to each affected PBX's carrier subaccount became active | Partial. Contemporaneous response messages document credential replacement and conversion of affected subaccounts to source-IP authentication. |
| Switchvox backup audit entries | Establishes the researcher's direct observation that, on every affected PBX, a fresh backup was created and downloaded after the associated SIP credentials had been rotated | The carrier's report that affected SIP subaccounts were still sending unauthorized traffic after the credential rotations prompted the researcher to review these entries. Containment, credential rotation, and service restoration were the immediate priorities, so the entries were not separately exported for long-term preservation. The raw entries have not since been recovered and are not currently available for independent reexamination. |
| Service-suspension correspondence | Establishes the scope and approximate 48-hour operational disruption | Recovered. The December 17 suspension notice, signed carrier warning letter, and December 19 restoration message are retained. |
| Test backups from affected versions | Reproduces plaintext credential storage | Synthetic reproduction and format observations are documented in Technical Analysis. Raw backups are not published. |
| Switchvox 8.4 test backup | Confirms changed backup format after remediation | Format observations are documented in Technical Analysis. The raw backup is not published. |

## Recovered Correspondence Index

The retained messages remain in the researcher's controlled mailbox. This public index omits customer names, account identifiers, telephone numbers, email addresses, and carrier ticket numbers.

This index lists recovered messages by date and is not a complete event-by-event chronology. A message date is not necessarily the date of every action described in that message. The relevant incident and research sequence was credential rotation; the carrier's report of continued unauthorized traffic after those rotations; the audit review prompted by that report; the review's finding that a fresh backup had been created and downloaded after each associated rotation; and later authorized reproduction establishing plaintext credentials in affected backups.

| Date | Artifact | What it supports | Limitation |
| --- | --- | --- | --- |
| 2025-12-03 | Initial carrier fraud notice | Opens the carrier fraud case and identifies traffic attributed to the affected carrier account | Does not identify the credential-acquisition path |
| 2025-12-04 through 2025-12-12 | Repeated carrier fraud notices | Confirms that reported unauthorized traffic involving affected SIP subaccounts continued and supplies dated call examples | Embedded examples are not a complete raw CDR export. This row does not identify the specific post-rotation notification that prompted the audit review. |
| 2025-12-17 | Suspension message and signed warning letter | Confirms that the carrier characterized the traffic as ongoing illegal impersonation and suspended outbound termination | Describes carrier enforcement, not the initial-access method |
| 2025-12-17 through 2025-12-23 | Incident-response correspondence | Documents credential replacement for each affected PBX's carrier subaccount and migration of affected subaccounts to source-IP authentication | Does not replace the unrecovered raw Switchvox audit entries from the affected PBXs. Message dates alone do not establish credential-rotation timestamps. |
| 2025-12-19 | Restoration message | Confirms removal of the outbound suspension after mitigation | Does not establish that every affected artifact was remediated |
| 2025-12-23 | Industry Traceback Group request | Characterizes a reported call as financial impersonation involving evidence of fraud, brand impersonation, and spoofing | Establishes the reported call and traceback, not the backup acquisition path |

## Public Evidence Policy

Production SIP credentials, customer backups, unredacted logs, packet captures, provider exports, and customer-identifying records are not included in this repository. Public reproduction examples use synthetic credentials.

Retained sensitive supporting records are available for authorized private review.
