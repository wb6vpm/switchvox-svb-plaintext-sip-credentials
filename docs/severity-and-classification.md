# Severity and Classification

## Summary

The published CVSS scores for CVE-2026-45362 are Low and Medium. The researcher's evidence-led carrier-abuse scenario is **CVSS-B 8.2, High** under CVSS v4.0. A confidentiality-only researcher baseline is **CVSS-B 6.9, Medium**.

The previous CVSS v3.1 and v4.0 10.0 vectors have been withdrawn as standalone assessments of CVE-2026-45362. They mixed different stages of the attack chain: they used Network because the recovered credential is later used against a carrier, None for privileges because the analysis began after a protected backup had already been acquired, and vulnerable-system integrity and availability values for effects that occur at the carrier. CVSS requires exploitability metrics to be assessed against the vulnerable system and subsequent-system effects to be recorded separately.

## Published Assessments

| Source | Version | Score | Vector |
| --- | --- | --- | --- |
| [MITRE CNA record](https://www.cve.org/CVERecord?id=CVE-2026-45362) | CVSS v3.1 | 3.2, Low | `CVSS:3.1/AV:L/AC:H/PR:N/UI:N/S:C/C:L/I:N/A:N` |
| [Sangoma advisory](https://github.com/sangoma/security-switchvox/security/advisories/GHSA-mfm3-g35x-c9w8) | CVSS v4.0 | 4.4, Medium | `CVSS:4.0/AV:L/AC:H/AT:N/PR:H/UI:P/VC:H/VI:L/VA:L/SC:L/SI:N/SA:N` |

CVSS v3.1 and v4.0 use different scoring systems, so their numeric results are not directly comparable.

## Researcher Assessments

### Evidence-led carrier-abuse scenario

The primary researcher vector models an attacker with High privileges on Switchvox using the network-bound administrative interface to create or download an `.svb` backup:

```text
CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:N/VA:N/SC:N/SI:H/SA:N
```

**CVSS-B score: 8.2, High**

| Metric | Value | Rationale |
| --- | --- | --- |
| AV | Network | Switchvox exposes backup creation and download through its network-bound administrative interface. This value does not rely on the later carrier connection. |
| AC | Low | Once administrative backup access is available, credential recovery is deterministic and requires no security-control bypass or cryptographic work. |
| AT | None | No race, interception position, or other deployment condition is required. |
| PR | High | The documented product path to create or download a backup requires administrative authority. |
| UI | None | No separate victim must interact with an attacker-controlled payload. |
| VC | High | The backup discloses a complete, active SIP provider secret and the configuration needed to use it. |
| VI | None | Reading the backup does not itself modify Switchvox data. |
| VA | None | Reading the backup does not itself deny availability of the Switchvox appliance. |
| SC | None | Inbound interception has not been treated as a demonstrated capability for the primary vector. |
| SI | High | Recovered credentials were validated for direct carrier authentication outside PBX controls. The credential permits attacker-selected authenticated call origination under a victim subaccount, defeating the carrier authentication boundary. The production incident also established carrier-attributed unauthorized call sessions under affected SIP subaccounts. This is serious integrity impact to a separately controlled carrier system. |
| SA | None | The provider's fraud-response suspension is real operational harm, but it is not treated as direct technical availability impact to the carrier for the base vector. |

`SI:H` is the decisive and most contestable metric in this scenario. It is appropriate where the recovered credential completely subverts the carrier account's call-authentication boundary, permits attacker-selected origination within the affected account's allowed call scope under the victim identity, and causes a direct, serious technical consequence. `SI:L` is the conservative choice when that direct, serious technical consequence cannot be substantiated.

Using `SI:L` produces the following intermediate assessment:

```text
CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:N/VA:N/SC:N/SI:L/SA:N
```

**CVSS-B score: 6.9, Medium**

High-level administrative access permits backup export and provider management, but the exposure still creates a meaningful privilege delta: the exported backup reveals the existing reusable carrier secret itself. That secret can be exercised directly against carrier infrastructure, outside PBX dial-plan and monitoring controls, and after access to the PBX is lost.

### Confidentiality-only baseline

For readers who do not accept a subsequent-system integrity score without fuller carrier-side evidence, the direct administrative-backup path has this confidentiality-only baseline:

```text
CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N
```

**CVSS-B score: 6.9, Medium**

Both researcher vectors model the network-bound Switchvox administrative backup workflow. An offline copy of an already-exported archive is a separate access scenario and should be scored according to how that copy is reached. Neither researcher vector assumes inbound interception, registration takeover, carrier-side confidentiality loss, or direct technical denial of service.

If another vulnerability or compromised account supplies backup access, each weakness should be scored individually and the combined path should be identified as a vulnerability chain. A separate acquisition flaw should not be hidden by removing the privilege requirement from this CVE.

## Why the Public Scores Understate the Technical Risk

The CVE record's 3.2 vector is a narrow artifact-access scenario, but `AC:H` and `C:L` are weak choices for this issue. Credential extraction is deterministic, and the disclosed value is a complete, active authentication secret rather than a passive identifier. The vector also omits the verified downstream integrity effect of reusable carrier credentials.

Sangoma's vector correctly calculates to 4.4 and reasonably assigns High privileges to the administrative backup path. Its High attack complexity is difficult to support where no security mechanism must be evaded after backup access is obtained. Passive user interaction is also unnecessary when an attacker uses the administrative workflow directly. Sangoma assigns Low integrity and availability impact to Switchvox without identifying a demonstrated modification or denial of the PBX, and it assigns no subsequent-system integrity impact despite recognizing toll fraud in the advisory.

## CVSS Is Not the Incident Record

CVSS measures technical severity. It does not directly quantify financial loss, response cost, customer count, or reputational harm.

For this vulnerability, those factors are not speculation. The incident involved multiple independent Switchvox PBX appliances, each using a separate carrier SIP subaccount; toll fraud across affected subaccounts; continued unauthorized traffic attributed to affected subaccounts after the associated credential rotations; and approximately 48 hours of outbound service suspension under the affected account structure. The suspension is not scored as `SA` here because it was a defensive carrier response, not attacker-controlled technical denial. It remains part of the actual risk and incident record.

## Exploitation Status

As of 2026-09-03, [CISA's ADP enrichment](https://cveawg.mitre.org/api/cve/CVE-2026-45362) reports `Exploitation: none` for CVE-2026-45362. The researcher disputes that designation based on the [observed production incident](observed-incident.md).

The evidence distinguishes between two propositions:

- The toll fraud, credential rotations for each affected PBX's carrier subaccount, continued unauthorized traffic after those rotations, and service disruption are confirmed. The researcher's contemporaneous assessment, based on records available during response, was that the replacement credentials had been reacquired and used.
- The researcher directly observed audit entries on every affected PBX showing post-rotation creation and download of a fresh backup. This repeated multi-appliance sequence strongly supports the newly generated backups as the credential reacquisition path. During the active response, containment, credential rotation, and service restoration were the immediate priorities, so the entries were not separately exported for long-term preservation. They have not since been recovered and are not currently available for independent reexamination. Accordingly, the report does not identify the person or system that initiated the backup activity or claim that the downloaded backups were the exclusive possible credential-acquisition path.

The current `none` assessment appears inconsistent with the CISA Coordinator decision point. The public [technical analysis](technical-analysis.md) provides deterministic extraction steps and a documented synthetic test case, while the CISA Coordinator definition of `poc` includes well-known exploitation methods and privately attested evidence that has not been shared publicly. At minimum, those public steps appear to support `poc`, even if `active` requires CISA's private review of the incident evidence.

See [CISA SSVC Exploitation Reassessment Request](cisa-ssvc-correction-request.md) for the requested `active` assessment, the minimum `poc` correction, and the available corroboration.

## CWE Classification

| CWE | Applicability |
| --- | --- |
| [CWE-312: Cleartext Storage of Sensitive Information](https://cwe.mitre.org/data/definitions/312.html) | Primary storage weakness. The backup contains readable secrets. |
| [CWE-522: Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html) | Directly applicable because the exposed values are active authentication credentials. |
| [CWE-530: Exposure of Backup File to an Unauthorized Control Sphere](https://cwe.mitre.org/data/definitions/530.html) | Contextually applicable to backup export, transfer, external storage, and retention. Access to the backup remains a required precondition. |

## Methodology References

- [CVSS v4.0 specification](https://www.first.org/cvss/v4.0/specification-document)
- [CVSS v4.0 calculator](https://www.first.org/cvss/calculator/4.0)
- [CVSS v3.1 specification](https://www.first.org/cvss/v3.1/specification-document)
