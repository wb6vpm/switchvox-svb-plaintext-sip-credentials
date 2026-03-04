# Switchvox .svb Plaintext SIP Credentials

This repository documents research related to plaintext SIP trunk credentials
stored within Switchvox `.svb` backup files.

The backup format contains carrier authentication credentials, carrier
hostname/IP information, and DID assignments in plaintext. Possession of a
backup file may allow extraction of these credentials and authentication
directly to the upstream carrier.

This effectively allows an external party to authenticate as the PBX endpoint
at the carrier level.

---

## Status

Coordinated disclosure in progress with the vendor.  
Additional technical details will be published after completion of the
disclosure process.

---

## Vulnerability Summary

Switchvox `.svb` backup files contain configuration data required to restore a
PBX installation, including SIP trunk configuration used to connect to
upstream carriers.

Analysis of the backup format shows that several pieces of carrier
authentication data are stored in plaintext within the backup structure,
including:

- SIP trunk authentication username
- SIP trunk authentication password
- carrier hostname or IP address
- assigned DID numbers
- trunk configuration parameters

Because this information is stored without encryption or obfuscation, any
party with access to the backup file can extract these credentials.

These credentials may allow direct authentication to the upstream SIP carrier
from outside the PBX environment.

---

## Potential Impact

Exposure of SIP trunk credentials may allow an attacker to authenticate as the
PBX endpoint at the carrier level.

Depending on carrier configuration, this may enable:

- outbound toll fraud
- caller ID spoofing
- impersonation of the PBX endpoint
- inbound call interception
- inbound call rerouting
- denial-of-service conditions for inbound calls

Because SIP registration determines the destination for inbound call delivery,
a successful registration from an attacker-controlled endpoint may cause calls
to be delivered to the attacker instead of the legitimate PBX.

---

## Example Attack Scenario

1. An attacker obtains a Switchvox `.svb` backup file through backup exposure,
   misconfigured storage, administrative access, or other data exposure.

2. The attacker extracts SIP trunk authentication credentials and carrier
   connection details from the backup.

3. Using these credentials, the attacker registers a SIP endpoint directly
   with the upstream carrier.

4. The carrier may treat the attacker-controlled endpoint as the legitimate
   PBX system.

Depending on carrier configuration, the attacker may be able to:

- originate outbound calls using the organization's trunk
- spoof outbound caller ID
- intercept inbound calls
- reroute inbound calls away from the legitimate PBX
- create inbound call denial-of-service conditions

---

## Repository Contents

`docs/`  
Technical analysis and documentation describing the vulnerability mechanics
and credential exposure behavior.

`timeline/`  
Disclosure timeline documenting vendor communication and the coordinated
disclosure process.

`README.md`  
Overview of the vulnerability and current disclosure status.

---

## Responsible Disclosure

This repository documents research related to a security issue identified in
Switchvox backup files.

The vendor was notified and coordinated disclosure is currently in progress.
Additional technical details will be published following completion of the
disclosure process.

---

## Legal Notice

All research described in this repository was conducted on systems owned or
explicitly authorized for testing by the researcher.

No unauthorized access to third-party systems was performed.
