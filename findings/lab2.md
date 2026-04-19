# Lab 2 - Blueprint Safe

## Objective

Identify the CORS behavior of the Blueprint Safe application.

---

## Credentials

- Username: `student`
- Password: `hacksmarter123`

---

## Process

I created a new authenticated session for Lab 2:

```bash
curl -i -c cookies2.txt -d "username=student&password=hacksmarter123" http://10.1.170.194:5002/
Then I accessed the dashboard with the session cookie and extracted the hidden endpoint:

curl -s -H "Cookie: session=VALUE" http://10.1.170.194:5002/dashboard | grep fetch

The dashboard again used:

fetch('/api/data', { credentials: 'include' })

Then I tested the endpoint directly with an attacker-controlled Origin:

curl -i \
-H "Origin: http://evil.com" \
-H "Cookie: session=VALUE" \
http://10.1.170.194:5002/api/data
Evidence

The server returned 200 OK and the JSON data, but the response did not include:

Access-Control-Allow-Origin
Access-Control-Allow-Credentials
Interpretation

Even though curl received the response, a browser would not allow JavaScript on another origin to read it because the required CORS headers were missing.

This means the application is not vulnerable to the same cross-origin data exposure as Lab 1.

Final Answer

No CORS Misconfiguration

