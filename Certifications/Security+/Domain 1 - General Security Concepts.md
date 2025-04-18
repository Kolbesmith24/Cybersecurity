# Cybersecurity Concepts and Controls

## AAA Framework
**Authentication, Authorization, and Accounting (AAA)** is a framework used to:
- **Authenticate**: Verify a user's identity.
- **Authorize**: Determine what actions or resources a user can access.
- **Account**: Track user activity and access logs.

---

## Types of Security Controls

### Preventive Controls
Designed to stop incidents before they occur.  
**Examples:** Firewalls, access control mechanisms.

### Detective Controls
Used to identify and detect incidents during or after they happen.  
**Examples:** Intrusion detection systems, log monitoring.

### Corrective Controls
Restore systems to normal after an incident.  
**Examples:** System patching, data backups, incident response.

### Deterrent Controls
Discourage malicious actions through visible consequences.  
**Examples:** Warning signs, visible surveillance cameras.

### Directive Controls
Guide and enforce expected behavior and actions.  
**Examples:** Security policies, procedures, training.

### Compensating Controls
Substitute controls used when primary controls are not feasible.  
**Example:** Manual reviews instead of automated scanning.

---

## Categories of Controls

### Technical Controls
Implemented using technology.  
**Examples:** Firewalls, antivirus, encryption.

### Operational Controls
Executed by people as part of procedures.  
**Examples:** Incident response, security training.

### Managerial Controls
Govern organizational security strategies.  
**Examples:** Risk assessments, policy planning.

---

## Risk & Change Management

### Gap Analysis
A method of identifying missing security controls by comparing current and desired security states.

### Change Management
Structured process for managing IT changes securely and efficiently.

#### Technical Implementations:
- Allow/Deny lists  
- Restricted activities  
- Planned downtime  
- Restarts and rollbacks  
- Legacy application considerations  
- Dependency tracking

### Impact Analysis
Assessment of the effects of changes, incidents, or risks on business operations.

### Version Control
Systems that track changes to documents, code, or configurations to maintain integrity and revision history.

---

## Cryptographic Concepts

### Asymmetric Encryption
Uses a **public key** for encryption and a **private key** for decryption.  
**Use Case:** Secure key exchange, digital signatures.

### Symmetric Encryption
Uses the same key for encryption and decryption.  
**Use Case:** Fast encryption for large data volumes.

### Obfuscation
Concealing code or data structure to prevent understanding or reverse engineering.

### Non-repudiation
Ensures a party cannot deny sending or signing a digital message.

### Security Through Obscurity
Relies on hiding system details as a security method (not a recommended sole defense).

---

## NIST Cryptographic Lifecycle
Phases developed by NIST to manage cryptographic algorithm adoption, usage, and retirement.

---

## Data Privacy Techniques

### Deidentification
Removing personal identifiers to prevent re-identification.

### Anonymization
Irreversibly removing data that can link back to individuals.

### HIPAA De-identification Standard
Defines how to de-identify Protected Health Information (PHI) in compliance with HIPAA.

### Data Obfuscation
Altering data values to hide real content while maintaining data structure.

---

## Hashing & Protection

### Hash Function
One-way transformation of data into a fixed-length hash value.

### Rainbow Table Attack
Uses precomputed hash values to reverse-engineer original data.

### Salting
Adds random data before hashing to prevent precomputed attacks like rainbow tables.

---

## Data Protection Techniques

### Tokenization
Replaces sensitive data with non-sensitive tokens.

### Masking
Replaces sensitive data with null or dummy values.

---

## Encryption Algorithms

### DES (Data Encryption Standard)
Outdated symmetric block cipher with short key length.

### Triple DES (3DES)
Stronger than DES; applies encryption three times; being phased out.

### AES (Advanced Encryption Standard)
Widely used, secure symmetric block cipher.

### aescrypt
Linux utility that uses AES to encrypt and decrypt files.

### Blowfish
Legacy symmetric block cipher replaced by Twofish.

### Twofish
Modern symmetric block cipher and successor to Blowfish.

---

## Cryptographic Tools and Protocols

### Steganography
Hiding data within other files (e.g., images).  
**Tool:** OpenStego

### RSA
Asymmetric encryption algorithm for key exchange and digital signatures.

### PGP (Pretty Good Privacy)
Uses a mix of symmetric and asymmetric encryption for secure communication.

#### PGP Process
- Encrypt with the recipient's public key.
- Decrypt with the recipient's private key.

### GnuPG
Linux terminal utility for encryption, decryption, and signing using PGP.

### Elliptic Curve Cryptography (ECC)
Efficient asymmetric encryption using elliptic curves for secure key generation.

### Perfect Forward Secrecy
Ensures that session keys remain secure even if private keys are compromised.

---

## Emerging Threats

### Quantum Computing
A future threat to traditional cryptographic methods due to potential for massive computation power.

---

## Network & Anonymity Concepts

### ZTNA (Zero Trust Network Access)
Security model that:
- Uses a **Control Plane** to make access decisions.
- Uses a **Data Plane** to enforce policies and transport data.

### SASE (Secure Access Service Edge)
Cloud-delivered security framework combining:
- Secure Web Gateway (SWG)
- Cloud Access Security Broker (CASB)
- ZTNA
- VPN

---

## Physical and Procedural Security

### Two-Person Integrity (TPI)
Requires two individuals to complete a task to prevent fraud or error.

### Two-Person Control
Both individuals must be present and agree to perform sensitive operations.

---

## Cyber Deception Techniques

### Darknets
Hidden areas of the internet (e.g., Tor) used for anonymity and threat research.

### Honeypots
Decoy systems designed to attract and analyze attackers.

### Honeynets
A network of honeypots simulating a full environment for analysis.

### Honeytokens
Fake data (e.g., credentials) that trigger alerts when accessed.

### Honeyfiles
Fake files used to detect unauthorized access.

---

## DNS-Based Security

### DNS Sinkhole
Redirects malicious domain requests to a controlled or non-routable IP for mitigation.

# Cryptographic Concepts and Security

## Key Exchange Methods

### In-Band Key Exchange
Key exchange occurs over the same communication channel as the encrypted data.

### Out-of-Band Key Exchange
Key exchange happens over a separate communication channel for added security.

### Diffie-Hellman Key Exchange
A method for securely exchanging cryptographic keys over a public channel to create a secret shared session key.

## Key Management

### Encryption Key Escrow
A system where encryption keys are stored securely for later recovery by authorized parties.

### Recovery Agents
Designated personnel or systems with the ability to recover encryption keys.

## Key Stretching Techniques

### Key Stretching
Enhancing password security by applying hashing and salting repeatedly.

### PBKDF2 (Password-Based Key Derivation Function 2)
A key derivation function that applies salting and repeated hashing to create strong keys.

### bcrypt
A password hashing function that includes a salt and is resistant to brute-force attacks.

## Hardware Security

### Hardware Security Modules (HSMs)
Physical devices that securely generate, manage, and store cryptographic keys.

### FIPS 140-3
The current standard for evaluating the security of cryptographic modules.

### FIPS 140-2 Security Levels
Defines levels of security assurance for cryptographic modules (Levels 1 to 4).

## Public Key Infrastructure (PKI)

### Web of Trust (WOT)
A decentralized model for verifying identities and public keys based on trust relationships.

### PKI (Public Key Infrastructure)
A system for managing digital certificates and public-private key pairs.

### CAs (Certificate Authorities)
Trusted entities that issue and verify digital certificates.

## Hash Functions

### Hash Function
A function that converts input data into a fixed-size string of characters, often used for integrity checks.

### MD5
An outdated hash function known for its vulnerabilities and collision issues.

### Message Digest / Hash
The output of a hash function used to verify data integrity.

### SHA (Secure Hash Algorithm)
Includes SHA-1 (insecure), SHA-2 (widely used), and SHA-3 (most recent).

### RIPEMD
A family of hash functions developed as alternatives to MD5 and SHA.

### HMAC (Hash-based Message Authentication Code)
A hash combined with a secret key to verify both data integrity and authenticity.

## Digital Signatures

### Digital Signatures
Cryptographic signatures that use private keys for signing and public keys for verification.

### Digital Signature Standard (DSS)
A federal standard that specifies algorithms for creating digital signatures.

### DSS Algorithms
Approved digital signature algorithms: RSA, ECDSA, and EDDSA.

## Certificates

### X.509 Certificates
A standard for digital certificates used in PKI.

### Certificate Signing Request (CSR)
A request sent from an applicant to a CA to apply for a digital certificate.

### Certificate Attributes
Includes fields like Subject, Issuer, Validity, SANs (Subject Alternative Names), and OIDs (Object Identifiers).

### OpenSSL (Linux Command)
Command-line toolkit for working with SSL/TLS and cryptographic operations.

### Certificate Revocation
The process of invalidating a certificate using CRL (Certificate Revocation List) or OCSP (Online Certificate Status Protocol).

### Certificate Stapling
A method where the server "staples" the OCSP response to the certificate to speed up verification.

### Digital Certificate Issuance
Process involving identity verification from CA of requestor, certificate creation by CA, CA signs the digital cert with the CA's private key, CA sends back to requestor, and requestor can verify with the CA’s public key.

### Self-Signed Certificates
Certificates signed by the issuer themselves, not by a trusted CA.

### Certificate Chaining
A chain of trust that connects a certificate to a trusted root certificate through intermediate certificates.

### Viewing a Site's Certificate
Click the lock icon next to a website's URL and view certificate details.

### Offline Certificate Authorities
CAs that are kept offline to protect root keys from compromise.

### Certificate Subject
The entity (person, organization, server, or device) identified by a certificate.

### OIDs in Certificates
(Object Identifiers) Numerical identifiers used to define certificate attributes and extensions.

### Certificate Types by Subject
Certificates can be issued for servers, devices, individuals, or application developers.

### Certificate Pinning
A method that binds a certificate to a domain for a fixed period to prevent impersonation.

### Root Certificates
Top-level certificates in a chain of trust, typically offline for security.

### Wildcard Certificates
Certificates that apply to all subdomains under a domain (e.g., `*.linkedin.com`).

### Types of Certificates Issued by CAs
- **Domain Validation**: Lowest level, verifies domain ownership.
- **Organizational Validation**: Medium level, verifies business name.
- **Extended Validation**: Highest level.

### Digital Certificate Formats
- **DER**: Binary format.
- **PEM**: ASCII format of DER.
- **PFX**: Binary, includes private keys.
- **P7B**: ASCII format of PFX.

## Encryption Protocols

### TLS (Transport Layer Security)
A protocol that encrypts communication over a network using a TLS handshake.

### TLS Handshake
A negotiation between client and server to establish an encrypted session using digital certificates and a shared session key.

### SSL (Secure Sockets Layer)
Predecessor to TLS; now considered insecure and deprecated.

## Blockchain

### Blockchain
A distributed and immutable ledger used for secure, transparent record-keeping.
