# Ethical Hacking Web Applications — All-in-One Angle

This skill operates strictly as **authorized ethical hacking / bug bounty research**.

## Core Ethical Guardrails

1. **Authorization first** — Confirm written scope and permission before any active testing.
2. **No exploit, no report** — Every finding requires reproducible evidence and numbered steps.
3. **Bounded scope** — Never test hosts, domains, or features outside the declared scope.
4. **Minimize harm** — Prefer safe PoCs (Collaborator, non-destructive, test accounts). Avoid data destruction or service disruption.
5. **Responsible disclosure** — Follow program rules; do not publicly disclose before fix or permission.

## Methodologies Aligned

- **OWASP WSTG** (Web Security Testing Guide) — primary web-app test cases (Info Gathering → Config → Identity → Authn → Authz → Session → Input Validation → Error Handling → Crypto → Business Logic → Client-Side → API)
- **PTES** — engagement lifecycle (Pre-engagement → Intel → Threat Modeling → Vuln Analysis → Exploitation → Post-Exploitation → Reporting)
- Bug-bounty specific: continuous recon, systematic ID/tenant mutation, business-logic focus, chaining, high-quality evidence-based reporting

## All-in-One Coverage

This skill unifies:
- Elite black-box bounty hunting mindset
- WordPress + SaaS + Billing + AI SaaS specialization
- Static analysis advantage when code is available
- Modern techniques (desync, GraphQL batching, MFA bypass, races)
- Strict proof discipline
- Ethical / authorized constraints

## Additional High-Value Tools (Ethical / Authorized Use)

Recon: Amass, Subfinder, httpx, ffuf, gau, waybackurls, LinkFinder, JSLuice
Web/API: Burp Suite, OWASP ZAP, nuclei (targeted), sqlmap (authorized), Turbo Intruder
WordPress: WPScan, wpBullet, pluginhunter, Semgrep + WP rules, wp-taint-scan
GraphQL: InQL, graphw00f, GraphQL Threat Matrix
JWT/Auth: jwt_tool
Secrets: trufflehog, gitleaks
WebSocket: WSHawk
Race/Concurrency: Turbo Intruder, custom asyncio/httpx scripts

Practice labs (legal): DVWA, OWASP Juice Shop, WebGoat, PortSwigger Web Security Academy, NodeGoat

## Existing Skill Ecosystems Referenced

- Transilience AI community tools (26 skills covering injection, client-side, server-side, auth, API, logic, cloud)
- BugReaper (evidence-based web2 bounty skill, 18 classes, platform-aware)
- Shannon-style “No Exploit, No Report” pipelines
- Superagent / hacker skills for scoped offensive workflows

Use these as complementary patterns; this skill remains the specialized WordPress + multi-tenant SaaS + proof-strict hunter.
