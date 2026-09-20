# Security — Day Two — Interview Notes

## 1. CIA Triad
Fundamental cybersecurity model defining the three core principles used to protect information and information systems: **Confidentiality, Integrity, Availability**.

| Principle | Meaning | Example |
|---|---|---|
| Confidentiality | Info accessible only to authorized users | Passwords, encryption, access control |
| Integrity | Info stays accurate, complete, unaltered without authorization | Hashing, digital signatures |
| Availability | Systems/info available to authorized users when required | Backups, redundancy, DDoS protection |

**Interview answer:** "The CIA Triad is a core cybersecurity model consisting of Confidentiality, Integrity, and Availability. Confidentiality prevents unauthorized access to data, Integrity ensures that data is accurate and has not been improperly modified, and Availability ensures that authorized users can access systems and data when needed."

**Banking example:**
- Confidentiality — only you can access your account
- Integrity — your balance can't be modified by an attacker
- Availability — you can access your account when you need it

---

## 2. Security Control (definition)
A safeguard or measure implemented to protect systems, networks, applications, and data from security threats and risks.

**Interview answer:** "A security control is a measure designed to prevent, detect, or respond to security threats and reduce security risks."

| Type | Example |
|---|---|
| Preventive | Firewall, access control, MFA |
| Detective | IDS, SIEM, security monitoring |
| Corrective | Backup restoration, incident response |
| Deterrent | Warning banners, security policies |

---

## 3. Zero Trust
Security model based on **"Never trust, always verify."** Requires continuous verification of users, devices, applications, and access requests, regardless of whether they're inside or outside the organization's network.

**Interview answer:** "Zero Trust is a security model that assumes no user or device is inherently trusted and requires continuous verification and least-privilege access before granting access to resources."

**Key principles:**
- Never trust, always verify
- Verify the user's identity
- Verify the device
- Apply least privilege
- Continuously monitor and validate access
- Segment networks and resources

**Traditional vs Zero Trust example:**
- Traditional: "They're inside the network → trust them."
- Zero Trust: "Being inside the network doesn't automatically make them trusted → verify identity, device, permissions, and the access request."

---

## 4. Attack Surface
The total set of possible entry points or attack vectors an attacker could use to gain unauthorized access to a system, network, application, or organization.

**Interview answer:** "Attack surface is the collection of all exposed assets, interfaces, services, and vulnerabilities that could potentially be exploited by an attacker."

**Examples:** open network ports/services, servers/endpoints, web apps/APIs, cloud resources, user accounts/credentials, mobile apps, third-party integrations.

**Example:** A company with 10 internet-facing servers + 5 web apps + 20 open network services + 100 employee accounts — all of these contribute to the attack surface.

---

## 5. Threat, Vulnerability, Risk

**Threat** — Any potential event, actor, or action that can exploit a vulnerability and cause harm to an information system, network, or data.
> "A threat is a potential danger that can exploit a vulnerability and negatively impact the confidentiality, integrity, or availability of a system."
> Example: a hacker attempting to exploit a SQL Injection vulnerability.

**Vulnerability** — A weakness or flaw in a system, application, network, or process that can be exploited by a threat.
> "A vulnerability is a weakness in a system that can be exploited by a threat to compromise security."
> Example: an application that doesn't properly validate SQL input.

**Risk** — The potential for loss or damage when a threat exploits a vulnerability, considering likelihood and impact.
> "Risk is the potential impact or loss resulting from a threat exploiting a vulnerability."
> **Formula:** `Risk = Likelihood × Impact`

**Chain to remember:** Threat → exploits → Vulnerability → creates → Risk
Example: Hacker (Threat) → exploits SQL Injection flaw (Vulnerability) → causes data theft (Risk/Impact).

---

## 6. Exploit, Threat Actor, IOC

**Exploit** — A technique, code, or method used to take advantage of a vulnerability to perform an unauthorized action.
> Example: a specially crafted SQL query used to exploit a SQL Injection vulnerability.

**Threat Actor** — An individual, group, or organization that intentionally performs or attempts malicious activity against a system, network, or organization.
> Examples: cybercriminals, hacktivists, insider threats, nation-state groups, script kiddies.
> Example: a cybercriminal group attempting to deploy ransomware.

**IOC (Indicator of Compromise)** — A piece of evidence or observable artifact indicating a system/network may have been compromised.
> Examples: malicious IP address, malicious domain, file hash, suspicious executable, unexpected registry modification, unusual login activity.
> Example: a known malware hash detected on an endpoint.

---

## 7. TTP & Security Control Categories

**TTP — Tactics, Techniques, and Procedures** — describes how a threat actor conducts an attack, from overall objective to specific steps.
- **Tactics** → the attacker's goal
- **Techniques** → the method used to achieve it
- **Procedures** → the specific implementation/steps

> Example: Tactic = Credential Access; Technique = Credential Dumping; Procedure = using a specific tool/command to extract credentials.

**Security Controls** — safeguards/measures/mechanisms to protect systems, networks, applications, and data from threats; commonly categorized as preventive, detective, and corrective.

| Control | Main Purpose | Examples |
|---|---|---|
| **Preventive** | Stop an incident before it occurs | Firewall, MFA, strong password policy, network segmentation, access control |
| **Detective** | Identify/alert on incidents during or after they occur | IDS, SIEM, security logs, File Integrity Monitoring, security monitoring, CCTV |
| **Corrective** | Restore systems to a secure state, reduce impact after an incident | Restoring from backups, removing malware, patching, resetting compromised credentials, system recovery |

**Easy sequence:** Prevent → Detect → Correct

---

## 8. Encryption vs Hashing vs Encoding

| Feature | Encryption | Hashing | Encoding |
|---|---|---|---|
| Main purpose | Confidentiality | Integrity / password storage | Data representation |
| Reversible | Yes, with key | No | Yes, no key needed |
| Uses key | Yes | No | No |
| Security mechanism | Yes | Yes | No |
| Example | AES | SHA-256, Argon2 | Base64 |

**Encryption** — converts plaintext into ciphertext using an algorithm + key, so only authorized parties can decrypt it. Reversible with the correct key.
> "Encryption protects data confidentiality by converting plaintext into ciphertext using an algorithm and key, which can be reversed through decryption."
> Example: HTTPS encrypts data between browser and server.

**Hashing** — converts data into a fixed-length hash value using a hash function, mainly for integrity verification or password storage. One-way (not reversible).
> "Hashing is a one-way process that converts data into a fixed-length hash value, commonly used for integrity verification and password storage."
> Examples: SHA-256, SHA-3, bcrypt, Argon2.

**Encoding** — converts data from one format into another standardized format for storage/transmission/processing. Reversible without a secret key; provides **no** confidentiality.
> "Encoding converts data into a standardized format for transmission or processing, and unlike encryption, it does not provide security or confidentiality."
> Examples: Base64, ASCII, UTF-8.

---

## 9. Digital Signature, Certificate, PKI, MFA, SSO, IAM, Secrets Management

### Digital Signature
Cryptographic mechanism verifying the authenticity, integrity, and non-repudiation of a digital message/document. Sender signs with their **private key**; receiver verifies with the sender's **public key**.
> "A digital signature is a cryptographic mechanism that verifies the sender's identity and ensures that the data has not been modified after signing."

### Certificate
An electronic document binding an identity to a public key, issued and digitally signed by a trusted Certificate Authority (CA).
> "A digital certificate is an electronic credential issued by a trusted Certificate Authority that binds an entity's identity to its public key."
> Example: an HTTPS certificate proves a website's public key belongs to its domain.

### PKI — Public Key Infrastructure
A framework of people, policies, processes, technologies, certificates, and cryptographic keys used to create, manage, distribute, validate, and revoke digital certificates and public keys.
- Components: CA (Certificate Authority), RA (Registration Authority), digital certificates, public/private key pairs, certificate revocation mechanisms.
> "PKI is a framework used to manage digital certificates and public-key cryptography to establish trust and secure communications."

### MFA — Multi-Factor Authentication
Requires two or more different authentication factors to verify identity.
- Something you know — password/PIN
- Something you have — phone, hardware token
- Something you are — fingerprint, face recognition
> Example: Password + OTP.

### SSO — Single Sign-On
Allows a user to access multiple applications/services using a single set of credentials.
- Common technologies: SAML, OAuth 2.0, OpenID Connect.
> Example: signing in to multiple enterprise apps with one corporate account.

### IAM — Identity and Access Management
Framework for managing digital identities and controlling who can access which resources and what actions they can perform.
- Includes: user identity management, authentication, authorization, roles/permissions, access policies, account lifecycle management.
> Example: giving an employee access to specific AWS resources based on their role.

### Secrets Management
Securely storing, accessing, rotating, and controlling sensitive credentials such as passwords, API keys, tokens, and private keys.
- Examples of secrets: DB passwords, API keys, SSH private keys, app tokens, cloud credentials.
- Example tools: HashiCorp Vault, AWS Secrets Manager, Azure Key Vault.

### Quick revision table
| Concept | Main Purpose | Key Point |
|---|---|---|
| Digital Signature | Authenticity & integrity | Private key signs, public key verifies |
| Certificate | Establish identity | Binds identity to public key |
| PKI | Manage trust & certificates | CA, certificates, keys, revocation |
| MFA | Strong authentication | Requires multiple factors |
| SSO | Simplify authentication | One login → multiple applications |
| IAM | Access control | Who can access what and what they can do |
| Secrets Management | Protect credentials | Secure storage, access, rotation |

---

*See [[02-security.md]] for Day One security notes (network security, attack surface basics, CIA Triad) and [[01-networking.md]] / [[03-devops.md]] for related topics.*
