# CVE-2026-45362: Plaintext SIP Credentials in Switchvox Backups

Sangoma Switchvox On-Premise releases before 8.4 can store configured live SIP provider credentials in plaintext inside `.svb` system backups.

An affected `.svb` file is a standard compressed archive containing a readable PostgreSQL database export. Anyone who obtains the backup can recover configured SIP usernames, passwords, carrier addresses, DID assignments, and related trunk configuration without Switchvox software, password cracking, or a cryptographic attack.

Authorized testing confirmed that credentials recovered from affected backups could authenticate directly to tested upstream SIP carrier infrastructure. Where the carrier does not enforce an independent source-IP restriction, this permits impersonation of the PBX at the carrier layer and can result in toll fraud, caller identity abuse, call-routing interference, and service disruption.

## Status

| Item | Status |
| --- | --- |
| Identifier | [CVE-2026-45362](https://www.cve.org/CVERecord?id=CVE-2026-45362) |
| Vendor advisory | [GHSA-mfm3-g35x-c9w8](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8) |
| Vendor affected range | Switchvox On-Premise releases before 8.4 |
| Researcher-tested versions | 6.8, 7.9.5.2, and 8.3 |
| Patched version | [8.4](https://sangomakb.atlassian.net/wiki/spaces/Switchvox/pages/1521647617) |
| Weaknesses | CWE-312, CWE-522, and CWE-530 |
| CISA SSVC status | `Exploitation: none`; correction request prepared by the researcher |

Sangoma released encrypted backup handling in Switchvox 8.4. Upgrading does not revoke credentials contained in backups created by earlier versions.

## Observed Production Incident

This vulnerability was discovered while responding to a carrier-confirmed toll-fraud incident affecting multiple subaccounts at the upstream carrier.

The following events were established or observed during the response:

1. The carrier attributed unauthorized calls to the affected account and SIP subaccounts.
2. Responders rotated the affected credentials.
3. Switchvox logs reviewed during the incident showed that a fresh system backup was created and downloaded after the rotation.
4. Unauthorized traffic attributed to affected SIP subaccounts continued after credential rotation. Based on carrier and account records available during the response, the researcher's contemporaneous assessment was that the replacement credentials had been reacquired and used.
5. Carrier fraud controls suspended outbound calling for customers under the affected account structure for approximately 48 hours.

The incident establishes real unauthorized carrier-account use, financial exposure, and operational disruption. The sequence strongly supports the assessment that the newly generated backup was used to reacquire the replacement credentials.

The original Switchvox log files and a standalone carrier authentication export have not been recovered for public preservation. The available record therefore does not establish who initiated the backup download, prove that the backup was the only acquisition path, or exclude every alternative source of credential exposure. Those limitations affect attribution of the acquisition path. They do not change the confirmed toll fraud, credential rotation, continued unauthorized traffic after rotation, or service suspension.

See [Observed Production Incident](docs/observed-incident.md) for the sanitized chronology and evidence status.

## Technical Summary

Affected backups contain `DUMP_MINUS_LOGS`, a PostgreSQL database export. Configured live SIP provider credentials appear in plaintext in multiple records, including:

- `admin_activity_log`
- `ps_auths`
- `sip_providers`

The exposed data includes:

- SIP authentication usernames and passwords
- carrier hostnames or IP addresses
- DID assignments
- trunk configuration parameters

Possession of an affected backup is the required precondition. Once the file is obtained, extraction is deterministic and requires only standard archive and text-processing tools. No further access to the Switchvox appliance is required.

## Impact

### Observed impact

- unauthorized outbound call origination
- toll fraud across multiple carrier subaccounts
- continued unauthorized traffic attributed to affected SIP subaccounts after credential rotation
- carrier fraud mitigation affecting legitimate customers
- approximately 48 hours without outbound calling under the affected account structure

### Additional carrier-dependent impact

The exact capabilities available after authentication depend on carrier configuration. They can include:

- impersonation of the legitimate PBX endpoint
- caller ID abuse
- inbound call interception or rerouting
- competing-registration denial of service

Fraud response can also lead to provider-level blocking or account suspension, as it did during the incident documented here.

## Remediation

1. Upgrade Switchvox to version 8.4 or later.
2. Treat every backup created by an affected release as potentially containing recoverable live credentials.
3. Inventory old backups on the appliance, backup servers, file shares, cloud storage, ticketing systems, email, removable media, and administrator workstations.
4. Remove or restrict old backups according to applicable retention requirements.
5. Rotate every SIP provider credential if the chain of custody for any affected backup is uncertain.
6. Ask the carrier to restrict SIP authentication by source IP where operationally possible.
7. Enable destination restrictions, call limits, fraud alerts, and automatic spending controls.
8. Review backup creation, download, and administrative activity logs after any suspected compromise.

Upgrading protects newly generated backups. It does not make older backups safe, and it does not invalidate credentials already recovered from them.

## Severity and Public Assessments

Published assessments currently differ:

- The MITRE CNA record reports CVSS v3.1 score **3.2, Low**: `CVSS:3.1/AV:L/AC:H/PR:N/UI:N/S:C/C:L/I:N/A:N`.
- Sangoma reports **CVSS-B v4.0 4.4, Medium**: `CVSS:4.0/AV:L/AC:H/AT:N/PR:H/UI:P/VC:H/VI:L/VA:L/SC:L/SI:N/SA:N`.
- The researcher's evidence-led carrier-abuse scenario is **CVSS-B v4.0 8.2, High**: `CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:N/VA:N/SC:N/SI:H/SA:N`.
- The researcher's confidentiality-only baseline is **CVSS-B v4.0 6.9, Medium**: `CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N`.

These scores make different assumptions about how the backup is acquired and whether the demonstrated carrier-side integrity impact is included. CVSS does not directly quantify financial loss, response cost, customer count, or reputational harm. The 48-hour suspension is not scored as subsequent-system availability impact here because it was a defensive carrier response, not attacker-controlled technical denial.

See [Severity and Classification](docs/severity-and-classification.md) for the scoring analysis, CWE mappings, and current exploitation-status dispute.

## Documentation

- [Technical Analysis](docs/technical-analysis.md)
- [Observed Production Incident](docs/observed-incident.md)
- [Severity and Classification](docs/severity-and-classification.md)
- [CISA SSVC Exploitation Reassessment Request](docs/cisa-ssvc-correction-request.md)
- [Disclosure Timeline](docs/disclosure-timeline.md)
- [Evidence Manifest](evidence/README.md)

## Disclosure Summary

- 2025-12-30: Vulnerability discovered and reported to Sangoma.
- 2026-03-20: Switchvox 8.4 released with encrypted backup handling.
- 2026-05-11: MITRE assigned CVE-2026-45362.
- 2026-06-08: Sangoma published GHSA-mfm3-g35x-c9w8.

The complete chronology is in the [Disclosure Timeline](docs/disclosure-timeline.md).

## Legal and Ethical Notice

All reproduction and validation described in this repository used systems, backups, carrier accounts, and infrastructure owned by the researcher or explicitly authorized for testing.

The researcher did not access any third-party system without authorization. Published examples use synthetic credentials and do not contain functioning production secrets, customer backups, or unredacted customer data.
