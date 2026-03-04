# Switchvox .svb Plaintext SIP Credentials

This repository documents research related to plaintext SIP trunk credentials stored within Switchvox `.svb` backup files.

The backup format contains carrier authentication credentials, carrier hostname/IP information, and DID assignments in plaintext. Possession of a backup file may allow extraction of these credentials and authentication directly to the upstream carrier.

This enables impersonation of the PBX endpoint at the carrier level and may allow abuse scenarios such as outbound call fraud, inbound call interception, service disruption, or endpoint impersonation depending on carrier configuration.

Additional technical documentation and reproduction details will be published following completion of coordinated disclosure.
