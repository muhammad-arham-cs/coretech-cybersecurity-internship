# Task 9: Ethical Hacking and Penetration Testing Basics

## 1. Penetration Testing Methodology Phases

A professional penetration test follows a structured, industry-standard lifecycle to ensure comprehensive evaluation and risk mitigation within legal boundaries.

```text
[RECONNAISSANCE] ➔ [SCANNING & VULN ASSESSMENT] ➔ [EXPLOITATION] ➔ [POST-EXPLOITATION] ➔ [REPORTING]
```

1. **Reconnaissance (Information Gathering):** The preparatory phase where the tester gathers raw intelligence about the target infrastructure. This can be passive (gathering OSINT without touching the target) or active (interacting directly with the target infrastructure).
2. **Scanning & Vulnerability Assessment:** Utilizing network tools to map live hosts, open ports, active operating systems, and software versions. These components are then evaluated against threat intelligence databases to discover known security flaws (CVEs).
3. **Exploitation:** Safely abusing verified software vulnerabilities or logical misconfigurations to bypass security restrictions and gain unauthorized access or execute code on the target system.
4. **Post-Exploitation:** Determining the true value and risk exposure of the compromised system. Activities include pivoting to adjacent subnets, upgrading shell privileges, auditing local configurations, and identifying sensitive administrative data.
5. **Reporting (Documentation):** The final operational deliverable. Aggregating all technical findings, attack paths, risk severity mappings, and step-by-step remediation roadmaps into a structured document for client stakeholders.

---

## 2. Controlled Lab Environment Architecture

To ensure 100% safety and legality, all activities are executed within a strictly isolated, host-only virtual network container.

* **Hypervisor Platform:** Oracle VirtualBox
* **Attacker Framework Machine:** Kali Linux (Rolling Edition)
* **Vulnerable Target Platform:** Metasploitable 2 Linux Sandbox VM
* **Network Isolation Configuration:** Host-Only Adapter Interface Network (`vboxnet0` IP Space: `192.168.56.0/24`)

---

### 1: LAB TOPOLOGY & ENVIRONMENT SETUP
<img width="655" height="229" alt="image" src="https://github.com/user-attachments/assets/caa03467-f6a6-4081-b6f6-f555aa125614" />


---

## 3. Metasploit Framework Operational Handbook

The Metasploit Framework (MSF) is a core system for infrastructure vulnerability exploitation. Below are the structural commands deployed during testing workflows:

| Command Syntax | Operational Purpose |
| :--- | :--- |
| `msfconsole` | Initializes the Metasploit interactive command-line environment interface. |
| `search [exploit_name]` | Queries the local framework database for specific module signatures or CVE references. |
| `use [exploit/path/name]` | Selects and loads an active exploit module into workspace memory. |
| `show options` | Lists all mandatory configuration targets, variables, and flags required for execution. |
| `set RHOSTS [Target_IP]` | Configures the destination target remote host IP address parameter. |
| `set LHOST [Attacker_IP]` | Specifies the local listener IP address interface on the attacker machine for reverse connections. |
| `set PAYLOAD [payload_string]` | Selects the specific shellcode architecture to execute on successful exploitation (e.g., Meterpreter). |
| `exploit` or `run` | Launches the payload package payload against the specified vector target vulnerability. |

---

## 4. Professional Penetration Testing Assessment Report

**Document Reference:** CT-PT-REP-2026-V1  
**Target Assessment Range:** Isolated Laboratory System Environment  
**Assigned Tester:** Muhammad Arham  
**Execution Date:** June 19, 2026  

### 1. Executive Vulnerability Summary
A comprehensive security assessment was completed against the target host environment. Testing identified critical service structural flaws, including a backdoored communication application running on unhardened network nodes. Access parameters were successfully compromised, proving that unpatched legacy software compromises system integrity.

### 2. Technical Finding Detail: Root Remote Code Execution (RCE)
* **Vulnerable Service Vector:** `vsftpd` (Version 2.3.4)
* **Identified Active Port:** TCP Port 21 (FTP Service daemon)
* **Associated CVE Mapping:** CVE-2011-2523 (vsftpd v2.3.4 Backdoor Vulnerability)
* **Assigned Threat Severity:** 🔴 10.0 Critical (CVSS v3 Base Score)

#### Technical Breakdown & Attack Vector
The target system runs an unpatched edition of the `vsftpd 2.3.4` application. This particular version contains an intentional backdoor modification that drops an instant interactive root shell listener on TCP port 6200 whenever a login attempt is initiated using a username ending with a smiley face symbol `:)`.

#### Proof of Concept (Exploitation Steps)
1. Initialize the Metasploit console and locate the relevant exploit:
   ```bash
   msfconsole
   search vsftpd
   use exploit/unix/ftp/vsftpd_234_backdoor
   ```
2. Configure targets and trigger execution parameters:
   ```bash
   set RHOSTS 192.168.56.101
   exploit
   ```
3. The framework payload triggers the smiley verification logic, catches the command shell execution trap, and drops an active shell execution line running under absolute `root` privileges.

---

### : SUCCESSFUL SERVICE EXPLOITATION
<img width="1256" height="453" alt="image" src="https://github.com/user-attachments/assets/ed0ded73-d19a-4d30-8ac8-c544c8bb5bb8" />

---

### 3. Comprehensive Remediation Strategy
* **Immediate Patch Management:** Completely remove the compromised `vsftpd 2.3.4` implementation software package. Upgrade the local file transfer engine to the latest stable edition of `vsftpd` or migrate to a secure protocol layer like SFTP (SSH File Transfer Protocol).
* **Network Segmentation & Boundary Enforcement:** Update local host firewall constraints to block public external traffic from processing open connections on legacy administrative ports unless routed through an explicitly encrypted VPN channel.
```
