**Task 5: Vulnerability Scanning Concepts and Tools**
**Target Domain / Host:** localhost (127.0.0.1) & authorized testing nodes  

---

**1. Understanding Vulnerability Scanning**

Vulnerability scanning is an automated asset-discovery and inspection process used to identify, categorize, and characterize security gaps, misconfigurations, and outdated software versions within an organizational network infrastructure. 

**Why it is Critical to Modern Security Operations:**
* Proactive Risk Mitigation: It uncovers exploitable gaps before external threat actors can locate and weaponize them.
* Operational Compliance: Regular compliance validation requires routine enterprise perimeter and internal mapping scans.
* Patch Verification: Confirms that newly applied security patches or systems hardening policies are active and functioning correctly.

---

**2. Local Infrastructure Inspection (Nmap Port Scanning)**

Nmap (Network Mapper) serves as an industry-standard network discovery utility used to perform low-level host verification, port discovery, and operating system analysis.

**Command Executed for Local Host Analysis:**
nmap -p 1-1024 127.0.0.1

<img width="582" height="165" alt="image" src="https://github.com/user-attachments/assets/b1bc5cf6-c52f-42c3-bb75-6483ee43108c" />


**Analysis of Findings:** The local loopback environment was scanned across the primary system ports. Active exposed listener services were recorded along with their corresponding socket states.

---

**3. Advanced Service Detection & Scripting (Nmap Scripting Engine - NSE)**

The Nmap Scripting Engine (NSE) allows automation of custom scripts to perform more detailed service discovery, banner grabbing, and structural vulnerability checks during active enumeration phases.

**Command Executed for Targeted Service Detection:**
nmap -sV --script=banner,http-vuln* 127.0.0.1

<img width="774" height="174" alt="image" src="https://github.com/user-attachments/assets/e0018de5-55b8-4f71-ab3f-4f889c88d0f8" />


**Analysis of Findings:** Utilizing the service validation switch (-sV) coupled with specific script parameters extracted structural banners from operational applications, providing granular information regarding version fingerprints and potential misconfigurations.

---

**4. The CVE (Common Vulnerabilities and Exposures) System**

The CVE dictionary is a standardized, publicly accessible glossary of information security vulnerabilities and exposures maintained by the MITRE Corporation. 

**Role in Vulnerability Management:**
* Unambiguous Identification: It assigns a unique identifier (e.g., CVE-YYYY-NNNNN) to specific flaws, ensuring security engineering teams, vendors, and researchers refer to the exact same issue across distinct systems.
* Interoperability: Enables absolute integration across distinct scanner utilities, patch management consoles, and firewalls using a uniform naming architecture.

---

**5. Real-World CVE Reference Library**

* **CVE-2021-44228 (Log4Shell)**
  * Severity Rating: 10.0 (Critical)
  * System Impact: Remote Code Execution (RCE) flaw within Apache Log4j 2 utility due to recursive JNDI log parsing.

* **CVE-2017-0144 (EternalBlue)**
  * Severity Rating: 8.8 / 9.3 (High / Critical depending on CVSS version)
  * System Impact: Memory management flaw in Microsoft Windows SMBv1 protocol allowing arbitrary code execution via malformed packets.

* **CVE-2014-0160 (Heartbleed)**
  * Severity Rating: 7.5 (High)
  * System Impact: Information disclosure bug via a buffer over-read flaw in the TLS Heartbeat extension within OpenSSL libraries.

* **CVE-2019-0708 (BlueKeep)**
  * Severity Rating: 9.8 (Critical)
  * System Impact: Pre-authentication remote code execution flaw within Remote Desktop Services (RDS) on legacy Windows deployments.

* **CVE-2024-3094 (XZ Utils Backdoor)**
  * Severity Rating: 10.0 (Critical)
  * System Impact: Malicious code injection inside build scripts for xz compression utilities, targeting OpenSSH server authentication.

---

**6. The CVSS Scoring Architecture Explained**

The Common Vulnerability Scoring System (CVSS) is an open framework used to communicate the characteristics and severity of software vulnerabilities. It converts multi-faceted engineering realities into a numerical score from 0.0 to 10.0.

**Core Metric Dimensions:**
* Attack Vector (AV): Defines the accessibility path of an exploit (Network, Adjacent, Local, Physical).
* Attack Complexity (AC): Reflects the amount of environmental control or setup required by an attacker to successfully compromise the asset.
* Privileges Required (PR): Assesses the authorization level required before triggering an attack vector (None, Low, High).
* User Interaction (UI): Measures whether a human operator must perform an explicit action (e.g., clicking a link) to initiate the payload.
* Scope (S): Tracks whether a vulnerability on one component impacts resources managed by a completely different security authority.
* Impact Metrics (C, I, A): Evaluates the absolute loss of Confidentiality, Integrity, and Availability across data processing environments.

---

**7. Institutional Vulnerability Scan Report Template**

Executive Overview
* Assessment Scope: [Insert Assets / CIDR Blocks / Target IPs]
* Scanning Schedule: [Insert Commencement and Completion Timestamps]
* Lead Security Assessor: [Insert Auditor Identity / Intern Identification]

Discovered Exposure Summary
* Total Risks Detected: [Count]
* Critical Severity (CVSS 9.0 - 10.0): [Count]
* High Severity (CVSS 7.0 - 8.9): [Count]
* Medium Severity (CVSS 4.0 - 6.9): [Count]
* Low Severity (CVSS 0.1 - 3.9): [Count]

Technical Vulnerability Ledger
* Vulnerability Asset Designation: [e.g., Web App Server 01 - 192.168.1.50]
* Associated Registry Listing: [e.g., CVE-YYYY-NNNN]
* Calculated CVSS Baseline Vector: [e.g., CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H]
* Remediation Strategy: [e.g., Apply vendor hotfix, update package to version X, or restrict port access via localized firewall rules]

---

**8. Ethical Declaration**

All local scanning protocols, script execution configurations, and device enumerations documented in this dossier were executed solely against local loopback architectures (127.0.0.1) and authorized lab resources. No external production environments were accessed without proper authorization.
