# Switchvox SVB Plaintext SIP Credentials

This repository documents security research related to plaintext SIP trunk credentials stored within Switchvox `.svb` backup files.

The backup format contains carrier authentication credentials, carrier hostname/IP information, and DID assignments in plaintext. Possession of a backup file may allow extraction of these credentials and authentication directly to the upstream SIP carrier.

This effectively allows an external party to impersonate the PBX endpoint at the carrier level.

Vendor remediation changes were released in Switchvox 8.4 prior to public release of this repository.

---

## Status

Coordinated disclosure was initiated with the vendor. Patch-related changes were released in Switchvox 8.4, and an independent CVE request was submitted to MITRE following expiration of the previously discussed publication timeline.

CVE-2026-45362 has been assigned to track this issue.

Disclosure timeline details are documented in `timeline/disclosure.md`.

---

## Security Advisory

A GitHub Security Advisory has been opened to coordinate disclosure of this issue.

CVE: `CVE-2026-45362`

GHSA: `GHSA-mfm3-g35x-c9w8`

GitHub Advisory:  
https://github.com/advisories/GHSA-mfm3-g35x-c9w8

(Note: The advisory is currently private or unpublished as part of the coordinated disclosure process and may not yet be publicly accessible.)

Additional advisory metadata and publication details may be updated as the disclosure process continues.

---

## Vulnerability Summary

Switchvox `.svb` backup files contain configuration data required to restore a PBX installation, including SIP trunk configuration used to connect to upstream carriers.

Analysis of the backup format shows that several pieces of carrier authentication data are stored in plaintext within the backup structure, including:

- SIP trunk authentication username
- SIP trunk authentication password
- carrier hostname or IP address
- assigned DID numbers
- trunk configuration parameters

Because this information is stored without encryption or obfuscation, any party with access to the backup file can extract these credentials.

These credentials may allow direct authentication to the upstream SIP carrier from outside the PBX environment.

---

## Potential Impact

Exposure of SIP trunk credentials may allow an attacker to impersonate the PBX endpoint at the carrier level.

Depending on carrier configuration, this may enable:

- outbound toll fraud
- caller ID spoofing
- impersonation of the PBX endpoint
- inbound call interception
- inbound call rerouting
- denial-of-service conditions for inbound calls

Because SIP registration determines the destination for inbound call delivery, a successful registration from an attacker-controlled endpoint may cause calls to be delivered to the attacker instead of the legitimate PBX.

---

## Severity

This issue is assessed as **Critical** under CVSS v4.0.

- **Vector:** `CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H`
- See: [CVSS v4.0 Justification](docs/cvss-v4-justification.md)

---

## Example Attack Scenario

1. An attacker obtains a Switchvox `.svb` backup file through backup exposure, misconfigured storage, administrative access, or other data exposure.

2. The attacker extracts SIP trunk authentication credentials and carrier connection details from the backup.

3. Using these credentials, the attacker registers a SIP endpoint directly with the upstream carrier.

4. The carrier may treat the attacker-controlled endpoint as the legitimate PBX system.

Depending on carrier configuration, the attacker may be able to:

- originate outbound calls using the organization's trunk
- spoof outbound caller ID
- intercept inbound calls
- reroute inbound calls away from the legitimate PBX
- create inbound call denial-of-service conditions

---

## Documentation

Technical documentation is available in the `docs/` directory:

- `archive-format.md` – structure of Switchvox `.svb` backup files  
- `vulnerability-mechanics.md` – explanation of the credential exposure vulnerability  
- `credential-storage.md` – example plaintext credential storage extracted from a backup  
- `cvss-v4-justification.md` – CVSS v4.0 scoring rationale and metric justification  
- `cwe-analysis.md` – discussion of applicable CWE classifications related to plaintext credential exposure and backup handling
- 
---

## Repository Contents

`docs/`  
Technical analysis and documentation describing the vulnerability mechanics and credential exposure behavior.

`timeline/`  
Disclosure timeline documenting vendor communication and the coordinated disclosure process.

`README.md`  
Overview of the vulnerability and current disclosure status.

---

## Responsible Disclosure

This repository documents research related to a security issue identified in Switchvox backup files.

The vendor was notified and coordinated disclosure was initiated prior to public release of this repository. Patch-related changes were released in Switchvox 8.4, and an independent CVE request was submitted to MITRE following expiration of the previously discussed publication timeline.

---

## Legal Notice

All research described in this repository was conducted on systems owned or explicitly authorized for testing by the researcher.

No unauthorized access to third-party systems was performed.
