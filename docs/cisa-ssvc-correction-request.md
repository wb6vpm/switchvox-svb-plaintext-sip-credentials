# CISA SSVC Exploitation Reassessment Request

## Requested Change

CISA's ADP enrichment for CVE-2026-45362 currently reports:

```text
Exploitation: none
Automatable: no
Technical Impact: partial
```

That assessment is timestamped `2026-05-12T13:22:12.871914Z`. It predates Sangoma's June 8 advisory and the researcher's revised public incident record.

The researcher requests that CISA reevaluate `Exploitation`. The public technical record appears to support `poc` under the CISA Coordinator definition. The researcher also asks CISA to review the first-hand incident observations and retained private corroboration and change the value to `active`. The requested outcome is `active`; `poc` is the minimum supportable correction.

The CISA Coordinator definition says `none` requires both no evidence of active exploitation and no public proof of concept. It defines `poc` to include privately attested evidence that has not been publicly shared, a typical public proof of concept, or a well-known exploitation method. This repository documents deterministic extraction using standard archive tools, a synthetic test case, and a first-hand account of the production incident that led to discovery.

## Basis for Reassessment

CVE-2026-45362 was identified while analyzing carrier-confirmed unauthorized calling involving multiple independent Switchvox PBX appliances, each using a separate SIP subaccount at the upstream carrier. The carrier treated the activity as toll fraud. The vulnerability was not discovered solely through hypothetical-impact analysis. The carrier-reported activity occurred earlier in December 2025, and analysis on December 30 identified the plaintext credential exposure in Switchvox backups.

The evidentiary record has three distinct layers.

### Retained carrier and traceback records

Retained correspondence supports the following:

1. The upstream carrier attributed unauthorized calls to the affected account and SIP subaccounts.
2. The carrier repeatedly reported continued unauthorized traffic involving affected SIP subaccounts during December 2025.
3. The carrier suspended outbound termination because of traffic it characterized as ongoing illegal impersonation.
4. The suspension disrupted legitimate outbound service for approximately 48 hours.
5. An Industry Traceback Group request characterized a reported call as financial impersonation involving evidence of fraud, brand impersonation, and spoofing.
6. Contemporaneous response messages document credential replacement for each affected PBX's carrier subaccount and migration of affected subaccounts to source-IP authentication.

These records corroborate the real production incident, its timing, mitigation, and operational consequences. They do not independently establish that a Switchvox backup was the credential-acquisition path.

Separately, the researcher's first-hand knowledge from administering the affected systems establishes that each affected SIP subaccount was configured on a separate Switchvox PBX appliance.

### First-hand incident-response observations

During the response, the researcher reviewed audit entries on every affected Switchvox PBX. On each appliance, the entries showed creation and download of a fresh backup after the SIP credentials assigned to that PBX's carrier subaccount had been rotated. The carrier later continued to attribute unauthorized traffic to affected SIP subaccounts after those rotations. Based on carrier and account records available during the response, the researcher's contemporaneous assessment was that the replacement credentials had been reacquired and used.

The raw Switchvox audit entries reviewed on the affected PBXs and a standalone carrier authentication export have not been recovered. The public report therefore does not claim that the downloaded backups were the exclusive possible credential source, identify who initiated any backup creation or download, or exclude every alternative acquisition path.

### Later authorized reproduction

Authorized testing on Switchvox 6.8, 7.9.5.2, and 8.3 independently established that affected `.svb` backups expose the current SIP authentication username, password, provider address, and trunk configuration in plaintext. Extraction is deterministic and uses standard gzip, tar, and text-processing tools. Authorized testing also confirmed that credentials recovered from affected backups could authenticate directly to tested upstream SIP carrier infrastructure where no independent source-IP restriction blocked their use.

The multi-appliance incident chronology, first-hand audit observations, and reproduced mechanics are consistent with reacquisition from the downloaded backups, but do not establish that path conclusively. The missing raw records prevent definitive attribution of the credential-acquisition path. The retained carrier records separately substantiate the unauthorized calling and service disruption.

## Public Support

- [Public research snapshot at commit 97879c7](https://github.com/wb6vpm/switchvox-svb-plaintext-sip-credentials/tree/97879c77c2c86fb25e8b6a91d7f9dd5f02522305)
- [Technical Analysis](technical-analysis.md), including deterministic archive-extraction steps and a documented synthetic test case
- [Observed Production Incident](observed-incident.md), including the sanitized chronology and evidentiary limits
- [Evidence Manifest](../evidence/README.md), including the retained-correspondence inventory
- [Sangoma advisory GHSA-mfm3-g35x-c9w8](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8), which confirms plaintext SIP credentials in affected backups and identifies toll fraud as a potential impact
- [CISA Vulnrichment record at commit 8456a09](https://github.com/cisagov/vulnrichment/blob/8456a09f90519e02a6ea1392914275c7cbcf2e64/2026/45xxx/CVE-2026-45362.json)
- [CISA Coordinator SSVC definition at commit 3b6f262](https://github.com/CERTCC/SSVC/blob/3b6f262ca4f135ce5875c047fe4f290636797d77/docs/ssvc-calc/CISA-Coordinator.json)

## Private Corroboration Available

The researcher can provide the following to CISA through an approved private channel:

- retained carrier fraud and support correspondence from December 2025
- the retained signed carrier warning letter
- explicit outbound-suspension and restoration messages
- the retained Industry Traceback Group request
- contemporaneous response messages documenting credential replacement and migration to source-IP authentication
- sanitized affected-version test artifacts or hashes, if requested

Preserve retained messages and attachments, including complete headers, unchanged and hashed in controlled storage. Before transmission, confirm CISA's requested scope and approved secure channel. Provide separate minimized or redacted copies unless originals are specifically required and disclosure is authorized. Treat headers, attachments, IP addresses, ticket numbers, telephone numbers, account identifiers, exact call data, and exact timing data as potentially identifying. Production SIP credentials and customer data will not be posted publicly.

## Ready-to-Send Message

**Subject:** `Request to reevaluate SSVC Exploitation for CVE-2026-45362`

```text
CISA SSVC Team,

I reported CVE-2026-45362, which concerns plaintext SIP authentication credentials in Sangoma Switchvox backups before version 8.4.

CISA's current ADP enrichment lists Exploitation as none. The assessment is timestamped May 12, 2026, before Sangoma's June 8 advisory and before I published a revised incident record. I request that CISA reassess the Exploitation value. The public technical record appears to support poc. I also ask CISA to review my first-hand incident observations and retained private corroboration and change the value to active. Active is the requested outcome; poc is the minimum supportable correction.

The vulnerability was identified while I was analyzing carrier-confirmed unauthorized calling involving multiple independent Switchvox PBX appliances, each using a separate SIP subaccount at the upstream carrier. The carrier treated the activity as toll fraud. Retained carrier records substantiate repeated unauthorized activity, a temporary outbound-service suspension, restoration after mitigation, and an industry traceback request. These records establish the incident and its consequences; they do not independently establish how the credentials were obtained.

During the response, I reviewed audit entries on every affected Switchvox PBX. On each appliance, the entries showed creation and download of a fresh backup after I had rotated the SIP credentials assigned to that PBX's carrier subaccount. The carrier later continued to attribute unauthorized traffic to affected SIP subaccounts after those rotations. Based on carrier and account records available during the response, my contemporaneous assessment was that the replacement credentials had been reacquired and used.

I later reproduced that affected Switchvox backups expose the current carrier username, password, provider address, and trunk context in plaintext. The multi-appliance incident chronology, audit observations, and reproduced mechanics are consistent with reacquisition from the downloaded backups, but do not establish that path conclusively. I have not recovered the raw Switchvox audit entries or a standalone carrier authentication export, so I do not claim that the downloaded backups were the exclusive possible source or that the identity of anyone who initiated a backup creation or download has been established.

The public technical record is here:
https://github.com/wb6vpm/switchvox-svb-plaintext-sip-credentials/tree/97879c77c2c86fb25e8b6a91d7f9dd5f02522305

The CISA assessment reviewed for this request is here:
https://github.com/cisagov/vulnrichment/blob/8456a09f90519e02a6ea1392914275c7cbcf2e64/2026/45xxx/CVE-2026-45362.json

At minimum, the public extraction steps and the CISA Coordinator definition appear to support poc. I ask CISA to determine whether my first-hand observations and the retained private corroboration satisfy active.

Please provide an approved secure method and requested scope for the retained carrier correspondence and other sensitive supporting records. I will preserve the source records and provide minimized, redacted, or original material as authorized and appropriate without exposing production credentials or customer data publicly.

Michael Overholser
GitHub: wb6vpm
```

Per [CISA's SSVC guidance](https://www.cisa.gov/resources-tools/resources/stakeholder-specific-vulnerability-categorization-ssvc), this request should be emailed to `vulnerability@cisa.dhs.gov` with `SSVC` in the subject line. This document is a prepared request, not a claim that CISA has already reviewed or accepted the correction.
