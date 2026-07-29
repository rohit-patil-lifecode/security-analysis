# W3Schools Cyber Security Foundations (Relevant to Bounty Hunting)

Source: https://www.w3schools.com/cybersecurity/ (educational primer)

## Web Application Fundamentals (must know)

- HTTP verbs: GET, POST, PUT, DELETE, PATCH
- Status codes: 200, 301/302, 400, 403, 404, 500
- Headers: Host, User-Agent, Cookie, Referer, Set-Cookie, Content-Type
- Sessions via cookies (PHPSESSID etc.) or tokens (JWT)
- URL structure, query parameters, URL encoding
- REST style paths
- Virtual hosts (Host header routing)
- TLS for transport security

## Core Web Application Attacks Covered

### IDOR (Insecure Direct Object Reference)
- Missing authorization check when accessing objects by ID
- Classic test: change numeric ID and observe unauthorized data
- Root cause fix: server-side ownership/authorization check before returning the object
- Avoid sequential “magic numbers”; prefer high-entropy IDs (UUID) + still enforce authz

### SQL Injection
- User input concatenated into SQL
- Classic payload: `' OR '1'='1`
- Root cause fix: parameterized queries / prepared statements only

### XSS (Cross-Site Scripting)
- Reflected: payload in URL/request reflected in response
- Stored: payload persisted and served to other users
- Impact: session/cookie theft, defacement, phishing
- Root cause fix: context-aware output encoding + Content-Security-Policy

## Recon Basics (Mapping & Port Scanning)

- Host discovery (ping, multi-protocol probes, ARP on LAN)
- Port scanning (TCP SYN, UDP, version detection `-sV`, scripts `-sC`, OS detection)
- Nmap timing templates (T0–T5) and aggressive mode `-A`
- Purpose: map attack surface before deeper testing
- Always authorised only

## Penetration Testing Mindset (from W3Schools)

- Black-box / Grey-box / White-box
- Proactive identification of vulnerabilities before real attackers
- Includes web apps, networks, wireless, mobile, social engineering
- Human factor remains critical (phishing, vishing, physical)

## How This Skill Uses the Foundations

These basics are assumed knowledge. The skill focuses on advanced, high-ROI bounty hunting (WordPress, multi-tenant SaaS, business logic, chaining, proof-strict reporting) built on top of this foundation. Always apply the 200% confidence + root-cause fix rules even to classic issues like IDOR/SQLi/XSS.
