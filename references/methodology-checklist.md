# Elite Hunter Methodology Checklist

## Phase 0 — Scope
- [ ] Authorised targets and rules confirmed
- [ ] Out-of-scope explicitly noted

## Phase 1 — Recon & Fingerprint
- [ ] Subdomains, IPs, ports, tech stack
- [ ] WordPress: core + every plugin/theme version → Wordfence/Patchstack/WPScan
- [ ] SaaS: tenant identifiers mapped (path/header/claim/body)
- [ ] JS source maps, feature flags, hidden routes, API keys
- [ ] GraphQL introspection or field suggestions
- [ ] XML-RPC methods, REST namespaces, admin-ajax actions
- [ ] Cloud metadata endpoints reachable?
- [ ] Backup/debug/.git/.env exposures

## Phase 2 — Auth Mapping
- [ ] Registration, login, password reset, magic link, invite flows
- [ ] MFA enforcement on UI vs API
- [ ] Session / JWT / refresh token behaviour
- [ ] OAuth/OIDC/SAML redirect and token handling

## Phase 3 — High-ROI Testing
- [ ] WordPress capability/nonce gaps on every AJAX/REST
- [ ] Cross-tenant IDOR/BOLA on every object type
- [ ] Business logic & races (coupons, credits, seats, stock, trials)
- [ ] Billing webhooks, price manipulation, subscription races
- [ ] File upload polyglots and path traversal
- [ ] GraphQL alias batching on auth endpoints
- [ ] SSRF to metadata and internal
- [ ] Object injection / unserialize sinks

## Phase 4 — Broader Coverage
- [ ] Classic injection (SQLi, XSS, SSTI, command)
- [ ] Client-side (DOM XSS, prototype pollution, postMessage)
- [ ] Smuggling / cache poisoning
- [ ] Secrets in responses, JS, configs
- [ ] Privilege escalation paths
- [ ] Hidden admin / debug / staging

## Phase 5 — Chaining & Impact
- [ ] Can any finding feed another?
- [ ] Full attack path written
- [ ] Concrete impact demonstrated (data, priv, money, RCE)
- [ ] Confidence labelled (Confirmed / High Confidence / Hypothesis)

## Phase 6 — Report
- [ ] Payout-oriented title and structure
- [ ] Exact evidence and numbered steps
- [ ] CVSS + CWE + OWASP
- [ ] Business impact clear
