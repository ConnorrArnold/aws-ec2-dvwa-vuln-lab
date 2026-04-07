# aws-ec2-dvwa-vuln-lab

Hands-on web application vulnerability lab using DVWA on AWS EC2. Covers SQL injection, XSS, CSRF, and file upload bypass with payloads and mitigations.

## Environment

| Component | Details |
|-----------|---------|
| Cloud | AWS EC2 |
| OS | Ubuntu 22.04 |
| Web Server | Apache 2.4.58 |
| Database | MySQL MariaDB |
| Application | DVWA |
| Security Level | Low |

## Vulnerabilities Covered

| # | Vulnerability | Severity | CVSS | Status |
|---|--------------|----------|------|--------|
| 1 | SQL Injection | Critical | 9.8 | Complete |
| 2 | XSS Reflected and Stored | High | 7.4 | Complete |
| 3 | CSRF | High | 8.0 | Complete |
| 4 | File Upload Bypass | Critical | 9.8 | Complete |

## Key Findings

### 1. SQL Injection (Critical)
The User ID field passed input directly into the MySQL query with no sanitization. Full database enumeration was achieved using UNION-based injection. All 5 user accounts and MD5 password hashes were dumped. ModSecurity WAF blocked all payloads with 403 Forbidden until disabled for lab purposes.

### 2. XSS Reflected and Stored (High)
User input rendered into HTML without output encoding allowed arbitrary script execution. Session cookie exposed via document.cookie. Stored XSS persisted in the database and fired automatically for every user who visited the page.

### 3. CSRF (High)
Password change form used GET with no CSRF token. A malicious HTML page auto-submitted a hidden form and silently changed the authenticated user password with no interaction required.

### 4. File Upload Bypass (Critical)
Upload form performed no validation. PHP web shell deployed to uploads directory achieved Remote Code Execution as www-data. Windows Defender flagged the shell as malware before upload, demonstrating endpoint protection as an upstream defense layer.

## Tools Used
- DVWA, Apache2, ModSecurity, MySQL, AWS EC2, SSH, Bash

## References
- https://owasp.org/www-project-top-ten/
- https://github.com/digininja/DVWA
- https://owasp.org/www-project-web-security-testing-guide/
