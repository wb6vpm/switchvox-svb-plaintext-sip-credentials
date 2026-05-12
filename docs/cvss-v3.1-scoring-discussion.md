# CVSS v3.1 Scoring Discussion

This document discusses CVSS v3.1 scoring considerations related to CVE-2026-45362 and the plaintext exposure of SIP carrier authentication credentials within Switchvox `.svb` backup files.

The vulnerability involves storage of active SIP carrier authentication credentials in a directly readable format within backup archives. Depending on carrier configuration and SIP registration behavior, extracted credentials permit direct authenticated interaction with upstream SIP carrier infrastructure.

This document is intended to provide technical context regarding CVSS v3.1 interpretation considerations for this issue and to explain why different scoring approaches may reasonably exist.

---

## Published CVSS v3.1 Vector

The currently published (initial) CVSS v3.1 vector associated with CVE-2026-45362 is:

    CVSS:3.1/AV:L/AC:H/PR:N/UI:N/S:C/C:L/I:N/A:N

This produces a CVSS v3.1 Base Score of:

    3.2 LOW

The published vector appears to model the vulnerability primarily as local plaintext credential disclosure with limited confidentiality impact.

---

## Nature of the Vulnerability

Switchvox `.svb` backup files may contain:

- SIP carrier authentication usernames
- SIP carrier authentication passwords
- carrier hostname or IP information
- DID assignments
- SIP trunk configuration details

These credentials are used to authenticate directly with upstream SIP carrier infrastructure.

Testing and validation performed during investigation confirmed that extracted credentials were capable of successful authentication against upstream SIP carrier infrastructure in affected real-world deployments.

---

## Operational Characteristics of SIP Authentication

In many SIP deployments, authentication credentials represent the trust relationship between the PBX and the upstream carrier.

Depending on carrier implementation details, successful SIP registration influences:

- inbound call routing behavior
- outbound caller identity authorization
- trunk ownership state
- call delivery destination
- service availability

As a result, compromise of SIP carrier credentials creates downstream operational consequences beyond generic information disclosure.

---

## CVSS Interpretation Ambiguity

One of the primary scoring ambiguities for this issue involves whether CVSS v3.1 should model the vulnerability primarily as:

- plaintext disclosure of sensitive information within a backup archive

or:

- exposure of active authentication credentials capable of enabling authenticated interaction with upstream telecommunications infrastructure

However, these two conditions are not reasonably separable in the context of this vulnerability.

The exposed information is not merely passive sensitive data or historical configuration information. The backup files contain active SIP carrier authentication credentials which directly represent the PBX system's authentication authority with upstream telecommunications infrastructure.

The significance of this vulnerability is not merely that sensitive information is disclosed, but that the disclosed information consists of active authentication credentials representing operational telecommunications trust authority.

Loss of confidentiality of the credentials directly enables downstream authenticated interaction with carrier systems in affected deployments. The operational consequences are therefore directly tied to the confidentiality failure itself, rather than representing an unrelated or purely environmental secondary issue.

Testing performed during investigation confirmed that extracted credentials were capable of successful authentication against upstream SIP carrier infrastructure in real-world affected deployments.

Because of this relationship, evaluating the vulnerability purely as local plaintext credential disclosure underrepresents the practical security impact associated with compromise of the exposed authentication material.

This creates a difficult scoring scenario within CVSS v3.1 because the vulnerability itself is a confidentiality failure, while the direct consequence of that confidentiality failure is authenticated access to downstream telecommunications infrastructure.

The practical security significance of the exposed credentials derives from their ability to authenticate remotely to upstream SIP carrier infrastructure. Without that downstream authenticated capability, the plaintext disclosure represents a materially different class of vulnerability with substantially reduced operational impact.

Where multiple technically defensible scoring interpretations exist, interpretations that incorporate the direct operational consequences enabled by compromise of exposed authentication material provide a more complete representation of practical security impact than interpretations which model the issue purely as passive information disclosure.

Both interpretations remain technically defensible within CVSS v3.1 methodology. However, this vulnerability illustrates how exposure of active authentication material blurs the distinction between confidentiality impact and the operational capabilities granted by the compromised credentials.

---

## Attack Vector Considerations

The published vector uses:

    AV:L

This interpretation models the vulnerability at the point where an attacker obtains access to the backup archive containing the exposed credentials.

An alternative interpretation considers the downstream use of extracted credentials against remotely accessible SIP carrier infrastructure. However, CVSS v3.1 guidance does not clearly define how credential extraction vulnerabilities with downstream authenticated abuse should be modeled.

The practical security significance of the exposed credentials derives from their ability to authenticate remotely to upstream SIP carrier infrastructure. Without that downstream authenticated capability, the plaintext disclosure represents a materially different class of vulnerability with substantially reduced operational impact.

---

## Attack Complexity Considerations

The published vector uses:

    AC:H

The vulnerability does not require bypass of cryptographic protections or complex exploitation conditions once a backup file is obtained. Credentials are extracted directly from the backup structure without additional technical exploitation steps.

However, environmental conditions such as carrier configuration and SIP registration behavior influence downstream operational impact.

---

## Credential Confidentiality and Downstream Impact

For this vulnerability, the confidentiality impact cannot reasonably be evaluated as exposure of passive information alone.

The exposed data consists of active SIP carrier authentication credentials. Loss of confidentiality of those credentials directly enables authenticated interaction with upstream SIP carrier infrastructure in affected deployments.

As a result, the security impact is not limited to the fact that sensitive information is readable within a backup file. The exposed credentials represent the PBX endpoint's authentication authority at the carrier layer. Compromise of that authority enables outbound call origination, caller identity impersonation, inbound call rerouting, inbound call interception, or denial-of-service conditions depending on carrier configuration and SIP registration behavior.

Because the operational impact flows directly from the loss of confidentiality of the credentials, the plaintext storage weakness and the downstream carrier-authentication impact should not be treated as unrelated or purely speculative issues.

---

## Confidentiality Impact Considerations

The published vector uses:

    C:L

Exposure of SIP carrier credentials and associated telecommunications configuration data clearly involves confidentiality impact.

The exact scope of confidentiality impact varies depending on carrier capabilities and deployment configuration.

---

## Integrity Impact Considerations

The published vector uses:

    I:N

However, extracted credentials enable:

- caller identity impersonation
- outbound call origination
- impersonation of the PBX endpoint at the carrier layer
- inbound call rerouting

These behaviors reasonably represent integrity impact affecting telecommunications routing and caller identity trust relationships.

---

## Availability Impact Considerations

The published vector uses:

    A:N

However, depending on carrier implementation behavior, extracted credentials enable:

- denial-of-service conditions affecting inbound call delivery
- inbound routing disruption
- SIP registration conflicts
- operational service interruption during mitigation activities

Operational impact observed during investigation included emergency mitigation measures requiring temporary suspension of outbound calling functionality while affected SIP authentication credentials were rotated and secured.

These characteristics reasonably support consideration of availability impact in scoring interpretations.

---

## Scope Considerations

The published vector uses:

    S:C

This appears consistent with the possibility that compromise of credentials stored within Switchvox backup files affects downstream telecommunications infrastructure outside the direct security authority of the PBX platform itself.

---

## CVSS v3.1 Limitations

This vulnerability highlights several areas where CVSS v3.1 interpretation becomes difficult:

- credential extraction leading to downstream authenticated abuse
- telecommunications trust relationships
- SIP registration semantics
- downstream infrastructure impacts
- operational effects dependent on carrier behavior

The distinction between:

- exposure of credentials themselves

and:

- downstream capabilities enabled by those credentials

can lead to materially different scoring interpretations.

---

## Relationship to CVSS v4.0

CVSS v4.0 introduces additional concepts intended to better model downstream and subsequent-system impacts.

Because this vulnerability involves downstream authenticated interaction with external telecommunications infrastructure, CVSS v4.0 provides a more expressive framework for modeling the operational consequences associated with exposed SIP carrier credentials.

Additional discussion regarding CVSS v4.0 scoring is documented in:

    docs/cvss-v4-justification.md

---

## Notes

This document reflects the researcher's technical analysis and interpretation of CVSS v3.1 scoring considerations associated with CVE-2026-45362.

Official CVSS scores, CWE mappings, or severity assessments published by MITRE, NVD, vendors, or other parties may differ.