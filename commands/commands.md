# Commands Used (with Explanations)

This file documents all commands used during the lab in a simple, study-friendly format.

---

## 1. Check network interfaces

```bash
ip -br a
What it does

Shows a short summary of network interfaces.

Why I used it

To confirm the VPN interface (tun0) is active.

2. Test connectivity
ping -c 2 10.1.170.194
What it does

Sends 2 packets to the target.

Why I used it

To confirm the target is reachable through VPN.

3. Nmap scan
nmap -p- 10.1.170.194
What it does

Scans all ports on the target.

Why I used it

To find all open ports.

4. Login with curl
curl -i -c cookies.txt -d "username=student&password=hacksmarter123" http://10.1.170.194:5001/
What it does

Sends login request and saves session cookie.

Key flags
-i → show headers
-c → save cookies
-d → send POST data
5. View cookies
cat cookies.txt
What it does

Displays saved session cookie.

6. Access dashboard with session
curl -i -H "Cookie: session=VALUE" http://10.1.170.194:5001/dashboard
What it does

Sends authenticated request.

7. Find hidden endpoint
curl -s -H "Cookie: session=VALUE" http://10.1.170.194:5001/dashboard | grep fetch
What it does

Searches for JavaScript API calls.

8. Call API directly
curl -i -H "Cookie: session=VALUE" http://10.1.170.194:5001/api/data
What it does

Requests sensitive data endpoint.

9. Test CORS
curl -i \
-H "Origin: http://evil.com" \
-H "Cookie: session=VALUE" \
http://10.1.170.194:5001/api/data
What it does

Simulates a malicious cross-origin request.

What I check
Access-Control-Allow-Origin
Access-Control-Allow-Credentials

10. Useful HTTP status codes
Code	Meaning
200	Success
302	Redirect (not logged in)
404	Not found
403	Forbidden
Summary

Key skills practiced:

using curl for web testing
handling cookies manually
reading JavaScript to find endpoints
testing and identifying CORS misconfigurations

