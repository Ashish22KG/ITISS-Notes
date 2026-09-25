# Windows + Active Directory — Interview Definitions & Answers

Windows and Active Directory are **very important for enterprise security, SOC, Blue Team, Red Team, IAM, and security engineering roles**.

This guide covers Windows fundamentals, Active Directory architecture, authentication protocols, common enterprise services, and major AD attack concepts.

---

# PART 1 — WINDOWS FUNDAMENTALS

## 1. Windows Architecture

**Definition:**  
Windows architecture is the structure of Windows components that work together to provide applications, system services, security, memory management, hardware interaction, and user interaction.

**Major components:**

- User Mode
- Kernel Mode
- Windows Executive
- Kernel
- Hardware Abstraction Layer (HAL)
- Device Drivers
- System Services
- Security components

### User Mode

Applications and many services run in user mode with restricted privileges.

### Kernel Mode

Core operating-system components and drivers run in kernel mode with highly privileged access.

**Interview Answer:**  
"Windows uses a layered architecture broadly divided into user mode and kernel mode. Applications generally run in user mode, while the Windows kernel, executive components, and drivers operate in kernel mode. This separation helps isolate applications from highly privileged system operations."

**Security Relevance:**  
Privilege boundaries are important because compromising highly privileged components can provide extensive control over the system.

---

# 2. Windows Users

**Definition:**  
A Windows user account represents an identity that can authenticate to a Windows system and receive permissions to access resources.

**Types include:**

- Local users
- Domain users
- Built-in administrative accounts
- Service accounts

**Useful commands:**

```powershell
whoami
whoami /user
net user
Get-LocalUser
```

**Interview Answer:**  
"A Windows user account represents an identity. The user's permissions determine which files, services, applications, and network resources the account can access."

---

# 3. Windows Groups

**Definition:**  
A Windows group is a collection of user or computer accounts used to simplify permission and access management.

**Examples:**

- Administrators
- Users
- Remote Desktop Users
- Domain Admins
- Domain Users

**Interview Answer:**  
"Groups simplify access control by allowing administrators to assign permissions to a group instead of individually configuring every user. In Active Directory, security groups are heavily used for role-based access control."

**Security Relevance:**  
Excessive group membership, especially membership in highly privileged groups, can create privilege-escalation risks.

---

# 4. Windows Services

**Definition:**  
A Windows service is a background process designed to perform a system or application function, often without direct user interaction.

**Examples:**
- Windows Update
- DNS Client
- Windows Defender services
- Print Spooler
- Active Directory Domain Services

**Useful PowerShell commands:**

```powershell
Get-Service
Get-Service -Name Spooler
Start-Service <service>
Stop-Service <service>
Restart-Service <service>
```

**Interview Answer:**  
"A Windows service is a background component that performs a specific function. Services can start automatically during boot, on demand, or according to configured triggers."

**Security Relevance:**  
Unexpected services, insecure service permissions, or malicious services can be indicators of persistence or privilege escalation.

---

# 5. Windows Processes

**Definition:**  
A process is a running instance of a program.

**Important concepts:**

- PID — Process ID
- Parent process
- Command line
- User/account context
- CPU and memory usage

**Commands:**

```powershell
Get-Process
tasklist
tasklist /svc
```

**Interview Answer:**  
"A Windows process is a running instance of an executable. During security analysis, I would examine its PID, parent process, command line, user context, network connections, and behavior."

**Security Relevance:**  
Process analysis can reveal malware, suspicious PowerShell activity, credential theft, or abnormal parent-child process relationships.

---

# 6. Windows Registry

**Definition:**  
The Windows Registry is a hierarchical database that stores configuration information used by Windows and applications.

**Important root keys:**

- `HKEY_LOCAL_MACHINE` (HKLM)
- `HKEY_CURRENT_USER` (HKCU)
- `HKEY_CLASSES_ROOT` (HKCR)
- `HKEY_USERS` (HKU)
- `HKEY_CURRENT_CONFIG` (HKCC)

**Interview Answer:**  
"The Windows Registry stores system, user, application, hardware, and configuration information. Security analysts can examine registry data during troubleshooting and incident response."

**Security Relevance:**  
Attackers can abuse specific registry locations for persistence, configuration changes, or defense evasion.

---

# 7. Event Viewer

**Definition:**  
Event Viewer is a Windows administrative tool used to view and analyze event logs generated by the operating system and applications.

**Common logs:**

- Application
- Security
- System
- Setup
- Forwarded Events

**Interview Answer:**  
"Event Viewer provides access to Windows event logs. Security analysts can use these logs to investigate authentication, process, service, policy, and system events."

**Security Relevance:**  
Event logs are important for detecting suspicious logins, privilege changes, account modifications, and other security activity.

---

# 8. Windows Firewall

**Definition:**  
Windows Firewall is a host-based firewall that controls inbound and outbound network traffic according to configured rules.

**Interview Answer:**  
"Windows Firewall provides host-level network access control. Rules can control traffic based on direction, application, protocol, port, profile, and other conditions."

**Security Relevance:**  
It can reduce exposure by blocking unnecessary inbound services and restricting network communication.

---

# 9. PowerShell

**Definition:**  
PowerShell is Microsoft's command-line shell and scripting/automation framework for Windows and other platforms.

**Examples:**

```powershell
Get-Process
Get-Service
Get-EventLog
Get-ChildItem
Get-LocalUser
Get-NetTCPConnection
```

**Interview Answer:**  
"PowerShell is an object-oriented command-line shell and scripting environment used for administration and automation. Security teams use it for system administration, investigation, and automation."

**Security Relevance:**  
PowerShell is also frequently monitored because attackers may abuse legitimate PowerShell capabilities for execution, discovery, and automation.

---

# 10. Windows Authentication

**Definition:**  
Windows authentication is the process of verifying the identity of a user or computer before granting access to resources.

Common mechanisms include:

- Kerberos
- NTLM
- Certificate-based authentication
- Smart cards
- Windows Hello for Business

**Interview Answer:**  
"Windows environments can use different authentication mechanisms depending on the environment and resource. In modern Active Directory domains, Kerberos is generally the preferred protocol for domain authentication, while NTLM remains available for compatibility and specific scenarios."

---

# 11. NTLM

**Definition:**  
NTLM (New Technology LAN Manager) is a Microsoft authentication protocol based on challenge-response mechanisms.

**Interview Answer:**  
"NTLM is an older Windows authentication protocol that uses challenge-response authentication. It is retained for compatibility but has security limitations compared with Kerberos and is associated with attacks such as pass-the-hash and NTLM relay under vulnerable configurations."

**Security Relevance:**
- Pass-the-Hash
- NTLM relay
- Credential exposure
- Legacy authentication risks

---

# 12. Kerberos

**Definition:**  
Kerberos is a ticket-based network authentication protocol used extensively by Active Directory.

**Main components:**

- Client
- Key Distribution Center (KDC)
- Authentication Service (AS)
- Ticket Granting Service (TGS)
- Service

**Interview Answer:**  
"Kerberos provides ticket-based authentication. A user authenticates to the domain's authentication service and obtains a Ticket Granting Ticket, or TGT. The client can then request service tickets from the TGS to access permitted services."

**Security Relevance:**
- Kerberoasting
- Pass-the-Ticket
- Golden Ticket
- Silver Ticket
- Ticket theft

---

# 13. Windows Logs

**Definition:**  
Windows logs are records of operating-system, application, security, authentication, and system activity.

**Important sources:**

- Windows Security log
- System log
- Application log
- PowerShell logs
- Sysmon logs, if deployed
- Microsoft-Windows-* event channels

**Interview Answer:**  
"Windows logs provide evidence of system and user activity. In a SOC environment, analysts correlate authentication, process, network, PowerShell, and account-management events to detect suspicious behavior."

---

# PART 2 — ACTIVE DIRECTORY

# 14. Active Directory

**Definition:**  
Active Directory Domain Services (AD DS) is Microsoft's directory service for managing identities, computers, authentication, authorization, and resources in a Windows domain environment.

**Interview Answer:**  
"Active Directory centrally manages users, computers, groups, policies, authentication, and access to resources in an enterprise Windows environment."

---

# 15. Domain

**Definition:**  
An Active Directory domain is a logical administrative and security boundary containing users, computers, groups, and other directory objects.

**Example:**

```text
corp.example.com
```

**Interview Answer:**  
"A domain is a logical environment in Active Directory where identities, computers, policies, and resources are centrally managed."

---

# 16. Domain Controller

**Definition:**  
A Domain Controller (DC) is a Windows Server system running Active Directory Domain Services that authenticates users and computers and provides directory services.

**Main functions:**

- Authentication
- Authorization support
- Directory storage
- Kerberos
- LDAP
- Group Policy processing support
- Replication with other DCs
- DNS integration in typical AD deployments

**Interview Answer:**  
"A Domain Controller hosts AD DS and provides authentication and directory services. It stores directory information and participates in replication with other domain controllers."

---

# 17. Forest

**Definition:**  
An Active Directory forest is the highest-level logical container in an AD environment and can contain one or more domains.

**Interview Answer:**  
"A forest is the top-level Active Directory security and directory structure. It can contain multiple domains that share a common schema and configuration and have built-in trust relationships."

---

# 18. Tree

**Definition:**  
An Active Directory tree is a collection of one or more domains that share a contiguous DNS namespace.

**Example:**

```text
example.com
|
+-- india.example.com
|
+-- us.example.com
```

**Interview Answer:**  
"An AD tree contains domains that share a contiguous DNS namespace. Multiple trees can exist within the same forest."

---

# 19. Organizational Unit (OU)

**Definition:**  
An Organizational Unit is a container in Active Directory used to organize objects and apply Group Policy.

**Examples:**

```text
Company
|
+-- Users
+-- Computers
+-- Servers
+-- HR
+-- IT
```

**Interview Answer:**  
"An OU organizes users, computers, and other objects in Active Directory. OUs are especially useful for administrative delegation and applying Group Policy."

---

# 20. AD Users

**Definition:**  
AD user objects represent identities stored in the Active Directory database.

**Examples of attributes:**

- Username
- Display name
- Email
- Group membership
- User principal name (UPN)
- Security identifiers

**Interview Answer:**  
"AD user objects represent domain identities. Their attributes and group memberships determine how they authenticate and what resources they can access."

---

# 21. AD Groups

**Definition:**  
AD groups are collections of users, computers, or other groups used to simplify access management.

**Important group scopes:**

- Domain Local
- Global
- Universal

**Common group types:**

- Security groups
- Distribution groups

**Interview Answer:**  
"AD groups simplify access control. Instead of assigning permissions individually, organizations can assign permissions to security groups and add appropriate users or computers to those groups."

---

# 22. Group Policy Object (GPO)

**Definition:**  
A Group Policy Object is a collection of configuration settings that administrators can apply to users and computers in an Active Directory environment.

**Examples:**
- Password policies
- Account lockout policies
- Windows Firewall settings
- Software configuration
- Security settings
- PowerShell policies

**Interview Answer:**  
"GPOs allow centralized configuration and security management of domain users and computers. They can be linked to sites, domains, or OUs."

**Security Relevance:**  
GPOs are powerful because they can configure security controls across many systems. Unauthorized modification of GPOs can have a major security impact.

---

# 23. LDAP

**Definition:**  
LDAP (Lightweight Directory Access Protocol) is used to query and manage directory information.

**Common ports:**

- 389 — LDAP
- 636 — LDAP over TLS

**Interview Answer:**  
"LDAP provides access to directory information stored in services such as Active Directory. Applications can use LDAP to query users, groups, computers, and other directory objects."

---

# 24. Kerberos in Active Directory

**Definition:**  
Kerberos is the primary authentication protocol used by modern Active Directory domains.

**Basic flow:**

```text
User
  |
  | Authentication
  v
KDC / Domain Controller
  |
  | TGT
  v
Client
  |
  | Service Ticket Request
  v
KDC
  |
  | Service Ticket
  v
Target Service
```

**Interview Answer:**  
"In AD, Kerberos enables ticket-based authentication. A user first obtains a TGT and then requests service tickets to access specific services."

---

# 25. NTLM in Active Directory

**Definition:**  
NTLM is a legacy authentication mechanism that can still be used in Windows environments when Kerberos is unavailable or certain compatibility scenarios require it.

**Interview Answer:**  
"Although Kerberos is preferred in domain environments, NTLM may still be used in some scenarios. Security teams monitor and reduce unnecessary NTLM usage because it has weaker security properties and is associated with attacks such as relay and pass-the-hash."

---

# 26. SPN

**Definition:**  
A Service Principal Name (SPN) is a unique identifier for a service instance associated with an account in Active Directory.

**Example format:**

```text
HTTP/webserver.example.com
MSSQLSvc/sqlserver.example.com:1433
```

**Interview Answer:**  
"An SPN identifies a service instance for Kerberos authentication. The SPN allows a client to request a Kerberos service ticket for the appropriate service account."

**Security Relevance:**  
Service accounts with SPNs can be targeted in Kerberoasting because their associated service tickets can be requested by authenticated domain users.

---

# 27. SMB

**Definition:**  
SMB (Server Message Block) is a network protocol used for file and printer sharing and other Windows network services.

**Common Port:**

```text
445/TCP
```

**Interview Answer:**  
"SMB is widely used in Windows environments for file and printer sharing and other network operations. It is an important protocol for enterprise administration and is also a major security monitoring point."

**Security Relevance:**
- Lateral movement
- Credential abuse
- File-share exposure
- Relay attacks in vulnerable configurations
- Legacy SMB vulnerabilities

---

# 28. DNS in Active Directory

**Definition:**  
DNS is a critical part of Active Directory because domain clients use DNS to locate domain controllers and services.

**Interview Answer:**  
"Active Directory relies heavily on DNS. Clients use DNS records, including SRV records, to locate domain controllers and services such as LDAP and Kerberos."

**Security Relevance:**  
Incorrect DNS configuration can cause authentication and domain-service failures. DNS manipulation can also have security implications.

---

# 29. Domain Trusts

**Definition:**  
A domain trust is a relationship that allows authentication or resource access between different AD domains or forests according to configured trust direction and permissions.

**Interview Answer:**  
"Trusts allow identities from one domain or forest to access resources in another domain or forest when the appropriate permissions exist. Trusts can be one-way or two-way and may be transitive or non-transitive depending on the trust type."

**Security Relevance:**  
Poorly designed or excessive trust relationships can expand the attack surface and enable attackers to move across security boundaries.

---

# PART 3 — ACTIVE DIRECTORY ATTACK CONCEPTS

These concepts should be understood from a **defensive and authorized security-testing perspective**.

---

# 30. Kerberoasting

**Definition:**  
Kerberoasting is an attack technique in which an authenticated domain user requests Kerberos service tickets for accounts associated with service principal names and attempts to crack the ticket's encrypted material offline to recover the service account password.

**Interview Answer:**  
"Kerberoasting targets service accounts associated with SPNs. An attacker who can request service tickets may obtain ticket material that can be attacked offline. Weak service-account passwords increase the risk."

**Security Controls:**
- Use strong, unique service-account passwords
- Prefer managed service accounts where appropriate
- Minimize unnecessary SPNs
- Monitor unusual service-ticket requests
- Apply least privilege to service accounts

---

# 31. AS-REP Roasting

**Definition:**  
AS-REP Roasting targets accounts that have Kerberos preauthentication disabled. An attacker can request authentication material that may be attacked offline to recover weak account passwords.

**Interview Answer:**  
"AS-REP Roasting targets accounts configured without Kerberos preauthentication. If the account has a weak password, the returned authentication material may be cracked offline."

**Security Controls:**
- Keep Kerberos preauthentication enabled unless there is a justified requirement
- Use strong passwords
- Monitor unusual authentication requests
- Audit accounts with preauthentication disabled

---

# 32. Pass-the-Hash

**Definition:**  
Pass-the-Hash is a technique where an attacker uses a captured NTLM password hash to authenticate without knowing the plaintext password.

**Interview Answer:**  
"Pass-the-Hash abuses NTLM authentication by using a stolen password hash as an authentication credential. The attacker does not necessarily need to recover the original password."

**Security Controls:**
- Reduce unnecessary NTLM usage
- Use privileged access controls
- Apply least privilege
- Protect administrator credentials
- Use endpoint security monitoring
- Segment administrative access

---

# 33. Pass-the-Ticket

**Definition:**  
Pass-the-Ticket is a technique where an attacker uses a stolen Kerberos ticket to authenticate to services without obtaining the user's plaintext password.

**Interview Answer:**  
"Pass-the-Ticket abuses stolen Kerberos tickets. If an attacker obtains a valid ticket, they may use it to access services for which the ticket is valid until it expires or is otherwise invalidated."

**Security Controls:**
- Protect privileged accounts
- Monitor unusual Kerberos activity
- Use endpoint detection
- Limit credential exposure
- Apply privileged access management

---

# 34. NTLM Relay

**Definition:**  
NTLM relay is an attack in which an attacker relays NTLM authentication from one system to another service instead of directly cracking the credentials.

**Interview Answer:**  
"NTLM relay abuses the challenge-response authentication process by forwarding authentication to another service that accepts NTLM. The attacker may gain access or perform actions with the relayed identity if protections are not in place."

**Security Controls:**
- Reduce or disable unnecessary NTLM
- Enable SMB signing where appropriate
- Use Extended Protection for Authentication where supported
- Use LDAP signing/channel binding where appropriate
- Segment networks
- Monitor relay-related authentication patterns

---

# 35. Credential Dumping

**Definition:**  
Credential dumping is the unauthorized extraction of authentication material such as password hashes, cached credentials, or authentication tokens from a system.

**Interview Answer:**  
"Credential dumping attempts to obtain authentication material from operating-system components or applications. Attackers can use stolen credentials or hashes for privilege escalation and lateral movement."

**Security Controls:**
- Endpoint Detection and Response
- Credential Guard where appropriate
- Least privilege
- Restrict administrative access
- Protect LSASS
- Monitor suspicious credential-access behavior

---

# 36. Lateral Movement

**Definition:**  
Lateral movement is the process of moving from one compromised system or account to other systems within an environment.

**Common mechanisms attackers may abuse:**
- SMB
- RDP
- WinRM
- Remote services
- PsExec-like administrative mechanisms
- Stolen credentials/tickets

**Interview Answer:**  
"Lateral movement occurs after initial compromise when an attacker attempts to access additional systems or accounts. In an AD environment, stolen credentials, administrative protocols, and excessive trust can facilitate movement."

**Security Controls:**
- Network segmentation
- Least privilege
- MFA where applicable
- Privileged access management
- Endpoint monitoring
- Restrict administrative protocols

---

# 37. Privilege Escalation

**Definition:**  
Privilege escalation is gaining permissions beyond those originally assigned to an account or process.

**Types:**

### Vertical Privilege Escalation

Moving from a lower privilege level to a higher one.

Example:

```text
Standard User
     ↓
Administrator
```

### Horizontal Privilege Escalation

Accessing another user's resources without necessarily gaining higher system privileges.

**Interview Answer:**  
"Privilege escalation occurs when an attacker gains permissions beyond their original authorization. In Windows environments, this may involve vulnerable services, excessive permissions, credential abuse, misconfigured GPOs, or other weaknesses."

---

# 38. BloodHound Concepts

**Definition:**  
BloodHound is a security-analysis tool that models relationships in Active Directory and helps identify attack paths based on privileges, group memberships, sessions, trusts, and other relationships.

**Important concepts:**

- Users
- Groups
- Computers
- Sessions
- Group membership
- ACLs
- Local administrator relationships
- Domain trusts
- GPO relationships
- Attack paths

**Interview Answer:**  
"BloodHound represents Active Directory relationships as a graph. Security teams can use it to identify excessive privileges and potential attack paths, while authorized red teams can use it to understand how an attacker might move through the environment."

**Security Relevance:**  
It is especially useful for identifying:

```text
Low-privileged user
       ↓
Group membership
       ↓
Computer access
       ↓
Local administrator
       ↓
Privileged account
       ↓
Domain-level impact
```

The exact path depends on the environment's permissions and relationships.

---

# PART 4 — WINDOWS + AD RAPID-FIRE INTERVIEW QUESTIONS

## Q1. What is Active Directory?

**Answer:**  
"Active Directory Domain Services is Microsoft's directory service for centrally managing identities, computers, authentication, authorization, policies, and resources in an enterprise Windows environment."

---

## Q2. What is a Domain Controller?

**Answer:**  
"A Domain Controller is a Windows Server running AD DS. It provides directory services and authentication and stores Active Directory data."

---

## Q3. What is the difference between a domain and a forest?

**Answer:**  
"A domain is a logical AD environment containing objects such as users and computers. A forest is the highest-level AD structure and can contain one or more domains."

---

## Q4. What is an OU?

**Answer:**  
"An Organizational Unit is a container used to organize AD objects and support administrative delegation and Group Policy application."

---

## Q5. What is GPO?

**Answer:**  
"A Group Policy Object contains centralized configuration settings that can be applied to users and computers through Active Directory."

---

## Q6. Kerberos vs NTLM?

**Answer:**  
"Kerberos is a modern ticket-based authentication protocol and is the preferred authentication mechanism in typical AD domain environments. NTLM is an older challenge-response protocol retained for compatibility and specific scenarios."

---

## Q7. What is an SPN?

**Answer:**  
"An SPN uniquely identifies a service instance for Kerberos authentication and maps that service to an account in Active Directory."

---

## Q8. Why is DNS important in Active Directory?

**Answer:**  
"AD depends heavily on DNS for locating domain controllers and services. Clients use DNS records, particularly SRV records, to locate services such as Kerberos and LDAP."

---

## Q9. What is Kerberoasting?

**Answer:**  
"Kerberoasting targets Kerberos service accounts associated with SPNs by obtaining service tickets and attempting offline password cracking. Strong service-account passwords and managed service accounts help mitigate the risk."

---

## Q10. What is AS-REP Roasting?

**Answer:**  
"AS-REP Roasting targets accounts where Kerberos preauthentication is disabled. Authentication material can potentially be obtained and attacked offline if the account uses a weak password."

---

## Q11. What is Pass-the-Hash?

**Answer:**  
"Pass-the-Hash uses a stolen NTLM hash as an authentication credential without requiring the plaintext password."

---

## Q12. What is Pass-the-Ticket?

**Answer:**  
"Pass-the-Ticket uses a stolen Kerberos ticket to authenticate to services without obtaining the user's plaintext password."

---

## Q13. What is NTLM relay?

**Answer:**  
"NTLM relay forwards NTLM authentication to another service. If the target service accepts the relayed authentication and appropriate protections are absent, the attacker may perform actions as the victim account."

---

## Q14. What is lateral movement?

**Answer:**  
"Lateral movement is the process of moving from one compromised system or account to other systems in the environment using available credentials, remote services, or other access mechanisms."

---

## Q15. What is credential dumping?

**Answer:**  
"Credential dumping is extracting authentication material such as password hashes, cached credentials, or tokens from a compromised system."

---

## Q16. What is BloodHound?

**Answer:**  
"BloodHound is a graph-based AD analysis tool that maps relationships such as group membership, permissions, sessions, trusts, and administrative access to identify potential attack paths."

---

# PART 5 — SECURITY MONITORING: WHAT TO WATCH

For a SOC or Blue Team interview, connect AD concepts with telemetry.

| Activity | Useful Security Data |
|---|---|
| User login | Windows Security events |
| Failed login | Windows Security events |
| Group membership change | Security audit events |
| New account | Security audit events |
| Process execution | Sysmon / EDR / process telemetry |
| PowerShell execution | PowerShell logs / EDR |
| Service creation | Windows event logs / EDR |
| Kerberos activity | Domain Controller security logs |
| NTLM activity | Authentication logs / network telemetry |
| SMB activity | Network and endpoint telemetry |
| GPO modification | AD and Windows audit logs |
| Privileged activity | Security logs / EDR / IAM telemetry |
| Lateral movement | Authentication + endpoint + network telemetry |

---

# PART 6 — WINDOWS + AD ATTACK CHAIN

A useful interview mental model is:

```text
Initial Access
      ↓
Execution
      ↓
Credential Access
      ↓
Privilege Escalation
      ↓
Discovery
      ↓
Lateral Movement
      ↓
Persistence
      ↓
Domain-Level Impact
```

Example conceptual chain:

```text
Compromised User
       ↓
Credential Access
       ↓
Identify AD Relationships
       ↓
Privilege Escalation
       ↓
Lateral Movement
       ↓
Compromise Privileged Account
       ↓
Potential Domain-Level Impact
```

**Interview Answer:**  
"In an AD environment, I would think in terms of attack paths rather than isolated vulnerabilities. A low-privileged compromise becomes more serious if the attacker can obtain credentials, discover privileged relationships, move laterally, and eventually reach highly privileged accounts."

---

# PART 7 — IMPORTANT WINDOWS COMMANDS

| Command | Purpose |
|---|---|
| `whoami` | Show current identity |
| `whoami /all` | Show identity, groups, privileges, and claims |
| `whoami /user` | Show current user's SID |
| `hostname` | Show computer name |
| `ipconfig /all` | Show network configuration |
| `tasklist` | List processes |
| `tasklist /svc` | Show services associated with processes |
| `net user` | Manage/view local users |
| `net localgroup` | Manage/view local groups |
| `Get-Process` | PowerShell process information |
| `Get-Service` | PowerShell service information |
| `Get-LocalUser` | List local users |
| `Get-LocalGroup` | List local groups |
| `Get-NetTCPConnection` | Show TCP connections |
| `Get-WinEvent` | Query Windows event logs |
| `gpresult` | Show applied Group Policy information |
| `nslookup` | DNS lookup |
| `Test-NetConnection` | Test network connectivity |
| `netstat` | Display network connections |
| `sc.exe` | Manage/query Windows services |

---

# PART 8 — IMPORTANT AD CONCEPTS TO MEMORIZE

```text
Active Directory
│
├── Forest
│   │
│   ├── Tree
│   │   │
│   │   ├── Domain
│   │   │   ├── Users
│   │   │   ├── Groups
│   │   │   ├── Computers
│   │   │   └── OUs
│   │   │
│   │   └── Domain
│   │
│   └── Trusts
│
├── Domain Controllers
│   ├── LDAP
│   ├── Kerberos
│   ├── DNS
│   └── Replication
│
└── Group Policy
```

---

# FINAL WINDOWS + ACTIVE DIRECTORY CHECKLIST

## Windows Fundamentals

- [ ] Windows architecture
- [ ] User Mode vs Kernel Mode
- [ ] Users
- [ ] Groups
- [ ] Services
- [ ] Processes
- [ ] Registry
- [ ] Event Viewer
- [ ] Windows Firewall
- [ ] PowerShell
- [ ] Windows authentication
- [ ] NTLM
- [ ] Kerberos
- [ ] Windows logs

## Active Directory

- [ ] Active Directory
- [ ] Domain
- [ ] Domain Controller
- [ ] Forest
- [ ] Tree
- [ ] Organizational Unit
- [ ] AD Users
- [ ] AD Groups
- [ ] GPO
- [ ] LDAP
- [ ] Kerberos
- [ ] NTLM
- [ ] SPN
- [ ] SMB
- [ ] DNS in AD
- [ ] Domain trusts
- [ ] AD replication basics

## AD Security

- [ ] Kerberoasting
- [ ] AS-REP Roasting
- [ ] Pass-the-Hash
- [ ] Pass-the-Ticket
- [ ] NTLM Relay
- [ ] Credential Dumping
- [ ] Lateral Movement
- [ ] Privilege Escalation
- [ ] BloodHound concepts
- [ ] Attack paths
- [ ] Windows security logs
- [ ] AD security monitoring
- [ ] Defensive controls

---

# INTERVIEW ANSWER FORMULA

For most Windows and AD questions:

**Definition → How it works → Security relevance → Example**

### Example: Kerberoasting

**Definition:**  
"Kerberoasting is an attack technique that targets Kerberos service accounts associated with SPNs."

**How it works:**  
"An authenticated domain user can request service tickets for applicable services. The ticket material can potentially be attacked offline to recover weak service-account passwords."

**Security relevance:**  
"Weak service-account credentials can allow privilege escalation or lateral movement."

**Example:**  
"A service account running a database service has an SPN and a weak password. If the account is sufficiently privileged, compromising that password could have significant impact."

---

# KEY TAKEAWAY

For enterprise security interviews, do not study Windows and Active Directory as separate topics.

Understand the relationship:

```text
Windows
  ↓
Users + Groups
  ↓
Authentication
  ↓
Kerberos / NTLM
  ↓
Active Directory
  ↓
Domain Controller
  ↓
GPO + LDAP + DNS + SMB
  ↓
Permissions + Trusts
  ↓
Attack Paths
  ↓
Detection + Defense
```

If you understand this flow, you can answer both **SOC/Blue Team** and **authorized Red Team/Pentest** interview questions much more effectively.
