# Lab 3 - Master Admin

## Objective

Identify the CORS misconfiguration in the Master Admin application.

---

## Credentials

- Username: `student`
- Password: `hacksmarter123`

---

## Process

I created an authenticated session for Lab 3:

```bash
curl -i -c cookies3.txt -d "username=student&password=hacksmarter123" http://10.1.170.194:5003/
Then I queried the dashboard and extracted the hidden endpoint:

curl -s -H "Cookie: session=VALUE" http://10.1.170.194:5003/dashboard | grep fetch

The dashboard used the same endpoint:

fetch('/api/data', { credentials: 'include' })

Then I tested the endpoint with a fake Origin header:

curl -i \
-H "Origin: http://evil.com" \
-H "Cookie: session=VALUE" \
http://10.1.170.194:5003/api/data
Evidence

The response included:

Access-Control-Allow-Origin: *

This means the server allows requests from any origin.

Interpretation

This is a wildcard CORS policy.

Even though credentials are not explicitly allowed, exposing sensitive data to any origin is still considered a misconfiguration.

Final Answer

Wildcard Origin

