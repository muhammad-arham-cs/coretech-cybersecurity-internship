Networking Fundamentals for Cybersecurity
========================================
This report covers the essential networking concepts every cybersecurity intern needs to know. All diagrams are kept in the images folder and can be added later.


1. OSI Model – All 7 Layers Explained
-------------------------------------

The OSI model is a conceptual framework that standardises network communication into seven layers. Each layer serves the one above it and gets services from the one below.

OSI MODEL IMAGE
<img width="891" height="413" alt="OSI-Model" src="https://github.com/user-attachments/assets/631a0a64-c517-4c9a-966a-e0012a08d8db" />


Layer 7 – Application
The layer closest to the user. Provides network services directly to applications like web browsers and email clients.
Protocols: HTTP, FTP, SMTP, DNS, DHCP.

Layer 6 – Presentation
Handles data translation, encryption, and compression. Makes sure data sent from one system is readable by another.
Examples: SSL/TLS encryption, character encoding.

Layer 5 – Session
Manages connections between applications. Sets up, maintains, and tears down sessions.
Functions: Authentication, session checkpointing.

Layer 4 – Transport
Responsible for end-to-end communication, reliability, and flow control. Uses ports to identify applications.
Protocols: TCP (reliable), UDP (fast, connectionless).

Layer 3 – Network
Handles logical addressing (IP addresses) and routing between networks.
Devices: Routers.
Protocols: IP, ICMP, ARP.

Layer 2 – Data Link
Provides node-to-node data transfer and uses physical addressing (MAC addresses). Organises bits into frames.
Devices: Switches, bridges.
Sub-layers: LLC and MAC.

Layer 1 – Physical
Transmits raw bits over a physical medium like copper cables, fibre, or radio waves.
Devices: Hubs, repeaters, cables, antennas.




2. TCP/IP Model
---------------

The internet actually runs on a simpler four-layer model called the TCP/IP model.

TCP/IP MODEL IMAGE
<img width="960" height="720" alt="tcp-ip model" src="https://github.com/user-attachments/assets/7c705820-edd5-436e-a593-f607354b7e38" />


TCP/IP Layer          Rough OSI Equivalent    Examples of Protocols
--------------------- ----------------------- -----------------------
Application           Layers 5–7              HTTP, HTTPS, DNS, SSH, FTP, DHCP
Transport             Layer 4                 TCP, UDP
Internet              Layer 3                 IP (IPv4/IPv6), ICMP, ARP
Network Access        Layers 1–2              Ethernet, Wi-Fi, MAC addresses

Why it matters: Different attacks target different layers. ARP poisoning hits the Network Access layer, while DNS spoofing targets the Application layer.




3. Common Protocols
-------------------

Protocol   Port(s)   Transport   Purpose                                Security Note
--------   ------    ---------   -------                                -------------
HTTP       80        TCP         Unencrypted web traffic                Insecure, plain text
HTTPS      443       TCP         Encrypted web traffic (HTTP over TLS)  Safe from eavesdropping
FTP        20,21     TCP         File transfer (old, clear-text)        Very insecure, use SFTP
SSH        22        TCP         Secure remote shell and file transfer  Encrypted authentication
DNS        53        UDP/TCP     Resolves domain names to IPs           Can be spoofed (use DNSSEC)
DHCP       67/68     UDP         Dynamically gives out IP addresses     Rogue DHCP server risk




4. IP Addressing and Subnetting Basics
--------------------------------------

An IPv4 address is 32 bits, written as four decimal octets, like 192.168.1.100.
The subnet mask tells you which part is the network and which part is the host.

Example: 192.168.1.100 with mask 255.255.255.0 (/24)
- Network ID: 192.168.1.0
- Host ID: .100

Private IP ranges (not routable on the internet):
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

Quick subnet reference:

CIDR   Subnet Mask         Usable Hosts
/24    255.255.255.0       254
/25    255.255.255.128     126
/26    255.255.255.192     62
/30    255.255.255.252     2 (point-to-point links)

Why subnet? It reduces broadcast traffic, improves security, and saves IP addresses.




5. TCP vs UDP – Key Differences
-------------------------------

Feature          TCP (Transmission Control Protocol)    UDP (User Datagram Protocol)
-------          ------------------------------------    ----------------------------
Connection       Connection-oriented (3-way handshake)  Connectionless
Reliability      Yes (ACKs, retransmission)             None
Ordering         Packets arrive in order                May arrive out of order
Speed            Slower (more overhead)                 Faster
Common Uses      Web browsing, email, SSH, FTP          Streaming, VoIP, DNS, DHCP

TCP 3-way handshake (text diagram):

Client                                Server
  | -------- SYN (seq=x) ----------->   |
  | <------ SYN+ACK (seq=y, ack=x+1) -- |
  | -------- ACK (ack=y+1) ---------->  |
  |    (data transfer starts)




6. Types of Network Attacks
---------------------------

A. Man-in-the-Middle (MitM)
An attacker secretly intercepts and possibly alters communication between two parties.
Example: Evil twin Wi-Fi hotspot.
Defence: Use HTTPS, VPNs, avoid untrusted networks.

B. DNS Spoofing (Cache Poisoning)
Fake DNS entries are injected into a resolver's cache. A victim typing "bank.com" gets sent to a malicious IP.
Defence: DNSSEC, encrypted DNS (e.g., 1.1.1.1 or 8.8.8.8).

C. ARP Poisoning (ARP Spoofing)
Works on local networks. The attacker sends fake ARP replies, linking their MAC address to the gateway's IP.
Result: All victim traffic passes through the attacker first (ideal for MitM).
Defence: Dynamic ARP Inspection (DAI) on switches, static ARP entries, network segmentation.




7. How Firewalls & Routers Work
-------------------------------

Routers (Layer 3)
Connect different networks. They use routing tables and IP addresses to forward packets. Can include basic ACLs for filtering.

Firewalls (Layers 3–7)
Filter traffic based on security rules.
- Stateless firewall: Inspects each packet alone, no memory.
- Stateful firewall: Remembers active connections and allows return traffic automatically.
- Next-Gen firewall (NGFW): Deep packet inspection, even looks at application data.

Simple analogy:
- Router = postman who reads the address on the envelope.
- Firewall = security guard who reads the address and opens the envelope to check the contents.

Example stateful rule:
  allow outbound TCP port 443 from any internal IP to any external IP
  allow inbound replies that match an existing connection
  deny everything else
