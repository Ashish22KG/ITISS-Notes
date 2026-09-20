# Security — Day Two — Interview Q&A

### Q1. What is the CIA Triad?
**A:** The CIA Triad is a core cybersecurity model consisting of Confidentiality, Integrity, and Availability. Confidentiality prevents unauthorized access to data, Integrity ensures data is accurate and hasn't been improperly modified, and Availability ensures authorized users can access systems and data when needed.

### Q2. Give a real-world example of the CIA Triad.
**A:** For an online banking system: Confidentiality — only you can access your account; Integrity — your account balance can't be modified by an attacker; Availability — you can access your bank account when you need it.

### Q3. What is a security control?
**A:** A security control is a measure designed to prevent, detect, or respond to security threats and reduce security risks — e.g., firewalls, access control, and MFA (preventive); IDS/SIEM (detective); backup restoration/incident response (corrective); warning banners/policies (deterrent).

### Q4. What is Zero Trust?
**A:** Zero Trust is a security model that assumes no user or device is inherently trusted and requires continuous verification and least-privilege access before granting access to resources — "never trust, always verify."

### Q5. How does Zero Trust differ from the traditional security approach?
**A:** Traditionally, being inside the corporate network implied trust. In Zero Trust, being inside the network does not automatically grant trust — every user's identity, device, permissions, and access request must be verified, with least-privilege access and continuous monitoring.

### Q6. What is an attack surface?
**A:** The collection of all exposed assets, interfaces, services, and vulnerabilities that could potentially be exploited by an attacker — e.g., open network ports/services, servers/endpoints, web apps/APIs, cloud resources, user accounts, mobile apps, third-party integrations.

### Q7. Define threat, vulnerability, and risk, and explain how they relate.
**A:** A threat is a potential danger that can exploit a vulnerability and negatively impact confidentiality, integrity, or availability. A vulnerability is a weakness in a system that can be exploited by a threat. Risk is the potential impact or loss resulting from a threat exploiting a vulnerability, calculated as Risk = Likelihood × Impact. They chain together: Threat → exploits → Vulnerability → creates → Risk.

### Q8. Give an example that ties together threat, vulnerability, and risk.
**A:** An internet-facing server has unpatched software (vulnerability). An attacker exploits it (threat). The result is server compromise, data theft, or service disruption (risk).

### Q9. What is an exploit?
**A:** An exploit is a method or piece of code used by an attacker to leverage a vulnerability and gain unauthorized access or perform malicious actions — e.g., a specially crafted SQL query used to exploit a SQL Injection vulnerability.

### Q10. What is a threat actor? Give examples.
**A:** A threat actor is a person or group responsible for carrying out or attempting a cyberattack. Examples: cybercriminals, hacktivists, insider threats, nation-state groups, and script kiddies.

### Q11. What is an IOC (Indicator of Compromise)?
**A:** An IOC is a piece of forensic evidence that can indicate a security compromise or malicious activity has occurred — e.g., a malicious IP/domain, a file hash, a suspicious executable, unexpected registry modification, or unusual login activity.

### Q12. What does TTP stand for, and what does each part mean?
**A:** TTP stands for Tactics, Techniques, and Procedures — it describes the methods and behaviors used by threat actors to conduct cyberattacks. Tactics = the attacker's goal (e.g., Credential Access); Techniques = the method used (e.g., Credential Dumping); Procedures = the specific implementation/steps (e.g., a specific tool or command).

### Q13. What are security controls, and how are they categorized?
**A:** Security controls are safeguards implemented to prevent, detect, or respond to security threats and reduce security risks. They're commonly categorized as preventive, detective, and corrective controls.

### Q14. Explain preventive, detective, and corrective controls with examples.
**A:** Preventive controls stop or reduce the likelihood of an incident before it occurs (e.g., a firewall blocking unauthorized connections, MFA). Detective controls identify and alert on incidents during or after they occur (e.g., an IDS detecting a port scan). Corrective controls contain, remediate, and recover from an incident (e.g., restoring a ransomware-affected server from a clean backup).

### Q15. Define encryption, hashing, and encoding, and explain how they differ.
**A:** Encryption converts plaintext into ciphertext using an algorithm and key, for confidentiality — reversible with the correct key (e.g., AES, HTTPS). Hashing converts data into a fixed-length hash value using a hash function, for integrity/password storage — one-way, not reversible (e.g., SHA-256, Argon2). Encoding converts data into a standardized format for storage/transmission/processing — reversible without a key, and provides no confidentiality/security (e.g., Base64).

### Q16. Is encoding a security mechanism?
**A:** No. Encoding converts data into a standardized format for transmission or processing, but unlike encryption, it does not provide security or confidentiality.

### Q17. What is a digital signature, and how does it work?
**A:** A digital signature is a cryptographic mechanism that verifies the sender's identity and ensures data hasn't been modified after signing. The sender signs the data with their private key, and the receiver verifies the signature using the sender's public key.

### Q18. What is a digital certificate?
**A:** An electronic credential issued by a trusted Certificate Authority (CA) that binds an entity's identity to its public key — e.g., an HTTPS certificate proves a website's public key belongs to that website's domain.

### Q19. What is PKI, and what are its main components?
**A:** PKI (Public Key Infrastructure) is a framework used to manage digital certificates and public-key cryptography to establish trust and secure communications. Main components: CA (Certificate Authority), RA (Registration Authority), digital certificates, public/private key pairs, and certificate revocation mechanisms.

### Q20. What is MFA, and what are the three common authentication factors?
**A:** MFA (Multi-Factor Authentication) strengthens authentication by requiring two or more independent factors to verify a user's identity: something you know (password/PIN), something you have (phone, hardware token), and something you are (fingerprint, face recognition) — e.g., password + OTP.

### Q21. What is SSO?
**A:** SSO (Single Sign-On) allows a user to authenticate once and access multiple authorized applications without repeatedly entering credentials — e.g., signing in to multiple enterprise apps with one corporate account, commonly using SAML, OAuth 2.0, or OpenID Connect.

### Q22. What is IAM?
**A:** IAM (Identity and Access Management) manages digital identities and controls who can access which resources and what actions they're allowed to perform — including user identity management, authentication, authorization, roles/permissions, access policies, and account lifecycle management.

### Q23. What is secrets management, and give some example tools?
**A:** Secrets management is the practice of securely storing, controlling access to, and rotating sensitive credentials such as API keys, passwords, and tokens (e.g., database passwords, SSH private keys, cloud credentials). Example tools: HashiCorp Vault, AWS Secrets Manager, Azure Key Vault.
