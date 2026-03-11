# Switchvox `.svb` Backup File Format

This document describes the internal structure of Switchvox `.svb` backup files and the process required to extract their contents.

Understanding the archive structure is important because the backup format does not apply encryption or obfuscation to sensitive data, including SIP provider credentials.

---

## File Format Overview

Switchvox backup files use the `.svb` extension but are internally standard compressed archives.

Inspection of the file header shows the following signature:

```text
1f 8b 08
```

This corresponds to the **gzip file format**.

As a result, `.svb` files can be decompressed using standard archive tools without requiring Switchvox software.

---

## Extraction Process

The backup can be extracted using common command-line utilities.

Example:

```bash
mv backup.svb backup.gz
gunzip backup.gz
tar -xf backup
```

After decompression, the archive reveals the internal backup structure.

---

## Archive Layout

The decompressed archive contains a directory named:

```text
backup_package/
```

This directory contains the full system backup.

Example structure:

```text
backup_package/
├── metadata.json
├── HST_PBX_CONFIG*.tar
├── MOH_FILES*.tar
├── PWE_VOICEMAIL_AND*.tar
├── XXX_SSL_CERTS*.tar
├── IAX_RSA_KEYS*.tar
├── ICE_SOUNDS*.tar
└── DUMP_MINUS_LOGS
```

---

## Subsystem Archives

The `backup_package` directory contains multiple subsystem archives.

These are standard tar files containing different components of the Switchvox system.

Examples observed in backups include archives containing:

- PBX configuration
- voicemail messages and greetings
- system prompts
- music-on-hold files
- SSL certificates
- IAX RSA keys
- other subsystem data

Each subsystem archive can be extracted independently using standard archive utilities.

---

## Database Dump

The file:

```text
DUMP_MINUS_LOGS
```

contains a PostgreSQL database export of the Switchvox configuration database.

The name indicates that the dump excludes runtime log tables but includes configuration and operational data required to reconstruct the PBX.

This dump contains multiple configuration tables including those used for SIP provider configuration.

---

## Sensitive Data Exposure

Inspection of the database dump shows that multiple sensitive configuration values are stored in plaintext.

Examples include:

- SIP trunk authentication usernames
- SIP trunk authentication passwords
- carrier hostnames or IP addresses
- DID assignments
- trunk configuration parameters

These values are sufficient to authenticate to the upstream SIP carrier from outside the PBX environment.

Because the backup format does not encrypt or obfuscate this data, any party with access to the backup file can extract these values using standard archive utilities.

---

## Security Implications

The `.svb` backup format contains all configuration information required to recreate the PBX system.

However, because sensitive authentication credentials are stored directly within the backup contents, exposure of a backup file may allow attackers to authenticate to upstream carriers and impersonate the PBX endpoint.

Further details regarding credential exposure are documented in:

```text
docs/credential-storage.md
```
