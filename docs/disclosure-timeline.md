# Disclosure Timeline

This timeline records discovery, vendor coordination, remediation, and publication for CVE-2026-45362. Dates are Pacific Time unless noted otherwise.

## 2025-12-30

- During response to toll fraud involving multiple independent Switchvox PBX appliances, each using a separate SIP subaccount at the upstream carrier, responders rotated the SIP credentials assigned to each affected PBX's carrier subaccount. The carrier then informed responders that affected SIP subaccounts were still sending unauthorized traffic after those rotations. That report prompted the researcher to review audit entries on every affected PBX. On each appliance, the entries showed creation and download of a fresh backup after the associated credential rotation.
- Later authorized reproduction of Switchvox `.svb` backups established that they contained plaintext SIP provider usernames, passwords, carrier addresses, DID assignments, and related configuration.
- The issue was reported to Sangoma security contacts.

## 2026-01-04

- The researcher provided additional technical detail and affected-version information.
- Sangoma acknowledged the report and began coordinated disclosure.

## 2026-02-02

- The researcher requested a status update after several weeks without further information.

## 2026-02-17 through 2026-02-19

- The researcher sent additional follow-ups.
- Sangoma confirmed that the report remained under review.
- The parties discussed CVSS v4.0 scoring, GitHub advisory metadata, and a proposed publication schedule.

## 2026-03-03

- Extended testing confirmed that recovered credentials could authenticate to upstream SIP infrastructure in authorized real-world deployments.
- Analysis documented potential carrier-dependent consequences beyond outbound toll fraud, including registration conflicts, inbound routing interference, and service disruption.
- A private research repository was created to organize the technical record.

## 2026-03-20

- Sangoma released Switchvox 8.4, build 105261.
- The release notes described an "Encrypted Backup Restore process."
- Initial testing found that 8.4 backups were no longer directly readable gzip/tar archives and instead began with a `Salted__` header.

## 2026-03-24 through 2026-03-26

- Sangoma stated that advisory publication remained planned for the end of March.
- Sangoma confirmed that backup encryption in Switchvox 8.4 was intended to address the reported issue.

## 2026-03-31

- The researcher completed initial validation of the changed 8.4 backup format.
- The researcher requested confirmation that publication would include a CVE identifier.

## 2026-04-13

- Sangoma stated that a CVE would be obtained before publication.

## 2026-04-20

- The researcher acknowledged the update and requested notice of further progress.

## 2026-05-01

- With the March publication target past, the researcher requested the advisory and CVE status.
- The researcher offered to request a CVE independently if Sangoma had not already done so.

## 2026-05-11

- No response had been received to the May 1 request.
- The researcher submitted an independent CVE request to MITRE and received a tracking case.
- MITRE assigned CVE-2026-45362.
- The initial record classified the issue as CWE-312 and published CVSS v3.1 score 3.2, Low.

## 2026-06-08

- Sangoma published [GHSA-mfm3-g35x-c9w8](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8).
- The advisory lists versions before 8.4 as affected and version 8.4 as patched.
- Sangoma added CWE-522 and CWE-530 and published CVSS v4.0 score 4.4, Medium.

## 2026-09-02 Pacific Time / 2026-09-03 UTC

- The public research record was revised to document the production incident that led to discovery, separate confirmed facts from acquisition-path attribution, and correct stale coordinated-disclosure language.
- The incident record was corrected to reflect that each affected carrier subaccount corresponded to a separate Switchvox PBX and that post-rotation backup creation and download was observed on every affected appliance.
- The researcher recovered retained carrier fraud correspondence, a signed warning letter, suspension and restoration messages, and an Industry Traceback Group request from the incident period.
- The [CISA ADP enrichment for CVE-2026-45362](https://cveawg.mitre.org/api/cve/CVE-2026-45362) continued to report `Exploitation: none`. The researcher disputes that assessment based on the observed incident and is assembling the surviving evidence for review.
- A corrected MITRE CVE record update request was prepared to supersede earlier standalone 10.0 proposals and request reassessment of the CNA's 3.2 Low score.

## Current Status

- CVE-2026-45362 is public.
- Sangoma's advisory is public.
- Switchvox 8.4 and later use the remediated backup format.
- Backups created by affected releases remain sensitive and should be protected or destroyed according to retention requirements.
- SIP credentials present in an affected backup should be rotated if the backup's chain of custody is uncertain.
