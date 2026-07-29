# OWASP API Security Top 10:2023

Source: https://owasp.org/API-Security/editions/2023/en/0x11-t10/

## API1:2023 – Broken Object Level Authorization (BOLA / IDOR)

APIs expose endpoints that handle object identifiers. Failure to check whether the authenticated user is authorized for the specific object.

Tests:
- Change object IDs (numeric, UUID, sequential) belonging to other users.
- Replay requests with different user tokens.
- Test create/read/update/delete for horizontal and vertical access.
- Check nested objects and related resources.

Prevention: enforce authorization checks on every function that accesses a data source using a user-supplied ID; use random non-guessable IDs where practical; record ownership.

## API2:2023 – Broken Authentication

Incorrect implementation of authentication mechanisms allowing token compromise or identity assumption.

Common issues: weak JWT validation (alg=none, key confusion), long-lived tokens, missing rate limiting on login, credential stuffing, insecure password recovery, token leakage in URLs/logs.

Prevention: short-lived access tokens + refresh tokens, strong token validation, MFA, progressive lockout, secure recovery flows, never expose tokens in URLs.

## API3:2023 – Broken Object Property Level Authorization

Combines excessive data exposure and mass assignment. Lack of authorization at the property level leads to reading or writing sensitive fields (isAdmin, balance, role, etc.).

Tests: examine responses for sensitive fields not needed by the client; attempt to set privileged properties via mass assignment (JSON body, form fields).

Prevention: respond only with needed properties (DTOs / explicit serialization); use allow-lists for writable properties; enforce property-level authorization.

## API4:2023 – Unrestricted Resource Consumption

Lack of limits on resource usage (CPU, memory, bandwidth, storage, third-party costs such as SMS/email).

Tests: large payloads, high pagination limits, expensive queries, recursive GraphQL queries, repeated expensive operations.

Prevention: rate limiting, quotas, payload size limits, query complexity analysis (GraphQL), timeouts, cost-based limits.

## API5:2023 – Broken Function Level Authorization

Complex policies and unclear separation between admin and regular functions allow privilege escalation to administrative endpoints.

Tests: access admin-only endpoints with regular user tokens; change HTTP methods; probe for hidden admin routes.

Prevention: deny by default; explicit role checks on every sensitive function; separate admin APIs where possible.

## API6:2023 – Unrestricted Access to Sensitive Business Flows

Business flows (purchase, voting, comment, account creation) that can be abused at scale without compensating controls.

Tests: automate the flow; look for missing CAPTCHA, rate limits, device fingerprinting, or business-rule enforcement under load.

Prevention: identify sensitive flows and add rate limiting, CAPTCHA, anomaly detection, or step-up authentication.

## API7:2023 – Server Side Request Forgery (SSRF)

API fetches a remote resource using a user-supplied URI without proper validation.

Tests: internal IPs (127.0.0.1, 169.254.169.254, metadata endpoints), cloud metadata services, file://, gopher://, redirects, DNS rebinding.

Prevention: allow-list of permitted domains/IPs; block private/link-local ranges; use network-level controls; avoid user-controlled URLs when possible.

## API8:2023 – Security Misconfiguration

Missing security hardening across the API stack: unnecessary HTTP methods, verbose errors, missing security headers, misconfigured CORS, default credentials, exposed debug endpoints, improper TLS.

Prevention: secure configuration baselines, automated scanning, least privilege, remove unused features, proper CORS (never * with credentials).

## API9:2023 – Improper Inventory Management

Lack of visibility into all API hosts, versions, and endpoints (including deprecated, shadow, and debug APIs).

Prevention: maintain accurate inventory; retire old versions; document all endpoints; discover via traffic analysis and code review.

## API10:2023 – Unsafe Consumption of APIs

Trusting data from third-party APIs more than user input, leading to weaker validation and subsequent compromise via the integration.

Prevention: treat third-party data as untrusted; validate and sanitize; apply the same controls as for user input; monitor third-party health and integrity.
