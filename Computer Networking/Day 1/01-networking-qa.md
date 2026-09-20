# Networking — Interview Q&A

### Q1. What is the OSI Model?
**A:** The OSI (Open Systems Interconnection) Model is a 7-layer conceptual framework developed by ISO to explain how data is transmitted between two devices over a network. The layers are Application, Presentation, Session, Transport, Network, Data Link, and Physical. Each performs a specific function — e.g., application services at Layer 7, reliable delivery at Layer 4, routing at Layer 3, and physical transmission at Layer 1.

### Q2. How do you remember the OSI layers in order?
**A:** Using the mnemonic "All People Seem To Need Data Processing" (Application → Presentation → Session → Transport → Network → Data Link → Physical).

### Q3. From a security perspective, what kinds of attacks map to each OSI layer?
**A:** L7 Application — SQL Injection, XSS; L4 Transport — TCP/UDP port-based attacks; L3 Network — IP spoofing; L2 Data Link — ARP spoofing; L1 Physical — cable tapping/physical access attacks.

### Q4. Is OSI used in real-world networking?
**A:** OSI is primarily a conceptual/reference model. Real-world networking commonly uses the TCP/IP model, whose layers don't map one-to-one with OSI.

### Q5. What is the TCP/IP Model?
**A:** A 4-layer networking framework used for communication between devices over a network or the Internet, defining how data is encapsulated, transmitted, routed, and delivered. Its layers are Application, Transport, Internet, and Network Access.

### Q6. How does the TCP/IP model map to the OSI model?
**A:** Application + Presentation + Session (OSI) → Application (TCP/IP); Transport → Transport; Network → Internet; Data Link + Physical → Network Access.

### Q7. Walk through what happens at each TCP/IP layer when you open https://google.com.
**A:** Application layer — HTTPS creates the application data; Transport layer — TCP breaks data into segments and uses port numbers; Internet layer — IP adds source/destination addresses and determines routing; Network Access layer — Ethernet/Wi-Fi packages data into frames and transmits it. The destination reverses this process.

### Q8. Define TCP and UDP.
**A:** TCP (Transmission Control Protocol) is a connection-oriented transport-layer protocol that establishes a connection and provides reliable, ordered, error-checked delivery. UDP (User Datagram Protocol) is a connectionless transport-layer protocol that sends data without establishing a connection, offering faster but unreliable, unordered delivery.

### Q9. When would you choose TCP over UDP, and vice versa?
**A:** TCP — e.g., downloading a file, where losing data would corrupt the file, so reliability matters. UDP — e.g., live voice/video, where speed and low latency matter more than retransmitting every lost packet.

### Q10. Name some protocols that use TCP and some that use UDP.
**A:** TCP: HTTP (80), HTTPS (443), FTP (21), SSH/SFTP (22), Telnet (23), SMTP (25), POP3 (110), IMAP (143), LDAP (389), SMB (445), RDP (3389). UDP: DNS (53), DHCP (67/68), TFTP (69), NTP (123), SNMP (161/162), RADIUS (1812/1813), SIP (5060), WireGuard (51820). Some protocols (like DNS and RDP) can use both TCP and UDP depending on the scenario.

### Q11. Is it correct to say "DNS is a UDP protocol"?
**A:** Not as an absolute statement. DNS primarily uses UDP port 53 for normal queries, but uses TCP port 53 for situations like zone transfers and responses that require TCP.

### Q12. What is an IP address?
**A:** A logical numerical address assigned to a device on a network to uniquely identify it and enable communication between devices — used to identify the source and destination of packets.

### Q13. What are the types of IP addresses?
**A:** IPv4 (32-bit, e.g. 192.168.1.10), IPv6 (128-bit, e.g. 2001:db8::1), Public (used over the Internet), Private (used inside LANs), Static (fixed), and Dynamic (assigned by DHCP, can change).

### Q14. What is a MAC address?
**A:** A unique hardware identifier assigned to a device's network interface (NIC), used to identify the device on a local network. It operates at Layer 2 (Data Link), is typically 48 bits, and is written in hexadecimal, e.g. `00:1A:2B:3C:4D:5E`.

### Q15. What's the difference between a MAC address and an IP address?
**A:** MAC operates at Layer 2 and identifies the network interface for local communication (used by switches); IP operates at Layer 3 and identifies a device for communication across networks (used by routers). MAC is 48-bit; IPv4 is 32-bit and IPv6 is 128-bit.

### Q16. What is a port?
**A:** A 16-bit logical number used by TCP or UDP to identify a specific application or network service running on a device, ranging from 0–65535.

### Q17. What are the port ranges, and what is each used for?
**A:** 0–1023 = Well-known ports (standardized services); 1024–49151 = Registered ports (registered with IANA for specific apps, e.g., 3306 MySQL, 5432 PostgreSQL); 49152–65535 = Dynamic/ephemeral ports (usually temporary client-side connections).

### Q18. What is IANA?
**A:** IANA stands for Internet Assigned Numbers Authority. It coordinates and maintains key Internet identifiers, including IP address allocations, the DNS root zone, protocol parameters, and port number assignments.

### Q19. Define "protocol."
**A:** A protocol is a set of rules and standards that govern communication between devices over a network. It defines how data is formatted, transmitted, received, and interpreted. Examples include TCP, UDP, HTTP, DNS, and SSH.

### Q20. What is the TCP 3-way handshake?
**A:** A three-step process used to establish a TCP connection before data transmission begins: the client sends SYN, the server responds with SYN-ACK, and the client sends ACK. After this, the connection is established and data transfer can begin.

### Q21. What is DNS?
**A:** DNS (Domain Name System) is a hierarchical, distributed naming system/network service that translates human-readable domain names into IP addresses, so clients can locate the destination server (e.g., www.google.com → 142.250.x.x).

### Q22. What is a DNS zone transfer?
**A:** The process of replicating DNS zone records from a primary (master) DNS server to a secondary (slave) DNS server to keep records synchronized. AXFR transfers the entire zone; IXFR transfers only the changes since the last transfer.

### Q23. Why is DNS zone transfer a security concern?
**A:** If a DNS server allows unauthorized zone transfers, an attacker can retrieve records revealing internal hosts, subdomains, and IP addresses — useful information for reconnaissance.

### Q24. What is a primary authoritative DNS server?
**A:** The server that maintains the original DNS zone data and is the source from which secondary authoritative DNS servers synchronize their records via zone transfer (AXFR/IXFR). "Authoritative" means it has official DNS info for that zone — not that it's the only server or handles every query.

### Q25. What is DHCP?
**A:** DHCP (Dynamic Host Configuration Protocol) is a client-server protocol that automatically assigns IP configuration — IP address, subnet mask, default gateway, DNS server — to devices, so they can communicate without manual configuration. It uses UDP: server on port 67, client on port 68.

### Q26. Explain the DORA process.
**A:** DORA is the four-step process a client uses to get an IP address from a DHCP server: **D**iscover (client broadcasts a request for a DHCP server), **O**ffer (server responds with an available IP and config), **R**equest (client broadcasts acceptance of the offered IP), **A**cknowledgment (server confirms the lease). The initial Discover is broadcast because the client doesn't yet know the DHCP server's IP.

### Q27. What is a default gateway?
**A:** A network device, usually a router, that forwards traffic from a local network to other networks when the destination is outside the local subnet. If the destination is on the same subnet, communication is direct; otherwise, traffic goes to the default gateway first.

### Q28. What is ARP and how does it work?
**A:** ARP (Address Resolution Protocol) is used in IPv4 networks to resolve a known IP address into its corresponding MAC address on a local network. The device broadcasts an ARP Request ("Who has this IP?"), the owning device replies with an ARP Reply containing its MAC address, and the sender caches this IP-to-MAC mapping (ARP cache) to send the frame.
