# Access Control Concepts

## AAA Framework

- **Authentication**: Verifies identity.
- **Authorization**: Determines access rights.
- **Accounting**: Tracks user activity.

---

# Types of Security Controls

## By Purpose

- **Preventive Controls**: Stop incidents (e.g., firewalls).
- **Detective Controls**: Identify incidents (e.g., IDS, log monitoring).
- **Corrective Controls**: Fix after an incident (e.g., backups, patching).
- **Deterrent Controls**: Discourage threats (e.g., security signs, cameras).
- **Directive Controls**: Mandate behavior (e.g., policies, training).
- **Compensating Controls**: Alternative controls when primary isn't possible.

## By Implementation

- **Technical Controls**: Tech-based (e.g., encryption, antivirus).
- **Operational Controls**: Human-based processes (e.g., training).
- **Managerial Controls**: Risk management (e.g., risk assessments).

---

# Security Frameworks & Models

## ZTNA (Zero Trust Network Access)

- **Control Plane**: Makes access decisions.
- **Data Plane**: Enforces access and carries traffic.

## SASE (Secure Access Service Edge)

- Combines security and networking in a cloud service (includes ZTNA, SWG, CASB, VPN).

---

# Personnel & Procedural Security

- **Two-Person Integrity (TPI)**: Two individuals for critical actions.
- **Two-Person Control**: Both must be present/agree (e.g., safe access).
- **Change Management**: Structured process to manage IT changes.
- **Impact Analysis**: Assessing the consequences of incidents or changes.

---

# Security Monitoring Tools

- **Honeytokens**: Fake credentials/files to detect intrusions.
- **Honeyfiles**: Decoy files placed on systems.
- **Honeypots**: Fake vulnerable systems for attacker analysis.
- **Honeynets**: Network of honeypots.
- **DNS Sinkhole**: Redirect malicious traffic to a safe endpoint.
- **Darknets**: Hidden parts of the internet (e.g., Tor).

---

# Encryption & Cryptography

## Types of Encryption

- **Symmetric**: Same key for encryption/decryption (e.g., AES).
- **Asymmetric**: Public key for encryption, private key for decryption (e.g., RSA).
- **PGP**: Uses both for secure communication.

## Common Algorithms

- **DES / Triple DES**: Obsolete.
- **AES**: Modern, secure.
- **Blowfish / Twofish**: Symmetric ciphers.
- **ECC**: Small, efficient asymmetric encryption.
- **RSA**: Asymmetric encryption & digital signatures.

## Cryptographic Concepts

- **Obfuscation**: Making data/code unclear.
- **Non-repudiation**: Can’t deny sending a message.
- **Perfect Forward Secrecy**: Compromised keys don’t expose past sessions.
- **Steganography**: Hiding data within other media.

## Key Exchange

- **In-Band**: Over the same channel.
- **Out-of-Band**: Over a different channel.
- **Diffie-Hellman**: Securely shares session keys.

## TLS Handshake Process

1. Client requests TLS.
2. Server responds with cipher and digital certificate.
3. Client validates cert via CA.
4. Client encrypts session key with server’s public key.
5. Server decrypts it with private key.
6. Secure session established using session key.

---

# Key Management

- **Key Escrow**: Secure key backup by a third party.
- **Recovery Agents**: Restore encrypted data.
- **Key Stretching**: Makes weak passwords stronger (e.g., PBKDF2, bcrypt).
- **Hardware Security Modules (HSMs)**: Secure key management hardware.
- **FIPS 140-3**: Standard for evaluating crypto modules (replaces FIPS 140-2).

---

# Hashing & Integrity

- **Hash Functions**: One-way functions to verify integrity (e.g., MD5, SHA, RIPEMD).
- **Message Digest**: The hash value itself.
- **HMAC**: Combines a hash with a secret key.
- **Rainbow Table Attack**: Uses precomputed hashes.
- **Salting**: Adds randomness to defeat rainbow tables.

---

# Data Protection Techniques

- **Tokenization**: Replace data with non-sensitive tokens.
- **Masking**: Hide sensitive parts of data.
- **Anonymization**: Irreversible removal of identifiers.
- **Deidentification**: Mask/removes identifiers but may be reversible.
- **Data Obfuscation**: Scrambles data while retaining format.

---

# Digital Certificates & PKI

## PKI (Public Key Infrastructure)

- Manages digital certificates.
- **CAs (Certificate Authorities)**: Issue and verify certs.
- **Digital Signatures**: Ensure authenticity using private keys.
- **X.509**: Standard for digital certs.
- **DSS Algorithms**: RSA, ECDSA, EDDSA.
- **CSR (Certificate Signing Request)**: Sent to a CA to request a certificate.

### Certificate Management

- **Revocation**: CRL or OCSP.
- **Stapling**: Speeds up OCSP checks.
- **Self-Signed**: Not from a CA.
- **Certificate Chaining**: Trust chain from intermediate to root.
- **Certificate Pinning**: Ties a cert to a domain.
- **Offline CAs**: Secure root certificates.

### Certificate Attributes

- **Subject**, **Issuer**, **Validity**, **OIDs**, **SANs**, etc.

### Types of Certificates

- **Domain Validation (DV)**: Basic.
- **Organization Validation (OV)**: Verifies organization.
- **Extended Validation (EV)**: Highest trust.

### Formats

- **DER**: Binary.
- **PEM**: ASCII of DER.
- **PFX**: Binary, includes private key.
- **P7B**: ASCII of PFX.

---

# TLS/SSL

- **TLS**: Secure protocol, replaces SSL.
- **SSL**: Deprecated, insecure.
- **OpenSSL**: Linux tool for SSL/TLS and cryptography.

---

# Blockchain

- Distributed, immutable ledger.
- Useful for transparent record-keeping (e.g., supply chains, birth certs).

