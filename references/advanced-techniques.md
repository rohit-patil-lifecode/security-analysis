# Advanced Techniques — Dense Hunter Reference

## HTTP Request Smuggling / Desync (Kettle lineage 2019–2026)

Modern vectors still alive:
- CL.0 (Content-Length: 0 + trailing body)
- Chunk-extension abuse (0;foo=bar)
- Obsolete line-folding
- Parser discrepancy (whitespace, case, HTTP/2 → 1.1 downgrade)
- Browser-powered desync
- Client-side desync

Proof of real desync requires observable impact:
- Smuggled prefix hits another user’s session
- Cache poisoning (especially CPDoS)
- WebSocket / SSE hijack
- Internal API or firewall bypass

Pure HTTP/1.1 pipelining is not smuggling. Use PortSwigger http-request-smuggler (v3+ parser-discrepancy mode) + Turbo Intruder.

Unkeyed headers (X-Forwarded-Host, X-Original-URL, X-Rewrite-URL, etc.) remain high-value for cache poisoning even when classic desync is harder on modern CDNs.

## SSRF Advanced

- URL parser differentials (Orange Tsai style) across languages and libraries
- Protocol smuggling, Unicode/IDNA tricks, DNS rebinding
- Redirect chains, DNS rebinding, alternate schemes
- Cloud metadata: 169.254.169.254 (IMDSv1 easy, IMDSv2 needs header), metadata.google.internal, Azure equivalent
- Internal port scanning via timing or error messages
- File:// and gopher:// where accepted

Always try to reach cloud metadata and observe credential or instance data.

## MFA / Auth Bypass (2025–2026 active techniques)

1. Session flag set after password, before MFA → direct access to /dashboard or API
2. MFA enforced on UI but missing on API endpoints
3. OTP/code returned in API response
4. Device-code flow hijacking (Microsoft 365, Google, Okta) — victim completes real MFA, attacker gets tokens
5. AiTM + MFA downgrade (force backup authenticator)
6. Push notification fatigue / exhaustion
7. SIM swap still works against SMS MFA

Test every endpoint reachable after password step and every API that should require MFA.

## GraphQL

- Introspection or field-suggestion leakage (“Did you mean…?”) when introspection disabled
- Alias batching: hundreds of operations in one HTTP request → rate-limit / OTP / brute bypass
- Nested query depth → DoS or expensive resolver chains
- Batching of mutations that lack per-operation authorization
- Mass assignment via extra input fields from schema
- Resolver-level IDOR

Run introspection or suggestion brute early, then target login/user/token/otp/admin resolvers with aliases.

## Business Logic & Race Conditions

Highest scanner-blind ROI.

Classic check-then-act:
- Balance check → debit
- Coupon “used?” check → mark used
- Seat / inventory limit check → increment
- Trial / one-per-account check → grant

Fire 10–50 parallel requests (Turbo Intruder or concurrent scripts). Observe if multiple succeed.

Other patterns:
- Negative quantity/price, zero total, type confusion (null, array, NaN)
- Workflow step skipping (confirm without payment)
- Coupon stacking that should be mutually exclusive
- Multi-account / self-referral
- Refund after partial fulfilment
- Credit / AI-credit / usage limit reset or race

## JWT / Token

- Algorithm confusion (RS256 → HS256 using public key as HMAC secret)
- alg=none
- jku / kid injection
- Editable claims (role, tenant_id, isAdmin, plan)
- Missing exp / nbf / aud / iss validation
- Long-lived refresh tokens without binding or rotation

## Chaining Playbook

Always ask “what does this enable next?”

Examples:
Info disclosure → IDOR → priv-esc → admin → RCE
Cross-tenant reset-token read → ATO
Stored XSS in admin context → session → plugin install
SSRF → cloud metadata → IAM → further pivot
Missing capability → role change → full admin
File delete wp-config → reinstall admin

Report the full chain severity.
