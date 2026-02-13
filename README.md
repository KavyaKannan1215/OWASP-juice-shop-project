
# OWASP-juice-shop-project

![Security Testing](https://img.shields.io/badge/Security-Penetration%20Testing-red)

![OWASP Top 10](https://img.shields.io/badge/OWASP-Top%2010-blue)

Hands-on security testing project demonstrating OWASP Top 10 vulnerabilities in Juice Shop.

---

## 📌 Project Overview
This project demonstrates a practical vulnerability assessment performed on the OWASP Juice Shop web application.  
The objective was to identify and exploit common web security vulnerabilities and understand how insecure configurations and improper input validation can impact application security.

Testing was performed in a controlled local environment for educational and learning purposes.

---

## 🎯 Objectives
- Identify common web application vulnerabilities
- Perform manual penetration testing
- Understand authentication and access control weaknesses
- Analyze application configuration exposure
- Document proof of concept with screenshots

---

## 🛠 Tools Used
- Web Browser
- Browser Developer Tools (Inspect → Network Tab)
- Localhost testing environment
- OWASP Juice Shop application

---

## 💻 Testing Environment
- Application: OWASP Juice Shop (locally hosted)
- Host URL: http://localhost:3000
- Operating System: Kali Linux (Virtual Machine)
- Browser: Google Chrome
- Testing Type: Manual penetration testing
- Network: Localhost isolated environment
  
---
## 🔐 Vulnerabilities Identified

---

### 1️⃣ SQL Injection — Login Bypass

**Description:**  
SQL Injection occurs when user input is not properly validated before being processed by the database.

**Test Performed:**  
Entered SQL command in login form.

**Result:**  
Authentication was bypassed and access was granted without valid credentials.

**Impact:**  
Unauthorized access to application accounts.

**Proof of Concept:**

Login page before SQL injection  
![SQL Login Page](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/SQLi_01_Login_Page.png.png)

SQL payload entered in login form  
![SQL Payload Entered](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/SQLi_02_Payload_Entered.png)

Authentication bypass successful  
![SQL Login Bypass](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/SQLi_03_Login_Bypass_Success.png)

Access granted to application dashboard  
![SQL Access Granted](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/SQLi_04_Access_Granted.png)


**Recommended Fix:**
- Use parameterized queries / prepared statements.
- Validate and sanitize all user inputs.
- Implement least privilege database access controls.


---

### 2️⃣ Cross-Site Scripting (XSS)

#### Reflected XSS — Search Bar

**Payload Used:**
```
<img src=x onerror=alert('XSS')>
```

**Test Performed:**  
Entered payload in search bar.

**Result:**  
JavaScript alert popup executed successfully.

**Impact:**  
Attackers can execute malicious scripts in user browsers.

**Proof of Concept:**  
![XSS Popup](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/xss_02.png)

**Recommended Fix:**
- Sanitize and validate all user input before rendering.
- Implement proper output encoding.
- Use Content Security Policy (CSP) to restrict script execution.


---

#### DOM-Based XSS

**Payload Used:**
```
<iframe src="javascript:alert(`xss`)">
```
**Description:**  
DOM XSS (Document Object Model Cross-Site Scripting) is a type of Cross-Site Scripting attack that happens entirely in the browser, not on the server.

**Test Performed:**  
Executed DOM XSS payload in application.

**Result:**  
Browser executed script via client-side processing.

**Impact:**  
Client-side script manipulation without server interaction.

**Proof of Concept:**  
![DOM XSS](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/DOM_XSS.png)

**Recommended Fix:**
- Avoid inserting untrusted data into the DOM using JavaScript.
- Use secure DOM APIs (textContent instead of innerHTML).
- Validate and sanitize client-side input.

---

### 3️⃣ Broken Access Control — Admin Section

**Description:**  
Restricted administrative functionality was accessible without authorization.

**Test Performed:**  
Entered `/administration` in search bar.

**Result:**  
Administrative panel became accessible.

**Impact:**  
Unauthorized users can access sensitive management features.

**Proof of Concept:**  
![Admin Access](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/Broken_access.png)

**Recommended Fix:**
- Enforce server-side authorization checks for all restricted endpoints.
- Implement role-based access control (RBAC).
- Restrict direct URL access to administrative functions.

---

### 4️⃣ Sensitive Data Exposure — robots.txt

**Description:**  
Sensitive application paths exposed through configuration files.

**Test Performed:**  
Accessed `/robots.txt`.

**Observation:**  
Hidden directory `/ftp` discovered.

**Impact:**  
Exposure of internal directories and potential sensitive data.

**Proof of Concept:**  
![Robots File](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/sensitive_dataexposure.png)

**Recommended Fix:**
- Do not expose sensitive paths in configuration files.
- Restrict access to internal directories.
- Implement proper file permission controls.

---

### 5️⃣ Insecure Direct Object Reference (IDOR) — Attempted

**Description:**  
IDOR occurs when attackers manipulate object identifiers to access unauthorized data.

**Test Performed:**  
Modified product ID values in requests.

**Result:**  
No unauthorized data returned.

**Conclusion:**  
ID manipulation attempt unsuccessful in this case.

**Proof of Concept — Unauthorized Request (No Authorization Header):**

![IDOR Unauthorized Request](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/IDOR.png)

**Recommended Fix:**
- Validate user authorization before granting access to resources.
- Use indirect reference mapping instead of direct object IDs.
- Implement access control checks for every request.

---

### 6️⃣ Security Misconfiguration — Exposed FTP Directory

**Description:**  
Improper configuration allowed direct access to internal files.

**Test Performed:**  
Accessed `/ftp` directory revealed in robots.txt.

**Result:**  
Multiple internal files were publicly accessible.

**Impact:**  
Information disclosure and exposure of sensitive data.

**Proof of Concept:**  
![FTP Files](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/security_misconfiguration.png)

**Recommended Fix:**
- Disable public access to internal directories.
- Remove unnecessary files from production environments.
- Apply secure configuration and hardening practices.


---

### 7️⃣ CSRF Security Observation — Token Based Protection

**Description:**  
CSRF attacks attempt to perform actions on behalf of authenticated users without consent.

**Test Performed:**  
Added product to cart and inspected network request in browser developer tools.

**Observation:**  
Authorization header contained JWT bearer token in the request.

**Conclusion:**  
Application uses token-based authentication which reduces CSRF risk.

**Proof of Concept:**

Full browser window showing POST request in Network tab  
![CSRF POST Request](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/csrf_01.png)

Network tab highlighting Authorization Bearer JWT token  
![Authorization JWT Token](https://github.com/KavyaKannan1215/OWASP-juice-shop-project/blob/main/media/csrf_02.png)

**Recommended Fix:**
- Continue using token-based authentication.
- Implement anti-CSRF tokens for sensitive operations.
- Validate request origin and headers.

---

## 📊 Summary of Findings

| Vulnerability | Status | Severity |
|---|---|---|
| SQL Injection | Confirmed | High |
| Cross Site Scripting | Confirmed | High |
| Broken Access Control | Confirmed | Critical |
| Sensitive Data Exposure | Confirmed | Medium |
| IDOR | Attempted | Unsuccessful |
| Security Misconfiguration | Confirmed | Medium |
| CSRF | Protected | Low |

---

## 🧠 Key Learning Outcomes
- Practical penetration testing experience
- Authentication and authorization testing
- API request inspection
- Input validation weaknesses
- Security configuration risks
- Exposure of hidden directories
- Understanding token-based authentication

---

## ⚠ Ethical Notice
Testing was performed in a controlled local lab environment for educational purposes only.

---

## 👩‍💻 Author
Kavya Kannan  
Cybersecurity Enthusiast  
B.Tech ECE Graduate
