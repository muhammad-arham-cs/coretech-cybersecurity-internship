# Task 8: Network Security and Firewall Configuration

## 1. Architectural Overview of Firewall Technologies

Firewalls serve as the primary line of defense in network security by controlling incoming and outgoing traffic based on predetermined security rules. Modern network infrastructures employ three primary classes of firewalls:

| Firewall Type | Operational Layer | Core Mechanism | Pros & Cons |
| :--- | :--- | :--- | :--- |
| **Packet Filtering** | Network Layer (Layer 3) | Inspects individual packets in isolation based strictly on source/destination IP addresses, protocols, and port numbers. | **Pros:** Extremely fast, low resource overhead.<br>**Cons:** Stateless; easily bypassed by advanced spoofing attacks. |
| **Stateful Inspection** | Transport Layer (Layer 4) | Tracks the state of active network connections (TCP handshake states: SYN, SYN-ACK, ACK) and maintains a state table to validate if packets belong to an established session. | **Pros:** Secure session awareness, blocks unsolicited traffic natively.<br>**Cons:** Higher memory requirements to track state tables. |
| **Application Layer (WAF / NGFW)** | Application Layer (Layer 7) | Deep Packet Inspection (DPI) that evaluates the actual data payload content (HTTP headers, SQL strings, FTP commands) to detect application-specific attacks. | **Pros:** Detects complex web defects (SQLi, XSS, malicious payloads).<br>**Cons:** Highly resource-intensive; causes processing latency. |

---

## 2. Linux Stateful Firewall Configuration (`iptables`)

Below is the hardened script utilized to configure native kernel space filtering on the Linux system. This ruleset enforces a strict **Default Deny** posture, permits critical administrative access (SSH), limits standard web traffic ports, and blocks unauthorized probing.

### Step-by-Step Configuration Commands

```bash
# 1. Flush all existing active rules and chains to establish a clean state
sudo iptables -F
sudo iptables -X

# 2. Set default restrictive policies (Drop all inbound and forwarded traffic, allow outbound)
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# 3. Allow all loopback traffic (Crucial for local system services interaction)
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT

# 4. Permit packets belonging to already established or related active sessions
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# 5. Securely open administrative channels: Allow inbound SSH traffic (Port 22)
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT

# 6. Open standard public web services parameters: Allow HTTP (80) and HTTPS (443)
sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT

# 7. Explicitly deny and log remaining unauthorized entry attempts
sudo iptables -A INPUT -j LOG --log-prefix "IPTABLES_DENIED: "
```

---

### 1: IPTABLES CONFIGURATION VERIFICATION
<img width="701" height="206" alt="image" src="https://github.com/user-attachments/assets/5f6a64fe-40a1-4ddb-b795-23c66731b49d" />

---

## 3. Deep Dive: Intrusion Detection (IDS) & Prevention Systems (IPS)

While firewalls guard network borders based on port logic, IDS/IPS platforms continuously monitor underlying traffic parameters to detect behavioral anomalies or known threat fingerprints.

### Snort vs Suricata Comparison
* **Snort:** Natively single-threaded, operating as a signature-based engine. It utilizes lightweight processing parameters to match passing packet components against an extensive database of known attack rules.
* **Suricata:** A modern, multi-threaded IDS/IPS architecture engineered to scale seamlessly across multi-core processors. It supports native application-layer logging, protocol identification, and file extraction logic.

### Network Deployment Mechanisms
1. **IDS Mode (Passive Sniffing):** Connected to a network switch via a **SPAN (Switch Port Analyzer)** port or a network **TAP**. The system receives a copy of the traffic stream out-of-band. It generates real-time alerts without introducing network latency.
2. **IPS Mode (Inline Mitigation):** Deployed directly in-line with the data flow path (packets pass through the IPS physical interfaces). If a signature evaluates as malicious, the system drops the packet instantly, truncating the attack sequence before it reaches internal servers.

---

## 4. Virtual Private Networks (VPN) & Traffic Obfuscation

A Virtual Private Network (VPN) extends a private enterprise intranet topology over a public infrastructure (the internet) while ensuring absolute data safety through advanced encapsulation and cryptographic wrappers.

### Core Cryptographic Pillars
* **Tunneling:** Encloses the original payload packet inside an entirely new outer transport packet envelope to abstract private IP routing schema away from public nodes.
* **Encryption (Confidentiality):** Utilizes symmetric encryption algorithms (AES-256-GCM or ChaCha20) to render the raw data payload completely unreadable to intercepting routers or ISP nodes.
* **Data Integrity:** Embeds asymmetric hashing checks (HMAC-SHA256) to verify that packets are not manipulated, corrupted, or replayed during transit across public channels.

Modern implementations heavily deploy protocols like **OpenVPN** (high configuration flexibility running over TLS) and **WireGuard** (modern, lightweight architecture executing fast cryptography inside the Linux kernel).

---

## 5. Wireshark Packet Analysis & Triage Report

### Assessment Scope
* **Analyzed Target File:** `network_capture_triage.pcap`
* **Analysis Methodology:** Application-layer dissection, TCP conversational stream indexing, and threshold tracking.

### Identified Threat Vectors & Suspicious Patterns

#### Finding 1: Automated Horizontal TCP Port Scan Tracking
* **Observation:** A single remote source host IP initiated hundreds of consecutive TCP packets targeted towards ascending port counts on an internal server within a millisecond timeline.
* **Traffic Signature:** Packets exclusively carried the **SYN Flag** enabled, without completing the subsequent three-way handshake sequence (Stealth SYN scanning behavior).
* **Mitigation Recommendation:** Configure adaptive firewall blocking limits or deploy `fail2ban` metrics to throttle source hosts demonstrating rapid connection requests.

#### Finding 2: Cleartext Sensitive Data Harvesting
* **Observation:** Reassembling unencrypted TCP connection streams traversing port 21 (FTP) exposed administrative credentials explicitly within plaintext lines.
* **Traffic Signature:** `USER admin` and `PASS SecretPassword123` strings cleanly resolved inside the ASCII decoding parameters window.

---

### 2: WIRESHARK PACKET TRAFFIC ANALYSIS
<img width="2226" height="1633" alt="image" src="https://github.com/user-attachments/assets/fd03ec8a-ca3d-438e-a1a8-5a766d3e5c51" />


---

## 6. Corporate Network Security Policy

**Document Reference:** CT-SEC-POL-2026-V1  
**Organization:** CoreTech Innovations  
**Effective Date:** June 19, 2026  
**Classification:** Internal Corporate Reference Only  

### 1. Objective and Policy Statement
The core objective of this document is to define strict operational boundaries governing the network topology, perimeter firewall parameters, and data transport layers across CoreTech Innovations infrastructure. This policy guarantees security containment, asset auditing, and absolute alignment with global standard controls.

### 2. Perimeter Hardening Rules (Firewall Policy)
* **Explicit Default Strategy:** All perimeter, access layer, and internal zone firewall devices must enforce a strict **Default Deny (Implicit Drop)** stance for all inbound connections. Traffic must be explicitly white-listed before passing.
* **Administrative Access Control:** Management endpoints (SSH, RDP, Web GUIs) are banned from public network visibility. All remote operational controls must go through a designated corporate bastion host or internal VPN appliance gateway.
* **Active Rules Retraction:** Rulesets must undergo a comprehensive audit review every six months. Any legacy, unused, or redundant access parameters must be purged immediately.

### 3. Remote Workforce Protocol (VPN Access Control)
* **Mandatory Gateway Tunneling:** All off-site personnel accessing internal CoreTech software assets, source files repositories, or databases must connect exclusively through the corporate VPN appliance gateway.
* **Access Control Pillars:** VPN sessions must be validated using multi-factor token verification (MFA) linked to Active Directory identity rosters.
* **Endpoint Sanitation Evaluation:** Remote computers connecting to the VPN infrastructure must pass automated posture checks verifying the execution of a live, updated endpoint protection suite.

### 4. Incident Escalation Matrix & Triage Strategy
Upon detection of anomalous network traffic patterns (e.g., active distributed denial-of-service spikes, verified IDS alert triggers, or boundary policy bypass violations), the security operations team must execute the following response cycle:

```text
[PHASE 1: IDENTIFICATION] -> Validate signature alerts via independent SIEM log sources.
[PHASE 2: CONTAINMENT]    -> Drop offending source IP pools at the border firewall interface.
[PHASE 3: ERADICATION]   -> Isolate affected subnets or nodes from internal routing loops.
[PHASE 4: POST-MORTEM]   -> Draft an updated IOC report to update detection filters.
```
```
