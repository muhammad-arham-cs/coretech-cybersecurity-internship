# Task 10: Incident Response and Digital Forensics

## 1. The Incident Response Lifecycle (NIST SP 800-61 r2 Framework)

A structured Incident Response (IR) capability minimizes operational downtime, preserves critical system metrics, and ensures legal and regulatory compliance during a security breach.

```text
  +-------------+      +-------------+      +-------------+
  | Preparation | ---> | Detection & | ---> | Containment,|
  |             |      |  Analysis   |      | Eradication |
  +-------------+      +-------------+      +-------------+
                                                   |
  +-------------+      +-------------+             v
  |   Lessons   | <--- |  Post-Inc.  | <--- |  Recovery   |
  |   Learned   |      |  Reporting  |      |             |
  +-------------+      +-------------+      +-------------+
```

1. **Preparation:** Establishing internal response capabilities, hardening host environments, deploying monitoring mechanisms (SIEM/EDR), and training the Cyber Incident Response Team (CIRT).
2. **Detection & Analysis:** Identifying anomalous alerts from centralized logs, verifying true positives against signature vectors, and assessing the overall impact scope.
3. **Containment, Eradication, & Recovery:** 
   * *Containment:* Isolating affected endpoints to stop threat propagation.
   * *Eradication:* Purging malicious code, identifying backdoors, and deleting compromised accounts.
   * *Recovery:* Restoring verified systems back into active production environments from clean backup points.
4. **Post-Incident Activity (Lessons Learned):** Conducting a thorough debrief session to document root-cause failures, evaluate team performance, and upgrade defensive controls to block similar attack vectors.

---

## 2. Ransomware Incident Response Plan for CoreTech Innovations

**Scenario Profile:** Inbound execution of a double-extortion ransomware strain encrypting administrative file shares and staging internal databases for exfiltration.

```text
[ALERT TRIPPED] ──> [HOST ISOLATION] ──> [SNAPSHOT PROTECTION] ──> [DECRYPTION EVAL] ──> [CLEAN RESTORE]
```

### Phase-by-Phase Operational Playbook

* **Phase 1: Identification & Triage**
  * Detect high-volume, rapid file modification alerts via active EDR rules.
  * Identify localized `.locked` or `.crypto` file extensions alongside raw ransom notes dropping inside user directory roots.
* **Phase 2: Immediate Containment Strategy**
  * **Network Layer Quarantine:** Programmatically isolate infected workstations and hypervisor clusters from the primary core switches. Disable local Wi-Fi subnets and drop external VPN tunnels instantly.
  * **Endpoint Containment:** Issue immediate host-isolation policies via the EDR console. Avoid hard-powering down running boxes to keep volatile RAM metrics alive for forensic collection.
* **Phase 3: Eradication**
  * Terminate persistent command-and-control (C2) scheduling mechanisms and registry run keys.
  * Audit active domain controller lists to drop any compromised service accounts or unauthorized administrative shadow users.
* **Phase 4: Recovery Execution Strategy**
  * Enforce standard offline snapshot restoration rules. Thoroughly verify backup image checksum paths before pushing files back onto production arrays.
  * **Corporate Policy Clause:** CoreTech Innovations explicitly prohibits processing extortion or financial cryptocurrency ransom transfers to threat syndicates. All assets must be rebuilt from verified offline points.

---

## 3. Digital Forensics Fundamentals & Chain of Custody

Digital forensics demands strict adherence to rigorous evidence acquisition rules to maintain structural integrity for potential legal evaluation.

### Core Scientific Pillars
* **Evidence Collection Prioritization (Order of Volatility):** Data must be collected based on how fast it degrades. CPU registers and cache memory are dumped first, followed by system RAM (volatile metrics), network connection states, local disk filesystems (non-volatile storage), and archival backups.
* **Chain of Custody Maintenance:** A continuous chronologically logged ledger documenting exactly who collected, handled, transferred, analyzed, and stored a specific piece of digital evidence. Any documentation gap nullifies evidence admissibility in court.
* **Bit-Stream Disk Imaging:** Making an exact bit-for-bit duplicate clone of the target physical storage media. Forensic investigators work exclusively on these duplicate master image containers (`.E01` or `.raw`) rather than the live evidence device to avoid altering local filesystems.
* **Cryptographic Verification (Data Integrity):** Generating an unalterable hashing footprint (MD5 or SHA-256) of the physical media immediately upon acquisition. This cryptographic signature is recalculated post-analysis; any hash value mismatch proves evidence alteration.

---

## 4. Forensic Artifact Investigation Triage

### Case Assessment Scope
* **Investigated File Resource:** `forensic_triage_target.E01`
* **Analysis Toolset:** Autopsy Forensic Browser Framework / FTK Imager

### Discovered Evidence Analysis

Using advanced forensic parsing matrices, the target master file system was safely dissected to isolate structural changes, file modifications, and timeline patterns.

1. **Malicious Staging Directory Discovery:** Parsing the `$MFT` (Master File Table) records revealed an anomalous runtime executable container dropped under `C:\Users\Public\Downloads\updater.exe`.
2. **Persistence Execution Analysis:** Forensic inspection of the offline Windows Registry hive located a custom execution string embedded within the `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` hive pointing explicitly back to the malicious updater file.

---

### 🔍 FORENSIC SOFTWARE ANALYSIS EVIDENCE
<img width="1151" height="934" alt="image" src="https://github.com/user-attachments/assets/d094824b-dfb7-4606-884e-a2a34c4bb53b" />


---

## 5. Centralized Log Analysis & SIEM Concepts

Security Information and Event Management (SIEM) systems function as the centralized command unit for modern Security Operations Centers (SOCs).

* **Log Aggregation:** Real-time collection of raw audit trails from network border firewalls, Windows Event Logs (Security.evtx), Linux syslog pipelines, database servers, and proxy controllers into a single pipeline.
* **Correlation Engines:** Specialized processing rules engineered to bind disparate system alerts together. For example, a SIEM links 50 consecutive failed SSH login alerts on an endpoint followed by a single successful login and immediate administrative group assignment into a unified high-priority brute-force alert tracker.

---

### 🔍 CENTRALIZED LOG ANALYSIS VIEW
<img width="1919" height="1010" alt="image" src="https://github.com/user-attachments/assets/a87b7154-cfd7-419a-af6a-c47fa8e08f6a" />


---

## 6. Corporate Post-Incident Engineering Template

**CoreTech Innovations Incident Report Matrix**  
**Document Code:** CT-IR-2026-TEMP  

### 1. Incident Metadata Profiles
* **Incident Reference Tracking ID:** CT-IR-2026-0619-X9
* **Primary Lead Investigator Asset:** Muhammad Arham (Roll: 24F-CS-116)
* **Initial Discovery Date/Timestamp:** June 19, 2026 @ 23:21:41 PKT
* **Containment Verification Timestamp:** June 19, 2026 @ 23:45:10 PKT

### 2. Impact Categorization Breakdown
* **Affected Asset Scopes (FQDNs / Host IPs):** CT-WKSTN-116 / 192.168.56.101
* **Data Privacy Classification Impacted:** `[ ] Public`  `[X] Internal Corporate`  `[ ] Restricted Sensitive PII`  `[X] Critical IP`
* **Operational Service Disruption Timeline:** `[ ] < 1 Hour`  `[X] 1 - 8 Hours`  `[ ] 8 - 24 Hours`  `[ ] > 24 Hours (Critical Block)`

### 3. Root-Cause Analysis Matrix (RCA)
*Detail the initial entry vector path utilized by threat agents (e.g., corporate phishing credential harvest, unpatched external edge asset vulnerability exploitation, third-party network interface bridge fault):*

```text
The threat actor gained initial execution on the endpoint via an unpatched application vulnerability, dropping a malicious staging binary into the public directory as cross-verified during the file system extraction in Autopsy (1_forensics_analysis.png). Post-execution, the threat agent leveraged a compromised system process layer (C:\Windows\System32\svchost.exe) to programmatically map and enumerate local administrative group memberships. This activity triggered an anomalous spike of over 30,000 security auditing entries, explicitly captured in the Windows Security Log under Event ID 4798 at 11:21:41 PM (2_siem_log_analysis.png), confirming active internal discovery maneuvers prior to host encryption.
```

### 4. Applied Remediation Track Log
| Executed Containment / Eradication Action | Targeted Endpoint Node | Resolution Sign-off |
| :--- | :--- | :--- |
| Isolated network adapter interface and terminated the malicious thread under svchost.exe | 192.168.56.101 | M. Arham (SecOps) |
| Extracted offline filesystem logs ($MFT) and registry run keys via Autopsy for IOC discovery | Forensic Workstation | M. Arham (SecOps) |
| Flushed active local group modifications and hardened local account management privileges | CT-WKSTN-116 | M. Arham (SecOps) |

### 5. Post-Incident Improvement Strategy (Lessons Learned)
*Identify the strategic operational flaws discovered during response actions and highlight required infrastructure architectural updates:*

```text
1. Implement strict application whitelisting policies and configure the corporate SIEM to immediately flag high-frequency local group enumeration behaviors (specifically monitoring Event ID 4798 logs).
2. Establish dynamic host isolation thresholds within the Endpoint Detection and Response (EDR) agents to automatically quarantine endpoints demonstrating rapid mass process auditing triggers.
```
