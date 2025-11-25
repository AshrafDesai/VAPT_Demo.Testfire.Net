## VAPT Assessment Report – demo.testfire.net##

📌 Overview

This repository contains the Vulnerability Assessment and Penetration Testing (VAPT) performed on the Altoro Mutual Online Banking Application hosted at https://demo.testfire.net
.

The assessment focused on identifying security weaknesses, evaluating real-world attack exposure, and providing remediation guidance to strengthen the application’s security posture.

### 1. Purpose of Assessment

* The VAPT engagement was conducted to:

* Identify vulnerabilities in the online banking application

* Assess risks to user accounts, financial data, and application integrity

* Evaluate the effectiveness of existing security mechanisms

* Provide mitigation strategies and a remediation roadmap

* The assessment simulated black-box testing, mimicking an external attacker with no internal knowledge of the system.

### 🧭 2. Scope of Work

Target URL: https://demo.testfire.net

Testing Type: Black Box

Methodology: OWASP Web Application Penetration Testing Guide

Environment: Production

####  Tools Used:

* Burp Suite

* Nmap

### ⚖️ 3. Limitations & Assumptions

* Testing strictly remained within the agreed scope

* No Denial-of-Service (DoS) attacks were performed

* Vulnerability exploitation was controlled and safe

* Only demonstration-level payloads were used

* Assessment was completed within a 7-day window to minimize impact

## 🧨 Summary of Findings

| No. | Vulnerability                           | Severity  |
|-----|------------------------------------------|-----------|
| 1   | SQL Injection                            | Critical  |
| 2   | Authentication Bypass                    | Critical  |
| 3   | HTTP Request Smuggling                   | High      |
| 4   | No Login Rate Limit                      | High      |
| 5   | Static Session ID                        | High      |
| 6   | Insecure Direct Object Reference (IDOR)  | High      |
| 7   | Reflected XSS                            | Medium    |
| 8   | Clickjacking                             | Medium    |
| 9   | Cleartext Password Submission            | Medium    |
| 10  | HTML Injection                           | Low       |
| 11  | Default Credentials                      | Info      |

### 🔍 5. Detailed Vulnerabilities Identified

#### 5.1 SQL Injection (Critical)

* Affected URL: /login.jsp

* Vulnerable Parameters: uid, passw

* Attackers can bypass login using classic payload:

``` bash
' OR '1'='1' --
``` 

* Impact: Full authentication bypass & potential database exposure

* Fix: Parameterized queries, input sanitization

### 5.2 Authentication Bypass (Critical)

* Default credentials: admin / admin

* Impact: Complete admin-level access

* Fix: Remove default creds, enforce strong passwords, MFA

### 5.3 HTTP Request Smuggling (High)

* Improper header parsing between front-end & backend servers

* Impact: Cache poisoning, unauthorized access

* Fix: Proper CL/TE header validation

### 5.4 No Rate Limiting (High)

* Unlimited login attempts

* Impact: Brute force & credential stuffing attacks

* Fix: Lockout thresholds, CAPTCHA, throttling

### 5.5 Static Session ID (High)

* Session IDs do not regenerate after login

* Impact: Session fixation → account takeover

* Fix: Regenerate and invalidate sessions properly

### 5.6 Reflected XSS (Medium)

* Vulnerable endpoint: search.jsp

Executable payload:
``` bash
<script>alert("XSS1")</script>
```
* Impact: Cookie theft, session hijacking

* Fix: Input/output encoding, CSP headers

### 5.7 Clickjacking (Medium)

* Missing X-Frame-Options header

* Impact: Forced clicks & unwanted user actions

* Fix: Frame protection headers

### 5.8 Cleartext Password Submission (Medium)

* Login transmitted over HTTP

* Impact: Credentials can be intercepted

* Fix: Enforce HTTPS (TLS 1.2+)

### 5.9 HTML Injection (Low)

* Unsanitized user input rendered directly

* Impact: UI manipulation & phishing risks

* Fix: Output encoding

### 5.10 Default Credentials (Info)

* Admin/admin accepted

* Impact: Unrestricted access

* Fix: Remove all factory-set credentials

### 5.11 IDOR – Insecure Direct Object Reference (High)

* Endpoint:
``` bash
/bank/showAccount?listAccounts=800000
```
* Non-admin user can access admin accounts by modifying account IDs

* Impact: Financial data exposure

* Fix: Enforce server-side authorization checks

### 🧩 7. Conclusion

The VAPT on demo.testfire.net revealed multiple critical and high-risk vulnerabilities that could lead to:

* Complete authentication bypass

* Account takeover

* Sensitive financial data disclosure

* Manipulation of user sessions

* Large-scale brute-force attacks

By implementing the recommended security controls, the application’s security posture will significantly improve. Continuous monitoring, regular VAPT cycles, and secure coding practices are essential for maintaining long-term safety.

### ⚠️ Educational Purpose Only

This assessment is conducted strictly for learning and cybersecurity training purposes.
No malicious activity was performed or intended.
