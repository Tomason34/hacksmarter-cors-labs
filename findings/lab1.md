# Lab 1 - Inventory Control

## Objective

Identify the CORS misconfiguration in the Inventory Control application.

---

## Credentials

- Username: `student`
- Password: `hacksmarter123`

---

## Process

First, I created an authenticated session with curl:

```bash
curl -i -c cookies.txt -d "username=student&password=hacksmarter123" http://10.1.170.194:5001/

The response returned a Set-Cookie header containing a valid session value.

Next, I used that session cookie to access the dashboard:

curl -i -H "Cookie: session=VALUE" http://10.1.170.194:5001/dashboard

The dashboard JavaScript revealed the hidden endpoint:

fetch('/api/data', { credentials: 'include' })

Then I requested the endpoint directly:

curl -i -H "Cookie: session=VALUE" http://10.1.170.194:5001/api/data

That returned confidential JSON data successfully.

Finally, I tested CORS behavior by sending a fake Origin header:

curl -i \
-H "Origin: http://evil.com" \
-H "Cookie: session=VALUE" \
http://10.1.170.194:5001/api/data
Evidence

The response included:

Access-Control-Allow-Origin: http://evil.com
Access-Control-Allow-Credentials: true

This means the application reflected the attacker-controlled Origin value and also allowed credentialed cross-origin requests.

Impact

A malicious website could potentially cause a logged-in victim's browser to make a request to the vulnerable application and read the confidential response.

Because Access-Control-Allow-Credentials: true was present, this configuration is especially dangerous.

Final Answer

Reflected Origin
