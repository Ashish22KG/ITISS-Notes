# Security — Interview Notes

## 1. Network Security — Definition
The practice of protecting a network, its devices, systems, and data from unauthorized access, attacks, misuse, and disruption using security controls, technologies, and policies.

**Interview answer:** "Network security is the process of protecting network infrastructure, devices, communication, and data from unauthorized access, attacks, and threats by using security mechanisms such as firewalls, IDS/IPS, VPNs, network segmentation, access control, and encryption."

### Major areas of network security
| Area | What it does | Examples |
|---|---|---|
| Firewall | Controls in/outbound traffic | pfSense, FortiGate, iptables |
| IDS/IPS | Detects/blocks malicious traffic | Suricata, Snort |
| VPN | Secure comms over untrusted network | OpenVPN, IPsec, WireGuard |
| Network Segmentation | Splits network into security zones | VLANs, DMZ |
| Access Control | Controls who accesses resources | ACLs, NAC, 802.1X |
| Encryption | Protects data in transit | TLS, IPsec |
| Authentication | Verifies identity | AD, RADIUS, TACACS+ |
| Network Monitoring | Monitors health/suspicious activity | Wireshark, Nagios, Zabbix |
| Security Monitoring | Detects security events | SIEM, Zeek, Suricata |
| Secure Network Design | Designs with security in mind | Zero Trust, DMZ |
| Endpoint/Device Security | Protects connected devices | EDR, antivirus, hardening |
| DDoS Protection | Protects against traffic floods | Rate limiting, mitigation services |
| Wireless Security | Secures Wi-Fi | WPA2/WPA3, 802.1X |
| DNS Security | Protects DNS from abuse | DNS filtering, DNSSEC |
| Network Hardening | Reduces device vulnerabilities | Secure configs, patching |
| Logging & IR | Records/responds to events | SIEM, SOC, incident response |

### Suggested study order
1. **Networking fundamentals** — OSI, TCP/IP, IP, MAC, Port, Protocol, TCP vs UDP, 3-way handshake, ARP, DNS, DHCP, default gateway, NAT/PAT, routing, subnetting
2. **Network security controls** — Firewall, ACL, Proxy, VPN, IDS, IPS, WAF, NAC, segmentation, VLAN, DMZ, Zero Trust
3. **Network attacks** — ARP spoofing, DNS spoofing, IP spoofing, MAC flooding, MITM, DoS/DDoS, port scanning, packet sniffing, session hijacking, DHCP/DNS/VLAN attacks
4. **Tools** — Wireshark (packet analysis), Nmap (scanning), Nessus (vuln scanning), Snort/Suricata (IDS/IPS), pfSense (firewall/router), Zeek (NSM), Netcat, tcpdump

### Example layout
```
                    INTERNET
                       │
                       ▼
                ┌─────────────┐
                │   FIREWALL  │
                └─────────────┘
                       │
                 ┌─────┴─────┐
                 │           │
                DMZ      INTERNAL
                 │           │
             Web Server   Employees
                             │
                         ┌───┴───┐
                         │ IDS/IPS│
                         └───────┘
```
- Firewall → controls traffic
- DMZ → isolates public-facing servers
- IDS/IPS → detects/blocks attacks
- Segmentation → separates zones
- Auth/access control → controls users
- Encryption/VPN → protects comms
- Monitoring/logging → detects suspicious activity

**One-liner:** "Network security is the protection of network infrastructure, devices, communications, and data against unauthorized access, attacks, and disruption using controls such as firewalls, IDS/IPS, VPNs, segmentation, authentication, encryption, and monitoring."

---

## 2. Attack Surface
The total number of possible entry points or weaknesses in a system that an attacker could exploit to gain unauthorized access or cause harm.

**Examples of attack surface elements:** web apps, open ports, servers/workstations, APIs, cloud services, user accounts, mobile apps, remote-access services (VPN/SSH).

**Interview answer:** "Attack surface is the total set of exposed entry points and potential vulnerabilities in a system that an attacker can use to compromise it... The goal of security teams is to reduce and continuously monitor the attack surface."

### Attack Surface vs Attack Vector
| Term | Meaning |
|---|---|
| Attack Surface | All possible entry points an attacker could target |
| Attack Vector | The specific method/path used to exploit an entry point |

Example: An exposed SSH service is part of the attack surface. Using stolen credentials to log in via SSH is the attack vector.

---

## 3. CIA Triad
Fundamental information security model: **Confidentiality, Integrity, Availability**.

| Principle | Meaning | Example |
|---|---|---|
| Confidentiality | Info accessible only to authorized users | Encryption, passwords, access control |
| Integrity | Info stays accurate/complete, unmodified without authorization | Hashing, digital signatures, file integrity monitoring |
| Availability | Systems/info accessible when needed | Backups, redundancy, DDoS protection |

**Banking example:**
- Confidentiality — only you see your account details
- Integrity — your balance can't be altered by unauthorized parties
- Availability — you can access your account when you need it

**Interview answer:** "The CIA Triad is a core information security model consisting of Confidentiality, Integrity, and Availability. Confidentiality protects data from unauthorized access, Integrity protects data from unauthorized modification, and Availability ensures that systems and data are accessible to authorized users when required."

---

*See [[01-networking.md]] for the networking fundamentals referenced above, and [[03-devops.md]] for deployment/cloud topics.*
