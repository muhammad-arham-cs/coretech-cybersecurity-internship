# Task 12: Final Project Report and Presentation

## Project Control Metrics
* **Lead Information Security Consultant:** Muhammad Arham
* **Academic Institution:** Dawood University of Engineering and Technology (DUET)
* **Target Environment:** CoreTech Innovations Enterprise Portal (`portal.coretech.local`)
* **Framework Standard:** OWASP Web Security Testing Guide (WSTG v4.2)
* **Classification:** Final Capstone Evaluation Deliverable
* **Date of Evaluation:** June 26, 2026

---

## 1. Executive Summary
CoreTech Innovations mandates strict, proactive verification across its digital web ecosystem to protect proprietary corporate data and maintain operational integrity. This final report concludes the Web Application Security Testing and Vulnerability Assessment (VAPT) performed against the CoreTech Internal Portal instance. 

Adhering strictly to the **OWASP Top 10 Framework**, structured boundary evaluations successfully confirmed critical input-sanitization flaws within active production routing layers. These logical gaps expose backend relational database schemas and allow unauthenticated client-side code execution. This document outlines the structured testing methodology, granular risk metrics, proof-of-concept validations, and engineering recommendations required to permanently secure the application infrastructure.

---

## 2. Scope and Objectives
The security assessment targeted specific web components and interaction layers within an isolated laboratory sandbox mirroring the core enterprise portal:

* **Target URL:** `http://portal.coretech.local:8000` (Staging Mirror Instance)
* **Scoping Boundaries:** Authentication routes, dynamic user-parameter handlers, session controllers, and presentation rendering arrays.
* **Core Objectives:**
  1. Map the external surface footprint and active service daemons of the web application container.
  2. Discover, validate, and document security flaws using non-disruptive Proof-of-Concept (PoC) vectors.
  3. Quantify identified vulnerabilities using the industry-standard Common Vulnerability Scoring System (CVSS v3.1).
  4. Provide standard architectural code corrections to completely eradicate the identified vulnerabilities.

---

## 3. Methodology Used
The audit implemented the structured, phase-based **OWASP Web Security Testing Guide (WSTG)** lifecycle to maintain absolute analytical accuracy:

1. **Information Gathering & Reconnaissance:** Passively and actively enumerating directory roots, reviewing HTTP headers, and tracking active processing parameters.
2. **Vulnerability Analysis:** Running targeted manual parameter fuzzing and boundary injections against forms to note anomalous system responses.
3. **Exploitation & Validation:** Injecting contextual safe string payloads to explicitly confirm the presence of structural application bugs without impacting data availability.
4. **Remediation Engineering:** Designing defense-in-depth code patches and header policies to close the entry pathways permanently.

---

## 4. Assessment Tools Used
* **Nmap (Network Mapper):** Deployed for surface footprinting, active port mapping, and initial service engine detection.
* **Burp Suite Community Edition:** Utilized as an upstream HTTP interception proxy to execute request tampering and manual input fuzzing parameters.
* **Mozilla Firefox Web Developer Tools:** Employed to inspect real-time DOM mutations, audit cookie structures, and review client-side script processing blocks.

---

## 5. Technical Findings with Severity Ratings

### Finding 1: Authentication Bypass via SQL Injection (SQLi)
* **OWASP Mapping:** A03:2021-Injection
* **Affected Component:** `/api/v1/auth/login` (Username Field Input)
* **Assigned Severity:** 🔴 **9.8 Critical** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)

#### Technical Analysis
The login parameter processor fails to filter incoming data strings through backend validation queries before running them against the relational database. By passing unchecked string escape sequences, an attacker can manipulate the query logic. This completely drops standard username and password evaluation restrictions, returning a valid administrative session token to the browser interface.

#### Proof of Concept (PoC) Payload
Our verified testing string sequence injected into the input field is: `' OR 1=1 -- -`

---

### Finding 2: Session Hijacking via Reflected Cross-Site Scripting (XSS)
* **OWASP Mapping:** A03:2021-Injection
* **Affected Component:** `/showcase/render/payload` (Textarea Form Controller)
* **Assigned Severity:** 🟠 **8.2 High** (CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:L/A:N)

#### Technical Analysis
The application accepts data payloads from the showcase form and dynamically reflects them straight back to the user view without sanitization or output entity encoding. This allows an external agent to craft an input string containing malicious browser scripts. When a user views the reflected output, the browser executes the script block, allowing potential session token exfiltration.

#### Proof of Concept (PoC) Payload
Our verified testing string sequence injected into the text field is: `<script>alert("XSS Vulnerability Verified")</script>`

---

## 6. Comprehensive Risk Analysis
The combination of critical authentication bypass parameters and client-side execution zones introduces significant risk vectors:

* **Confidentiality Loss:** Exploiting the SQL Injection flaw allows unauthenticated read access to internal user tables, enabling threat actors to extract database schemas and corporate account directories.
* **Integrity Loss:** Exploiting Reflected XSS enables attackers to intercept browser session states, perform unauthorized account actions, or execute client-side state manipulation.

---

## 7. Remediation Recommendations for CoreTech Innovation

### 1. Enforce Parameterized Queries (Prepared Statements)
All application database connection components must be immediately migrated to typed **Prepared Statements**. Database drivers must strictly isolate incoming data values from the compilation logic of the SQL statement:

```java
// Secure Implementation Pattern Blueprint
String query = "SELECT * FROM accounts WHERE username = ? AND password = ?";
PreparedStatement stmt = connection.prepareStatement(query);
stmt.setString(1, userSuppliedUsername);
stmt.setString(2, userSuppliedPassword);
ResultSet results = stmt.executeQuery();
```

### 2. Context-Aware Output Encoding
Deploy robust, context-aware HTML entity encoding across all user data rendering points. Reserved characters (`<`, `>`, `"`, `'`, `/`, `&`) must be safely converted to their corresponding display entities before parsing in browser layout engines.

### 3. Deploy Content Security Policy (CSP) Headers
Configure enterprise web servers to broadcast strict HTTP response headers, restricting script execution strictly to verified local origins:
```http
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';
```

---

## 8. Conclusion
The final security assessment of the CoreTech Innovations portal identifies vulnerabilities that require prompt technical resolution. Implementing database query parameterization and context-aware output encoding will remove the root vulnerabilities, bringing the application baseline in line with enterprise cybersecurity standards.

---

## 9. Appendix: Laboratory Evidence & Technical Demo

### Forensic Evidence Artifacts

#### Evidence 1: Active Input Parameter Exploitation (SQLi Bypass Proof)
*The screenshot below tracks the successful drop of authentication restrictions when the database parsing layer evaluates malicious input.*  
<img width="899" height="555" alt="image" src="https://github.com/user-attachments/assets/7ee11c34-9cf1-498e-bb40-6b4791b158a9" />


#### Evidence 2: Successful Application Context DOM Alert (XSS Validation Proof)
*The screenshot below documents the execution of client-side validation logic when loading unencoded input scripts.*  
<img width="918" height="459" alt="image" src="https://github.com/user-attachments/assets/7d4a119f-a480-45ff-9c80-2c1646b9c2ed" />


The video means there is no project to take the video and i didnt make the video of the documentation i just provided the screnshots as the proof that i did all the tasks i took the help of ai ofcourse but i used all these tools so thats it from myself.

