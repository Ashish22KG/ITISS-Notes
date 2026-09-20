# Networking — Interview Notes

## 1. OSI Model
7-layer conceptual framework (ISO) explaining how data moves between two devices over a network.

| Layer | Name | Main Function | Examples |
|---|---|---|---|
| 7 | Application | Provides network services to applications | HTTP, DNS, FTP, SMTP |
| 6 | Presentation | Data formatting, encryption, compression | TLS/SSL, JPEG, ASCII |
| 5 | Session | Establishes/manages sessions | RPC, session mgmt |
| 4 | Transport | End-to-end delivery, reliability, flow control | TCP, UDP |
| 3 | Network | Logical addressing and routing | IP, ICMP, routers |
| 2 | Data Link | Frames, MAC addressing, local delivery | Ethernet, ARP, switches |
| 1 | Physical | Transmits raw bits | Cables, fiber, radio |

**Mnemonic (top→bottom):** All People Seem To Need Data Processing

**Interview answer:** "The OSI model is a seven-layer conceptual framework used to understand and standardize network communication... Each layer performs a specific function, such as application services at Layer 7, reliable delivery at Layer 4, routing at Layer 3, and physical transmission at Layer 1."

**Security angle per layer:**
- L7 Application — SQLi, XSS
- L4 Transport — TCP/UDP, port-based attacks
- L3 Network — IP spoofing
- L2 Data Link — ARP spoofing
- L1 Physical — cable tapping, physical access

> Note: OSI is a *conceptual* model; real networks run on TCP/IP, which doesn't map 1:1 to OSI.

---

## 2. TCP/IP Model
4-layer practical model for communication over networks/Internet.

| Layer | Function | Examples |
|---|---|---|
| 4. Application | Network services to apps | HTTP, HTTPS, DNS, FTP, SSH |
| 3. Transport | End-to-end comms, ports | TCP, UDP |
| 2. Internet | Addressing & routing | IP, ICMP, IPsec |
| 1. Network Access | Frames over physical/local net | Ethernet, Wi-Fi, ARP |

**OSI → TCP/IP mapping:**
- Application + Presentation + Session → Application
- Transport → Transport
- Network → Internet
- Data Link + Physical → Network Access

**Interview answer:** "TCP/IP is a four-layer networking model... Application, Transport, Internet, and Network Access."

---

## 3. TCP vs UDP

**TCP** — connection-oriented; establishes a connection; reliable, ordered, error-checked delivery.
**UDP** — connectionless; sends data without a handshake; faster but unreliable/unordered.

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable | Not guaranteed |
| Ordering | Maintains order | No guarantee |
| Acknowledgment | Yes | No |
| Retransmission | Yes | No |
| Speed | Slower | Faster |
| Example | HTTP/HTTPS, SSH, FTP | DNS, DHCP, VoIP, streaming |

**One-liner:** TCP = reliability/order; UDP = speed/low overhead, no delivery guarantee.

---

## 4. Common Protocols (TCP vs UDP) — Ports to Memorize

**TCP:** HTTP 80 · HTTPS 443 · FTP 21 · SSH/SFTP 22 · Telnet 23 · SMTP 25 · DNS 53 (also TCP) · POP3 110 · IMAP 143 · LDAP 389 · SMB 445 · RDP 3389

**UDP:** DNS 53 · DHCP 67/68 · TFTP 69 · NTP 123 · SNMP 161/162 · RADIUS 1812/1813 · SIP 5060 · WireGuard 51820

> Caution: don't say "DNS is a UDP protocol" absolutely — DNS mainly uses UDP/53, but uses TCP/53 for zone transfers and large responses. Several protocols run over both TCP and UDP depending on use case.

---

## 5. IP Address
Logical numerical address assigned to a device to uniquely identify it and enable communication.

| Type | Example | Description |
|---|---|---|
| IPv4 | 192.168.1.10 | 32-bit |
| IPv6 | 2001:db8::1 | 128-bit |
| Public | 8.8.8.8 | Used over the Internet |
| Private | 192.168.1.10 | Used inside LANs |
| Static | 192.168.1.10 | Fixed |
| Dynamic | via DHCP | Can change |

**Learning path:** IPv4 → IPv6 → Public vs Private → Static vs Dynamic → Subnet Mask → Default Gateway → NAT.

---

## 6. MAC Address
Unique hardware identifier for a NIC, used to identify a device on a LAN.

- Layer: Data Link (L2)
- Length: 48 bits (6 bytes), 12 hex chars
- Example: `00:1A:2B:3C:4D:5E`
- A device can have multiple MACs (multiple interfaces)
- Switches forward frames using MAC addresses

| MAC Address | IP Address |
|---|---|
| Layer 2 | Layer 3 |
| Identifies interface | Identifies device on a network |
| Local network comms | Cross-network comms |
| 48-bit | IPv4=32-bit, IPv6=128-bit |
| Switches use it | Routers use it |

---

## 7. Ports
16-bit logical number (TCP/UDP) identifying an application/service on a device.

- Range: 0–65535
  - 0–1023: Well-known ports
  - 1024–49151: Registered ports (assigned by IANA, e.g. 3306 MySQL, 5432 PostgreSQL, 8080 HTTP-alt, 1433 MSSQL)
  - 49152–65535: Dynamic/ephemeral ports

**IANA** = Internet Assigned Numbers Authority — coordinates IP allocations, DNS root zone, protocol parameters, and port assignments.

**Rule of thumb:** IP address identifies the *device*; port identifies the *service/application*.

---

## 8. Protocol (definition)
A set of predefined rules/standards defining how devices communicate, exchange data, and interpret information over a network — what to send, how to send it, and how to interpret it.

---

## 9. TCP 3-Way Handshake
Process to establish a reliable TCP connection before data transfer.

| Step | Direction | Flag | Purpose |
|---|---|---|---|
| 1 | Client → Server | SYN | Request connection |
| 2 | Server → Client | SYN+ACK | Acknowledge + own sync request |
| 3 | Client → Server | ACK | Acknowledge; connection established |

**Remember:** SYN → SYN-ACK → ACK → Connection Established

---

## 10. DNS
Translates human-readable domain names into IP addresses.

- Example: `www.google.com → 142.250.x.x`
- Hierarchical, distributed naming system.
- Protocol: primarily UDP/53; TCP/53 for zone transfers / large responses.

### DNS Zone Transfer
Copies zone data from a primary (master) DNS server to a secondary (slave) server to keep records in sync.

- **AXFR** — full zone transfer
- **IXFR** — incremental (only changes)
- **Security risk:** unauthorized zone transfers can leak internal hosts, subdomains, IPs — useful for attacker recon.

### Primary Authoritative DNS Server
Holds the original, authoritative copy of a zone's records — the source of truth. Secondary authoritative servers sync from it via zone transfer (AXFR/IXFR) and can also answer queries authoritatively.

---

## 11. DHCP
Automatically assigns IP configuration (IP, subnet mask, default gateway, DNS server) to devices.

- Protocol: UDP — Server port 67, Client port 68

### DORA Process
| Step | Name | What happens |
|---|---|---|
| D | Discover | Client broadcasts: "Any DHCP server available?" |
| O | Offer | Server responds with an offered IP + config |
| R | Request | Client broadcasts: "I accept this IP" |
| A | Acknowledgment | Server confirms the lease |

Initial Discover is broadcast because the client doesn't yet know the DHCP server's IP.

---

## 12. Default Gateway
The router IP that forwards traffic from the local network to other/outside networks.

- Same subnet → direct communication
- Different subnet → traffic sent to the default gateway, which forwards it

---

## 13. ARP (Address Resolution Protocol)
Resolves a known IPv4 address into its corresponding MAC address on a local network.

**Flow:**
1. Device broadcasts ARP Request: "Who has this IP?"
2. Owner replies with ARP Reply containing its MAC.
3. Sender caches the IP–MAC mapping (ARP cache) and sends the frame.

---

*See [[03-devops.md]] for deployment/cloud/EC2 topics and [[02-security.md]] for CIA Triad, Attack Surface, and Network Security controls.*
