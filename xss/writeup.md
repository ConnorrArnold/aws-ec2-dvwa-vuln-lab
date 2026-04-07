# Cross-Site Scripting (XSS) Write-Up

## Overview

| Field | Details |
|-------|---------|
| Vulnerability | XSS Reflected and Stored |
| Severity | High |
| CVSS Score | 7.4 |
| Target | DVWA on AWS EC2 |
| Security Level | Low |

## What Is XSS?

XSS occurs when an attacker injects malicious scripts into pages viewed by other users. The browser executes the script in the context of the vulnerable site.

## Reflected XSS

Location: DVWA > XSS Reflected

Payload 1 - Basic injection:
script alert XSS /script
Result: JavaScript alert fires confirming input reflected without sanitization.

Payload 2 - Cookie theft:
script alert document.cookie /script
Result: Session cookie (PHPSESSID) displayed. An attacker can exfiltrate this to hijack the session.

## Stored XSS

Location: DVWA > XSS Stored

Payload submitted in Name or Message field:
script alert Stored XSS /script
Result: Payload saved to database. Fires automatically for every user who visits the page without any further attacker interaction.

## Reflected vs Stored

| Type | Persistence | Victims | Severity |
|------|------------|---------|----------|
| Reflected | Per request only | One at a time | High |
| Stored | Permanent in DB | All users | Critical |

## Impact
- Session hijacking via cookie theft
- Account takeover without credentials
- Page defacement
- Redirecting users to malicious sites

## Mitigations
- Encode all user output with htmlspecialchars()
- Implement Content Security Policy headers
- Set HttpOnly flag on session cookies
- Set SameSite flag on cookies
- Validate and sanitize all input

## References
- https://owasp.org/www-community/attacks/xss/
- https://cwe.mitre.org/data/definitions/79.html
