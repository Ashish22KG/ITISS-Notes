# Security — Interview Q&A

### Q1. What is network security?
**A:** Network security is the process of protecting network infrastructure, devices, communication, and data from unauthorized access, attacks, and threats using security mechanisms such as firewalls, IDS/IPS, VPNs, network segmentation, access control, and encryption.

### Q2. What all comes under network security?
**A:** Firewalls, IDS/IPS, VPNs, network segmentation (VLANs/DMZ), access control (ACLs/NAC/802.1X), encryption (TLS/IPsec), authentication (AD/RADIUS/TACACS+), network monitoring (Wireshark/Nagios/Zabbix), security monitoring (SIEM/Zeek/Suricata), secure network design (Zero Trust/DMZ), endpoint/device security (EDR/antivirus/hardening), DDoS protection, wireless security (WPA2/WPA3/802.1X), DNS security (filtering/DNSSEC), network hardening, and logging & incident response.

### Q3. What order should you learn network security topics in for interviews?
**A:** 1) Networking fundamentals (OSI, TCP/IP, IP/MAC/Port, protocols, TCP vs UDP, 3-way handshake, ARP, DNS, DHCP, gateway, NAT/PAT, routing, subnetting). 2) Network security controls (Firewall, ACL, Proxy, VPN, IDS, IPS, WAF, NAC, segmentation, VLAN, DMZ, Zero Trust). 3) Network attacks (ARP/DNS/IP spoofing, MAC flooding, MITM, DoS/DDoS, port scanning, packet sniffing, session hijacking, DHCP/DNS/VLAN attacks). 4) Tools (Wireshark, Nmap, Nessus, Snort, Suricata, pfSense, Zeek, Netcat, tcpdump).

### Q4. In a typical network security design with a firewall, DMZ, and internal network, what role does each component play?
**A:** The Firewall controls incoming/outgoing traffic; the DMZ isolates public-facing servers (like a web server) from the internal network; IDS/IPS on the internal side detects/blocks attacks; network segmentation separates zones; authentication and access control manage users; encryption/VPN protect communications; and monitoring/logging detect suspicious activity.

### Q5. What is an attack surface?
**A:** The total number of possible entry points or weaknesses in a system that an attacker could exploit to gain unauthorized access or cause harm — e.g., web applications, open ports, servers/workstations, APIs, cloud services, user accounts, mobile apps, and remote-access services like VPN/SSH.

### Q6. What's the difference between an attack surface and an attack vector?
**A:** Attack surface is the total set of exposed entry points an attacker could target; an attack vector is the specific method/path used to exploit one of those entry points. Example: an exposed SSH service is part of the attack surface; using stolen credentials to log in via SSH is the attack vector.

### Q7. What is the CIA Triad?
**A:** A fundamental information security model consisting of Confidentiality, Integrity, and Availability, used to protect information and systems from unauthorized access, modification, and disruption.

### Q8. Explain each element of the CIA Triad with an example.
**A:** Confidentiality — ensures info is accessible only to authorized users (e.g., encryption, passwords, access control); on a banking app, only you can see your account details. Integrity — ensures info stays accurate and unmodified without authorization (e.g., hashing, digital signatures); your bank balance can't be altered by an unauthorized person. Availability — ensures systems/data are accessible when needed (e.g., backups, redundancy, DDoS protection); you can access your bank account whenever you need it.
