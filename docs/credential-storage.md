# Example Credential Storage

This document demonstrates how SIP provider authentication credentials appear in plaintext within a Switchvox `.svb` backup.

The example below was generated using a test SIP provider created within the Switchvox administrative interface.

---

## Provider Configuration (GUI)

A test provider was created with the following values:

- **SIP Provider Name:** Test VoIP Provider  
- **Account ID:** testaccount  
- **Password:** test1234!  
- **Hostname/IP Address:** notarealdomain.com  

These values were entered through the Switchvox administrative web interface.

---

## Database Dump Source

After extracting the `.svb` backup archive, the file `DUMP_MINUS_LOGS` contains a PostgreSQL database export of Switchvox configuration data.

Inspection of this dump shows the provider credentials stored directly within several database tables.

---

## Administrative Activity Log

The `admin_activity_log` table records the provider creation event.

Example entry:

```text
submit_add_sip_provider
...
'provider_password' => 'test1234!'
...
```

This shows that the provider password is captured in plaintext within administrative activity records.

---

## Authentication Table

The `ps_auths` table stores authentication credentials used by the system.

Example entry extracted from the dump:

```text
sip_provider_107    ...    password=test1234!    username=testaccount
```

This table contains the authentication credentials used when the PBX registers with the upstream SIP carrier.

---

## SIP Provider Configuration Table

The `sip_providers` table contains provider configuration data, including authentication credentials and carrier connection details.

Example row from the database dump:

```text
107    Test VoIP Provider    testaccount    test1234!    notarealdomain.com
```

This row contains:

- provider name
- authentication username
- plaintext password
- upstream SIP hostname

These values correspond directly to the configuration entered through the administrative interface.

---

## Security Implications

The database dump contained within a Switchvox `.svb` backup therefore exposes all information required to authenticate to the upstream SIP carrier:

- authentication username
- authentication password
- carrier hostname or IP
- trunk configuration context

Because these values are stored in plaintext and are duplicated across multiple tables, extraction of usable carrier credentials from the backup file is straightforward.

Possession of a backup file may therefore allow an attacker to authenticate as the PBX endpoint at the carrier level.
