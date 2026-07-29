# PayloadsAllTheThings Integration

Primary external payload & technique reference:  
https://github.com/swisskyrepo/PayloadsAllTheThings  
(Also browsable at https://swisskyrepo.github.io/PayloadsAllTheThings/)

This is the standard community cheatsheet for web application payloads, bypasses, and exploitation techniques. Use it as the go-to source when crafting test payloads or looking for known bypass patterns.

## Most Relevant Sections for This Skill (Priority Order)

### Authorization / Access Control (Highest ROI)
- Insecure Direct Object References
- Mass Assignment
- Account Takeover
- OAuth Misconfiguration
- Business Logic Errors
- Race Condition

### Injection & Server-Side
- SQL Injection
- NoSQL Injection
- Command Injection
- Server Side Request Forgery
- Server Side Template Injection
- XXE Injection
- GraphQL Injection
- Insecure Deserialization
- File Inclusion / Directory Traversal
- Upload Insecure Files (Zip Slip, polyglots, etc.)

### Client-Side & Request
- XSS Injection
- Cross-Site Request Forgery
- CORS Misconfiguration
- Request Smuggling
- HTTP Parameter Pollution
- Open Redirect
- Prototype Pollution
- CRLF Injection

### Auth / Token
- JSON Web Token
- SAML Injection
- Type Juggling

### Other High-Value
- Prompt Injection (AI SaaS)
- Web Cache Deception
- Web Sockets
- Hidden Parameters
- Dependency Confusion
- API Key Leaks

## How to Use Inside This Skill

1. When testing a specific class (IDOR, SSRF, XSS, upload, JWT, GraphQL, race, etc.), consult the matching folder for current payloads and bypasses.
2. Prefer the methodology and examples in the README of each folder over random payload lists.
3. Always adapt payloads to the target technology and still apply the 200% confidence + exact reproduction rules before reporting.
4. PayloadsAllTheThings is a technique library — it does not replace proof, impact demonstration, or root-cause analysis.

## Methodology & Resources Folder

Also contains useful cheatsheets for broader pentest methodology, cloud, privilege escalation, etc. Use selectively when the engagement expands beyond pure web/SaaS/WordPress scope.
