# Task 11: Final Cybersecurity Project Development - Phase 1 Progress

## Project Overview
* **Project Chosen:** Web Application Security Testing Report (OWASP Methodology)
* **Target Application:** CoreTech Innovations Internal Portal (`portal.coretech.local`)
* **Lead Security Engineer:** Muhammad Arham
* **Project Status:** Phase 1 - Initial Work & Target Scoping (In-Progress)

---

## 1. Chosen Project & Objectives
For the final capstone evaluation, the **Web Application Security Testing Report** option was selected. The objective is to evaluate CoreTech Innovations' web infrastructure components against critical flaws highlighted within the industry-standard **OWASP Top 10 Framework**.

---

## 2. Testing Methodology
The initial assessment phase deploys the **OWASP Web Security Testing Guide (WSTG)** lifecycle to maintain analytical consistency:
* **Information Gathering:** Mapping system directory footprints and identifying web application daemon behaviors.
* **Vulnerability Assessment:** Executing focused manual input fuzzing against application-layer boundary vectors.

---

## 3. Initial Findings & Risk Ratings
Initial automated scanning and manual boundary validations have successfully flagged two high-severity logical issues within the target application:

### Finding 1: Authentication Bypass via SQL Injection (SQLi)
* **Affected Endpoints:** `/api/v1/auth/login`
* **Severity Rating:** 🔴 Critical
* **Initial Analysis:** The application takes user-supplied username input parameters and appends them directly into raw SQL string expressions without parameterization filters.

### Finding 2: Reflected Cross-Site Scripting (XSS)
* **Affected Endpoints:** `/showcase/render/payload`
* **Severity Rating:** 🟠 High
* **Initial Analysis:** Text area input components reflect data strings directly back onto client browser display views without context-aware sanitization filters.

---

## 4. Phase 1 Progress Conclusion
The target application footprinting and phase one vulnerability classification sequences are fully completed. Initial proof-of-concept verification frameworks have been drafted. Full exploitation tracking, deep risk evaluations, and defensive coding implementations will be delivered in the final report within the Task 12 folder.
