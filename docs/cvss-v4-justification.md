# CVSS v4.0 Justification

## Vector
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H

---

## Summary

This vulnerability allows extraction of SIP authentication credentials from Sangoma Switchvox backup files. These credentials can be used to authenticate directly to upstream SIP carriers, enabling impersonation of the PBX, unauthorized call origination, caller ID spoofing, and disruption of inbound call routing.

---

## Attack Vector (AV:N)

Exploitation does not require local or physical access to the PBX. Backup files may be obtained through remote access paths such as administrative interfaces, remote backups, exposed storage locations, or compromised accounts. Use of the extracted credentials occurs over network protocols against upstream SIP infrastructure.

---

## Attack Complexity (AC:L)

No special conditions or timing requirements are required. Credential extraction from backup files is deterministic and succeeds consistently once the backup file is obtained.

---

## Attack Requirements (AT:N)

Per CVSS v4.0 definition, attack success does not depend on deployment or execution conditions of the vulnerable system. Once a backup file is obtained, credential extraction succeeds in all cases without reliance on environmental factors such as configuration, timing, or race conditions.

---

## Privileges Required (PR:N)

No privileges are required to extract credentials from the backup file.

---

## User Interaction (UI:N)

No user interaction is required.

---

## Vulnerable System Impact

### Confidentiality (VC:H)

SIP authentication credentials are fully exposed in backup files, allowing unauthorized parties to recover sensitive authentication material.

### Integrity (VI:H)

An attacker can use recovered credentials to impersonate the PBX, originate calls, and spoof caller identity, impacting trust and correctness of communications.

### Availability (VA:H)

An attacker may register to the SIP carrier using the recovered credentials, causing conflicts or takeover of the legitimate PBX registration. This can result in loss of inbound call functionality, misrouting of calls, or complete denial of telephony service.

---

## Subsequent System Impact

### Confidentiality (SC:H)

Exposure of SIP credentials enables interaction with external carrier systems, potentially exposing call metadata or enabling interception scenarios depending on carrier behavior.

### Integrity (SI:H)

The attacker can manipulate call flows and signaling at the carrier level, impacting systems beyond the original PBX.

### Availability (SA:H)

Unauthorized registration or misuse of credentials can disrupt carrier-level routing, resulting in loss of service beyond the vulnerable system boundary.

---

## Alternate Interpretation

Some interpretations may consider Attack Requirements (AT) as Present due to the need for an attacker to obtain a backup file. However, per CVSS v4.0 definitions, Attack Requirements refer to conditions affecting exploitation success, not the method of acquiring the vulnerable artifact. In this case, exploitation is deterministic once the backup file is obtained, and no additional conditions influence success.
