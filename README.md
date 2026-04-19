# HackSmarter CORS Labs

## Overview

This repository documents my walkthrough of three HackSmarter CORS Misconfiguration labs.

The goal of this project was to:
- connect to the lab environment through VPN
- enumerate exposed services
- identify the hidden API endpoint used by each application
- manually test CORS behavior with `curl`
- classify each lab based on its CORS configuration

This project helped me practice:
- VPN-based lab access
- Nmap enumeration
- session handling with `curl`
- reading JavaScript to discover API endpoints
- identifying reflected origin and wildcard origin misconfigurations

---

## Lab Environment

After connecting to the VPN, I verified connectivity and enumerated the target.

### Reachability check

```bash
ping -c 2 10.1.170.194

Nmap scan workflow

I used a two-stage scan:

full TCP scan with -p-
service/version detection on only the open ports

The scan identified:

22/tcp SSH
80/tcp HSM Hub
5001/tcp Lab 1
5002/tcp Lab 2
5003/tcp Lab 3

All three labs were served by:

Werkzeug/3.1.5
Python/3.12.3
Credentials Used

These were the lab-provided credentials used during testing:

Username: student
Password: hacksmarter123
Methodology

My workflow for each lab was:

Connect to the VPN
Confirm tunnel interface with ip -br a
Verify target reachability with ping
Enumerate services with Nmap
Log in with known lab credentials
Use curl to create an authenticated session cookie
Request /dashboard with the session cookie
Inspect the dashboard HTML to identify the hidden endpoint
Send requests to /api/data
Add a fake Origin header to test CORS behavior
Classify the misconfiguration based on the response headers
Results
Lab	Application	Endpoint	Result
Lab 1	Inventory Control	/api/data	Reflected Origin
Lab 2	Blueprint Safe	/api/data	No CORS Misconfiguration
Lab 3	Master Admin	/api/data	Wildcard Origin
What I Learned
1. HTTP status codes give immediate clues

Some of the most useful codes during this lab were:

200 OK → request succeeded
302 Found → redirected, usually due to missing authentication
404 Not Found → wrong endpoint
403 Forbidden → endpoint exists, but access is denied
2. Session cookies matter

The browser may be logged in while curl is not.
To reproduce authenticated requests in terminal, I had to save the session cookie and manually include it in later requests.

3. Hidden endpoints can be discovered from JavaScript

The visible button did not expose the endpoint directly in the UI.
By reviewing the dashboard source, I found:

fetch('/api/data', { credentials: 'include' })

That revealed the real endpoint used in all three labs.

4. CORS classification depends on response headers

The most important headers were:

Access-Control-Allow-Origin
Access-Control-Allow-Credentials

These headers determined whether the application was secure, reflected attacker-controlled origins, or allowed wildcard access.

Repository Structure
hacksmarter-cors-labs/
├── README.md
├── commands/
│   └── commands.md
├── findings/
│   ├── lab1.md
│   ├── lab2.md
│   └── lab3.md
└── evidence/
Final Answers
Inventory Control → Reflected Origin
Blueprint Safe → No CORS Misconfiguration
Master Admin → Wildcard Origin
Notes

This lab was a good reminder that understanding the request flow is more important than relying only on visible UI behavior.

The most useful skills I practiced here were:

building authenticated curl requests
handling cookies manually
extracting API endpoints from JavaScript
recognizing the difference between reflected origin, wildcard origin, and secure CORS behavior

