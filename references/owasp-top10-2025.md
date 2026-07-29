# OWASP Top 10:2025

Source of truth: https://owasp.org/Top10/2025/

## A01:2025 – Broken Access Control

Access control enforces policy so users cannot act outside intended permissions. Failures lead to unauthorized disclosure, modification, destruction of data, or performance of functions outside limits.

Common issues: IDOR / BOLA, missing function-level checks, privilege escalation, CORS misconfig, force browsing, JWT/cookie manipulation, path traversal, SSRF (now included).

Key CWEs: CWE-284, CWE-285, CWE-639, CWE-862, CWE-863, CWE-918, CWE-352, CWE-22, etc.

Prevention highlights:
- Enforce access control in trusted server-side / serverless code only.
- Deny by default except for public resources.
- Implement once and reuse; model ownership rather than trusting client-supplied IDs.
- Invalidate sessions/tokens properly; short-lived JWTs + refresh tokens.
- Log failures and rate-limit.
- Test with both horizontal and vertical privilege scenarios.

## A02:2025 – Security Misconfiguration

Insecure defaults, incomplete configs, open cloud storage, verbose errors, unnecessary features enabled, missing security headers, misconfigured CORS/CSP, default accounts, exposed admin interfaces.

Prevention: hardened baselines, automated configuration scanning, least privilege, remove unused features, security headers, environment segregation.

## A03:2025 – Software Supply Chain Failures

Broader than outdated components: compromised dependencies, build systems, distribution infrastructure, CI/CD pipeline poisoning, malicious packages, integrity failures across the ecosystem.

Prevention: SBOM, dependency pinning + verification, signed artifacts, isolated build environments, continuous monitoring of advisories (OSV, GitHub Advisories, NVD), SCA tools.

## A04:2025 – Cryptographic Failures

Failures related to cryptography that often lead to sensitive data exposure or system compromise. Weak algorithms, improper key management, missing encryption, poor random number generation, certificate validation failures.

Prevention: use modern libraries and algorithms (AES-GCM, ChaCha20-Poly1305, TLS 1.3+), proper key storage (HSM/KMS), never roll own crypto, encrypt data at rest and in transit.

## A05:2025 – Injection

SQL, NoSQL, OS command, LDAP, XPath, template (SSTI), XSS (now under Injection), expression language, etc. Hostile data used directly in commands or queries.

Prevention: parameterized queries / prepared statements, safe APIs, input validation + output encoding contextually, least privilege DB accounts, allow-lists.

## A06:2025 – Insecure Design

Missing or ineffective security controls from the design phase. Threat modeling failures, lack of secure design patterns, missing rate limiting or business logic protections.

Prevention: threat modeling, secure design patterns, reference architectures, “secure by design” reviews early in SDLC.

## A07:2025 – Authentication Failures

Weak credential storage, credential stuffing, session fixation, missing MFA, weak password recovery, JWT algorithm confusion, token leakage.

Prevention: strong password policies + MFA, secure session management, protect against automated attacks, proper token handling (short-lived, secure storage, rotation).

## A08:2025 – Software or Data Integrity Failures

Code and infrastructure that does not protect against integrity violations. Insecure CI/CD, auto-update without verification, deserialization of untrusted data, unsigned software.

Prevention: digital signatures, integrity checks, secure deserialization libraries or avoid deserialization of untrusted input, protect CI/CD secrets and pipelines.

## A09:2025 – Security Logging and Alerting Failures

Insufficient logging, missing alerts for critical events, logs that can be tampered with or are inaccessible during incidents.

Prevention: log authentication, access control, and input validation failures; protect log integrity; implement real-time alerting; retain logs sufficiently.

## A10:2025 – Mishandling of Exceptional Conditions

Failing open, improper error handling, logical errors under abnormal conditions, resource exhaustion not handled gracefully.

Prevention: fail closed, consistent error handling, graceful degradation, proper exception management without leaking sensitive info.
