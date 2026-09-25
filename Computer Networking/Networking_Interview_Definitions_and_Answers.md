# Networking — Interview Definitions & Answers

## 1. OSI Model

**Definition:**  
The OSI (Open Systems Interconnection) model is a 7-layer conceptual model used to understand how data moves between networked systems.

**7 Layers:**
1. Physical — cables, signals, bits
2. Data Link — frames, MAC addresses, switches
3. Network — packets, IP addresses, routing
4. Transport — TCP/UDP, ports, reliability
5. Session — manages communication sessions
6. Presentation — data format, encryption, compression
7. Application — network services used by applications such as HTTP, DNS, SMTP

**Interview Answer:**  
"The OSI model divides network communication into seven layers. Each layer has a specific responsibility, which helps with network design, troubleshooting, and security analysis. For example, IP and routing operate at Layer 3, while TCP/UDP and ports operate at Layer 4."

---

## 2. TCP/IP Model

**Definition:**  
The TCP/IP model is the practical networking model used by the Internet and modern IP networks.

**Layers:**
- Application
- Transport
- Internet
- Network Access / Link

**Interview Answer:**  
"TCP/IP is a four-layer model used in real-world networking. For example, HTTP and DNS are in the Application layer, TCP and UDP are in the Transport layer, IP is in the Internet layer, and Ethernet/Wi-Fi operate in the Link layer."

---

## 3. IP Addressing

**Definition:**  
An IP address is a logical address assigned to a network interface so devices can be identified and communicate over an IP network.

**Interview Answer:**  
"An IP address identifies a device or interface at the network layer and allows packets to be delivered between networks. IPv4 uses 32-bit addresses, while IPv6 uses 128-bit addresses."

---

## 4. IPv4 / IPv6 Basics

**Definition:**  
IPv4 is a 32-bit addressing system written in dotted-decimal notation, while IPv6 is a 128-bit addressing system written in hexadecimal notation.

**Example:**
- IPv4: `192.168.1.10`
- IPv6: `2001:db8::10`

**Interview Answer:**  
"IPv4 provides about 4.3 billion addresses and commonly uses private addressing with NAT. IPv6 provides a vastly larger address space and includes features such as SLAAC for address configuration."

---

## 5. Subnetting

**Definition:**  
Subnetting divides a larger IP network into smaller logical networks called subnets.

**Interview Answer:**  
"Subnetting is used to divide a network into smaller networks to improve address utilization, routing efficiency, and network segmentation. The subnet mask or CIDR prefix determines which part is the network portion and which part is the host portion."

**Example:**  
`192.168.1.0/24` can be divided into smaller `/26` networks.

---

## 6. MAC Address

**Definition:**  
A MAC address is a link-layer hardware/interface identifier used for communication within a local network.

**Interview Answer:**  
"A MAC address operates primarily at the Data Link layer and is used by Ethernet and Wi-Fi networks for local delivery. Switches learn MAC addresses and use them to forward frames."

---

## 7. ARP

**Definition:**  
ARP (Address Resolution Protocol) maps an IPv4 address to a MAC address on a local network.

**Interview Answer:**  
"When a host knows the destination IPv4 address but needs its local MAC address, it can use ARP. The host sends an ARP request and the device owning the IPv4 address responds with its MAC address."

**Security Note:**  
ARP has no built-in authentication, so ARP spoofing/poisoning is a possible attack.

---

## 8. DHCP

**Definition:**  
DHCP (Dynamic Host Configuration Protocol) automatically provides network configuration such as IP address, subnet mask, default gateway, and DNS server information.

**Interview Answer:**  
"DHCP automates IP configuration. A typical DHCP exchange is DORA: Discover, Offer, Request, and Acknowledgment."

---

## 9. DNS

**Definition:**  
DNS (Domain Name System) translates domain names into IP addresses and can also provide other DNS records.

**Interview Answer:**  
"DNS acts like a naming system for networks. When a client needs to access a domain, DNS resolution can provide the corresponding IP address. Common records include A, AAAA, CNAME, MX, and TXT."

**Security Note:**  
DNS can be abused through techniques such as DNS spoofing, cache poisoning, tunneling, and malicious domains.

---

## 10. HTTP / HTTPS

**Definition:**  
HTTP is an application-layer protocol used for web communication. HTTPS is HTTP protected by TLS.

**Interview Answer:**  
"HTTP transfers web requests and responses between clients and servers. HTTPS adds TLS encryption and authentication to protect data in transit and help prevent interception and tampering."

**Common Ports:**
- HTTP: 80
- HTTPS: 443

---

## 11. TCP vs UDP

**Definition:**  
TCP is connection-oriented and provides reliable, ordered delivery. UDP is connectionless and provides a lightweight datagram service without TCP's reliability mechanisms.

**Interview Answer:**  
"TCP is used when reliable and ordered delivery is important, such as many web and file-transfer connections. UDP has lower protocol overhead and is useful where low latency or application-controlled reliability is preferred, such as DNS queries and many real-time applications."

genui{"learning_viz":{"type_id":"TCP_VS_UDP","initial_values":{"protocol":"tcp","lossMode":"drop_packet_3"}}}

---

## 12. TCP 3-Way Handshake

**Definition:**  
The TCP three-way handshake establishes a TCP connection between a client and server.

**Steps:**
1. SYN — client requests a connection.
2. SYN-ACK — server acknowledges and responds.
3. ACK — client acknowledges the server.

**Interview Answer:**  
"The TCP three-way handshake establishes the initial connection and synchronizes sequence numbers between the endpoints before normal data transfer begins."

---

## 13. Ports

**Definition:**  
A port is a logical endpoint identifier used by the Transport layer to direct network traffic to a particular service or application.

**Examples:**
- 22 — SSH
- 25 — SMTP
- 53 — DNS
- 80 — HTTP
- 443 — HTTPS
- 389 — LDAP
- 445 — SMB
- 3389 — RDP

**Interview Answer:**  
"IP addresses identify hosts or interfaces, while ports help identify services or application endpoints on those hosts."

---

## 14. Common Protocols

| Protocol | Common Port(s) | Purpose |
|---|---:|---|
| HTTP | 80 | Web traffic |
| HTTPS | 443 | Secure web traffic |
| SSH | 22 | Secure remote administration |
| FTP | 21 | File transfer control |
| SFTP | 22 | File transfer over SSH |
| DNS | 53 | Name resolution |
| DHCP | 67/68 | Automatic IP configuration |
| SMTP | 25, 587, 465 | Email submission/transfer |
| SNMP | 161/162 | Network monitoring/management |
| LDAP | 389 | Directory services |
| LDAPS | 636 | LDAP over TLS |
| Kerberos | 88 | Authentication |
| ICMP | No TCP/UDP port | Network control/diagnostics |

**Interview Answer:**  
"I identify a protocol by its purpose, transport, and common port, but I do not assume that a service is using its default port because administrators can configure services to listen elsewhere."

---

## 15. Routing

**Definition:**  
Routing is the process of selecting a path and forwarding IP packets between different networks.

**Interview Answer:**  
"Routers use routing tables to determine where packets should be forwarded. Routes can be static or learned dynamically using routing protocols such as OSPF or BGP."

---

## 16. Default Gateway

**Definition:**  
A default gateway is the router or Layer 3 device a host sends traffic to when the destination is outside the host's local subnet.

**Interview Answer:**  
"If a destination is not on the local subnet, the host sends the packet to its configured default gateway, which then routes it toward the destination."

---

## 17. NAT / PAT

### NAT

**Definition:**  
NAT (Network Address Translation) translates IP addresses between address spaces, commonly between private and public IPv4 addresses.

### PAT

**Definition:**  
PAT (Port Address Translation) allows multiple private hosts to share a public IPv4 address by translating transport-layer port information.

**Interview Answer:**  
"NAT changes IP address information between network address spaces. PAT extends this by using different port mappings so many internal devices can share one public IPv4 address."

---

## 18. Port Forwarding

**Definition:**  
Port forwarding maps traffic arriving at a particular address and port on a gateway to a specified internal host and port.

**Interview Answer:**  
"Port forwarding allows an external client to reach a selected internal service through a gateway. It should be restricted carefully because it can expose internal services to untrusted networks."

---

## 19. VLAN

**Definition:**  
A VLAN (Virtual Local Area Network) logically separates devices into different Layer 2 broadcast domains on the same physical switching infrastructure.

**Interview Answer:**  
"VLANs provide logical network separation without requiring separate physical switches for every network. For example, an organization can separate users, servers, and guest devices into different VLANs and control communication between them with Layer 3 routing and firewall policies."

---

## 20. VPN

**Definition:**  
A VPN (Virtual Private Network) creates a protected logical connection over an untrusted or shared network.

**Interview Answer:**  
"A VPN can provide confidentiality and integrity for traffic over an untrusted network and can also provide remote-access or site-to-site connectivity. The exact security properties depend on the VPN protocol and configuration."

---

## 21. Proxy

**Definition:**  
A proxy is an intermediary that receives requests from a client and forwards them to another server.

**Interview Answer:**  
"A forward proxy acts on behalf of clients, while a reverse proxy acts on behalf of servers. Proxies can provide traffic control, filtering, logging, caching, or application-layer security functions."

---

## 22. Firewall

**Definition:**  
A firewall is a security control that monitors and controls network traffic according to defined rules or policies.

**Interview Answer:**  
"A firewall can allow or deny traffic based on attributes such as source, destination, protocol, port, interface, connection state, or application context, depending on its capabilities."

---

## 23. IDS vs IPS

### IDS

**Definition:**  
An IDS (Intrusion Detection System) monitors traffic or activity and generates alerts when suspicious behavior is detected.

### IPS

**Definition:**  
An IPS (Intrusion Prevention System) can actively block or prevent detected malicious traffic or activity.

**Interview Answer:**  
"The main difference is the response. IDS primarily detects and alerts, while IPS is deployed inline and can take preventive action such as dropping or blocking traffic."

---

## 24. Network Segmentation

**Definition:**  
Network segmentation divides a network into separate security or administrative zones and controls communication between them.

**Interview Answer:**  
"Segmentation limits unnecessary communication and can reduce lateral movement if one system is compromised. VLANs, subnets, firewalls, and access-control policies can all be used to implement segmentation."

---

## 25. Switching vs Routing

| Switching | Routing |
|---|---|
| Primarily operates at Layer 2 | Primarily operates at Layer 3 |
| Uses MAC addresses | Uses IP addresses |
| Forwards frames | Forwards packets |
| Connects devices within LANs/VLANs | Connects different IP networks |

**Interview Answer:**  
"A switch primarily forwards Ethernet frames within a Layer 2 network using MAC addresses, while a router forwards IP packets between different networks using IP addresses and routing information."

---

## 26. ICMP

**Definition:**  
ICMP (Internet Control Message Protocol) is used by IP networks for control, diagnostic, and error-reporting messages.

**Interview Answer:**  
"ICMP is not a transport protocol like TCP or UDP. Tools such as ping use ICMP Echo Request and Echo Reply messages, while other ICMP messages report network conditions such as unreachable destinations."

---

## 27. SSH

**Definition:**  
SSH (Secure Shell) is a protocol for secure remote administration and other secure communication over an IP network.

**Common Port:**  
22/TCP

**Interview Answer:**  
"SSH provides encrypted remote access to systems and supports authentication using passwords or, preferably in many administrative environments, cryptographic keys. It is commonly used to administer Linux servers."

---

## 28. FTP / SFTP

### FTP

**Definition:**  
FTP (File Transfer Protocol) is a file-transfer protocol that traditionally uses separate control and data connections and does not encrypt traffic by itself.

### SFTP

**Definition:**  
SFTP (SSH File Transfer Protocol) is a file-transfer protocol that operates over SSH.

**Interview Answer:**  
"FTP does not provide encryption by default, so credentials and data can be exposed on an untrusted network. SFTP operates through SSH and provides encrypted transport."

**Important:**  
SFTP is not simply 'secure FTP'; it is a different protocol.

---

## 29. SMTP

**Definition:**  
SMTP (Simple Mail Transfer Protocol) is used for sending and relaying email.

**Common Ports:**
- 25 — server-to-server SMTP / relay
- 587 — message submission
- 465 — commonly used for SMTP submission with TLS

**Interview Answer:**  
"SMTP is responsible for sending and relaying email. Mail clients typically use a message-submission service, while mail servers use SMTP for server-to-server delivery."

---

## 30. SNMP

**Definition:**  
SNMP (Simple Network Management Protocol) is used to monitor and manage network devices and systems.

**Common Ports:**
- UDP 161 — queries/management
- UDP 162 — traps/notifications

**Interview Answer:**  
"SNMP allows monitoring systems to retrieve information from network devices and can also receive asynchronous notifications called traps. SNMPv3 adds security features including authentication and encryption."

---

## 31. LDAP

**Definition:**  
LDAP (Lightweight Directory Access Protocol) is a protocol for accessing and managing directory information.

**Common Ports:**
- 389 — LDAP
- 636 — LDAP over TLS

**Interview Answer:**  
"LDAP is commonly used to access directory services containing users, groups, computers, and other organizational objects. It can be used by applications for centralized identity and directory lookups."

---

## 32. Kerberos

**Definition:**  
Kerberos is a network authentication protocol that uses tickets and a trusted Key Distribution Center (KDC) to authenticate users and services.

**Common Port:**  
88/TCP and UDP

**Interview Answer:**  
"Kerberos provides ticket-based authentication. Instead of repeatedly sending a password to services, a client obtains tickets from the authentication infrastructure and uses those tickets to authenticate to permitted services."

**Security Note:**  
Kerberos is a core authentication protocol used by Active Directory environments.

---

# Security Interview Rapid-Fire Questions

## Q1. Why is networking important in cybersecurity?

**Answer:**  
"Most attacks involve network communication at some stage. Strong networking knowledge helps me understand traffic flows, identify abnormal communication, analyze packets, configure firewalls, perform reconnaissance, and troubleshoot security incidents."

## Q2. What happens when you enter a URL in a browser?

**Answer:**  
"First, the browser resolves the domain through DNS if the address is not already available from cache. The client determines how to reach the destination, establishes the required transport connection, and for HTTPS performs a TLS handshake. The browser then sends the HTTP request and processes the server's response."

## Q3. What happens when a host communicates with another host on the same subnet?

**Answer:**  
"The sender determines that the destination is local using its subnet information. For IPv4, if it needs the destination MAC address, it can use ARP. The Ethernet frame is then sent toward the destination through the local network."

## Q4. What happens when the destination is on another subnet?

**Answer:**  
"The sender determines that the destination is remote and sends the packet to its default gateway. The gateway/router then uses its routing table to forward the packet toward the destination network."

## Q5. What is the difference between a switch and a router?

**Answer:**  
"A switch primarily forwards Layer 2 frames using MAC addresses, while a router forwards Layer 3 IP packets between different networks."

## Q6. What is the difference between a firewall and an IDS?

**Answer:**  
"A firewall enforces traffic-control policies, while an IDS primarily detects suspicious activity and generates alerts. An IPS can additionally block detected traffic when deployed inline."

## Q7. Why is subnetting important for security?

**Answer:**  
"Subnetting can help organize and separate networks. When combined with VLANs and firewall policies, it can reduce unnecessary communication and restrict access between security zones."

## Q8. Why is HTTPS more secure than HTTP?

**Answer:**  
"HTTPS uses TLS to protect HTTP traffic in transit and authenticate the server using certificates. This helps provide confidentiality and integrity and reduces the risk of network-level interception and tampering."

## Q9. What is the difference between authentication and authorization?

**Answer:**  
"Authentication verifies who a user or system is. Authorization determines what that authenticated identity is allowed to access or do."

## Q10. How would you troubleshoot a connectivity problem?

**Answer:**  
"I would troubleshoot layer by layer: check the physical/link state, interface configuration, IP address and subnet, default gateway, ARP, DNS resolution, routing, firewall rules, listening ports, and application-level connectivity. Tools such as ping, traceroute/tracert, ipconfig/ip, arp, nslookup/dig, netstat/ss, and packet capture can help isolate the problem."

---

# Important Security Connections to Remember

| Networking Concept | Security Relevance |
|---|---|
| IP addressing | Host identification and traffic analysis |
| Subnetting | Network organization and segmentation |
| ARP | ARP spoofing/poisoning |
| DNS | DNS spoofing, tunneling, malicious domains |
| HTTP/HTTPS | Web security and traffic inspection |
| TCP/UDP | Port scanning and traffic analysis |
| Ports | Service discovery and access control |
| Routing | Traffic paths and segmentation |
| NAT/PAT | Address translation and exposure considerations |
| VLAN | Layer 2 segmentation |
| VPN | Protected remote/site-to-site connectivity |
| Proxy | Traffic inspection and access control |
| Firewall | Network access control |
| IDS/IPS | Detection and prevention |
| SSH | Secure administration |
| FTP/SFTP | Secure vs insecure file transfer |
| SMTP | Email security |
| SNMP | Infrastructure monitoring |
| LDAP | Directory and identity services |
| Kerberos | Authentication and Active Directory |

---

# Interview Answer Formula

For networking questions, use this structure:

**1. Definition → 2. How it works → 3. Why it matters → 4. Example**

### Example: Firewall

**Definition:**  
"A firewall is a security control that monitors and controls network traffic based on defined rules."

**How it works:**  
"It evaluates traffic attributes such as source, destination, protocol, port, and connection state against configured policies."

**Why it matters:**  
"It can restrict unauthorized network communication and reduce the attack surface."

**Example:**  
"For example, an organization may allow HTTPS traffic to a public web server while blocking direct inbound SSH access from the Internet."

---

# Final Networking Checklist

Before a cybersecurity interview, make sure you can explain without notes:

- [ ] OSI 7 layers
- [ ] TCP/IP model
- [ ] IPv4 and IPv6
- [ ] Subnetting and CIDR
- [ ] MAC and ARP
- [ ] DHCP and DORA
- [ ] DNS and DNS records
- [ ] HTTP vs HTTPS
- [ ] TCP vs UDP
- [ ] TCP 3-way handshake
- [ ] Ports and common protocols
- [ ] Routing and routing tables
- [ ] Default gateway
- [ ] NAT/PAT
- [ ] Port forwarding
- [ ] VLANs
- [ ] VPNs
- [ ] Forward vs reverse proxy
- [ ] Firewalls
- [ ] IDS vs IPS
- [ ] Network segmentation
- [ ] Switching vs routing
- [ ] ICMP
- [ ] SSH
- [ ] FTP vs SFTP
- [ ] SMTP
- [ ] SNMP
- [ ] LDAP
- [ ] Kerberos
- [ ] Basic network troubleshooting
- [ ] Security relevance of each concept
