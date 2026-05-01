# Disclosure Timeline

This document records the timeline for discovery and coordinated disclosure of plaintext SIP trunk credentials stored within Switchvox `.svb` backup files.

The issue allows extraction of carrier authentication credentials and related configuration data from backup files. These credentials may allow direct authentication to upstream carriers, enabling impersonation of the PBX endpoint for both outbound and inbound SIP traffic depending on carrier configuration.

Dates below document the disclosure process and the evolution of the analysis (all dates are Pacific Time (PT), USA).

## 2025-12-30
Initial discovery during analysis of Switchvox `.svb` backup files.

Plaintext storage of SIP trunk credentials, carrier hostname/IP information, and DID assignments identified within the backup structure.

## 2025-12-30
Initial disclosure sent to Sangoma security contacts describing credential exposure within `.svb` backups and potential authentication abuse scenarios.

## 2026-01-04
Additional details provided confirming tested and affected versions.

## 2026-01-04
Vendor acknowledgement received and coordinated disclosure process initiated.

## 2026-02-02
Follow-up sent after several weeks without updates requesting status on the disclosure and advisory progress.

## 2026-02-17
Additional follow-up sent requesting update on the disclosure and advisory status.

## 2026-02-17
Vendor response indicating the issue would be checked internally.

## 2026-02-19
Vendor response thanking the reporter for the submission and confirming the report was under review.

## 2026-02-19
CVSS v4 scoring discussion initiated and preliminary scoring applied within the GitHub security advisory.

## 2026-02-19
Additional clarification provided regarding CVSS metric selection and GitHub's CVSS tooling limitations.

## 2026-02-19
Vendor acknowledged additional context and discussion around the scoring and impact.

## 2026-02-19
Discussion continued regarding potential final CVSS scoring.

## 2026-02-19
Disclosure timeline discussed and agreed upon in principle.

## 2026-03-03
Additional impact analysis identified during extended review.

It was determined that credentials extracted from `.svb` backup files may allow direct SIP registration to the upstream carrier. Because SIP registration determines where inbound calls are routed by the carrier, this may allow an attacker to:

- impersonate the PBX endpoint at the carrier level  
- intercept or reroute inbound calls  
- create inbound call denial-of-service conditions  
- potentially capture inbound call audio depending on call flow  

This expands the previously documented impact beyond outbound abuse scenarios such as toll fraud and caller-ID spoofing.

## 2026-03-03
Private research repository created to document technical analysis, version coverage, disclosure notes, and supporting material related to this vulnerability.

## 2026-03-20
Vendor releases Switchvox 8.4.

Sangoma releases Switchvox version 8.4. The release includes a change described as an "Encrypted Backup Restore process."

Testing confirms that backup files in version 8.4 are no longer stored as directly readable gzip/tar archives and instead use an encrypted format (e.g., "Salted__" header observed).

The impact of this change is consistent with mitigation of plaintext credential exposure in backup files described in this research. However, no public security advisory or CVE has been issued by the vendor at the time of this release.

## 2026-03-24
Vendor indicates publication remains on track for end of March.

Vendor notes that the initial fix involves password-encrypted protection of backup files to address the immediate issue, with additional improvements planned.

## 2026-03-26
Vendor confirms Switchvox 8.4 has begun shipping and includes backup encryption changes intended to address this issue.

## 2026-03-31
Researcher validates updated backup format in version 8.4.

Testing confirms backups are no longer directly readable in plaintext and now use an encrypted format. Credentials remain recoverable when the encryption mechanism is satisfied, reinforcing the importance of protecting backup files and associated key material.

Researcher requests confirmation regarding whether publication will include a CVE or rely solely on the GHSA.

## 2026-04-13
Vendor confirms that a CVE will be obtained prior to publication.

## 2026-04-20
Researcher acknowledges vendor update and requests to be kept informed of further progress.

## 2026-05-01
Researcher submits follow-up requesting updated timeline for advisory publication.

Follow-up references:
- original March 30 publication target  
- March 20 release of Switchvox 8.4  

Researcher requests clarification on CVE status, explicitly distinguishing between:
- CVE request not yet submitted  
- CVE request submitted but pending issuance  

Researcher offers to submit a CVE request independently if necessary to ensure timely tracking.

## Current Status (as of 2026-05-01)

- Patch addressing the issue released in Switchvox 8.4 (2026-03-20)  
- Vendor-stated publication target (end of March 2026) has passed  
- CVE has not yet been issued  
- GitHub Security Advisory remains unpublished with incomplete metadata (e.g., affected versions, patched versions, severity)  

The coordinated disclosure process remains ongoing.

Additional entries will be added as the coordinated disclosure process continues.
