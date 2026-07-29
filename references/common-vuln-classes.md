# Common Vulnerability Classes — Hunter Decision Trees

## Broken Access Control / IDOR / BOLA / BFLA

Decision tree:
1. Is there an object identifier (ID, UUID, filename, email, slug)?
2. Can I change it while keeping my session?
3. Does the server re-check ownership or only trust the session + ID?
4. Horizontal (same privilege) or vertical (privilege escalation)?
5. Cross-tenant version of the same question?

Always test both GET and state-changing methods (PUT/PATCH/DELETE/POST).

## Authentication

- Password reset: token predictability, host-header injection, user enumeration, token not bound to user
- Magic link / one-time login: replay, not single-use, long expiry
- MFA: see advanced-techniques.md
- JWT / session: see advanced-techniques.md
- OAuth/OIDC: redirect_uri manipulation, state missing, token leakage in referrer or logs

## Injection

- SQLi / NoSQLi: classic, boolean, time, OOB, second-order
- Command: separators, blind via time/DNS
- SSTI: polyglots to identify engine then RCE
- XSS: context (HTML, attr, JS, URL), stored vs reflected vs DOM, mutation XSS
- XXE: classic, blind, parameter entities
- Template / expression injection in server-side renders

## SSRF

See advanced-techniques.md. Always include cloud metadata targets.

## File Upload

1. Extension blacklist vs whitelist
2. Content-Type vs magic bytes
3. Filename path traversal
4. Polyglot / SVG / Office / archive
5. Does the file get executed, included, or parsed by a vulnerable library?
6. Zip Slip on extractors

## Business Logic

See advanced-techniques.md and woocommerce-billing-ai.md.
Primary questions: Can I make money/credits go negative? Can I skip payment? Can I replay? Can I race the check?

## Deserialization

- PHP: unserialize / PHAR
- Java / .NET / Python pickle / Node / Ruby
- Look for gadget chains already present in dependencies

## Cache & Smuggling

See advanced-techniques.md (Kettle).

## Secrets & Info Disclosure

- .env, .git, backup files, phpinfo, debug endpoints, source maps
- Stack traces, verbose errors, directory listing
- JWT secrets, API keys in JS, mobile apps, or responses
