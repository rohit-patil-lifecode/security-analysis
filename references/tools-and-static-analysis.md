# Tools, Libraries & Static Analysis Advantage

Since we often have (or can obtain) source code, we have a massive advantage over pure black-box hunters. Use it aggressively.

## How Elite Hunters Find Bugs Without Code (and how we do better)

Black-box hunters rely on:
1. Exhaustive recon (subdomains, JS analysis, archives, parameter discovery)
2. Traffic interception + systematic ID/tenant swapping
3. Business-logic understanding and race testing
4. Known CVE matching after fingerprinting
5. Creative chaining of low signals

With code we add:
- Direct search for dangerous patterns (current_user_can missing, unserialize, permission_callback = __return_true)
- Taint-style reasoning from user input to sinks
- Exact capability and nonce analysis
- Understanding of intended vs actual authorization
- Fast identification of high-risk plugins/functions

## WordPress / PHP Static Analysis Tools & Libraries

**High-value for researchers:**
- **wpBullet** (Python) — static analysis specifically for WP plugins/themes
- **WP-Hunter / Temodar Agent** — recon + SAST + Semgrep for WP plugins, prioritizes abandoned/popular
- **pluginhunter** (PyPI) — AST-based, taint analysis, 48 WP-specific rules
- **wp-taint-scan** (Go) — WordPress-aware taint engine (understands nonce ≠ authz, capability tiers, REST/AJAX entrypoints)
- **Semgrep** with PHP + custom WP rules (most scalable for bulk plugin hunting)
- **plecost**, **wpat** — fingerprinting + known vuln matching
- Manual: grep/ripgrep for `wp_ajax_`, `register_rest_route`, `unserialize`, `current_user_can`, `permission_callback`, `$_REQUEST`, `eval(`, `assert(`, `create_function`

**Recommended workflow when code is available:**
1. Fingerprint versions → check Wordfence/Patchstack
2. Ripgrep for high-risk patterns
3. Run Semgrep / wp-taint-scan / pluginhunter
4. Manually verify every missing capability or unserialize sink
5. Build PoC from the exact vulnerable path

## General / API / Node.js Tools

- **bbot** — powerful recon automation (Python)
- LinkFinder / JSLuice / relative-url-extractor — pull endpoints from JS
- GraphQL: InQL, graphql-cop, graphw00f, GraphQL Threat Matrix
- JWT: jwt_tool, custom scripts for alg confusion / claim injection
- Race: Turbo Intruder, requests-racer, custom concurrent Python
- HTTP Smuggling: smuggler, PortSwigger http-request-smuggler
- Secrets: trufflehog, gitleaks, custom regex for .env / keys
- API: custom Node/Python scripts for mass IDOR/tenant swapping

## Python Libraries Useful for Custom Hunter Scripts

- requests / httpx / aiohttp — concurrent testing
- beautifulsoup4 / selectolax — HTML parsing
- pyjwt / python-jose — JWT manipulation
- concurrent.futures or asyncio — race conditions
- semgrep (as library) or subprocess calls
- python-wordpress-xmlrpc (for XML-RPC testing)
- dnspython, censys, shodan (recon)

## Node.js Ecosystem Notes

- Prototype pollution, JWT libraries (jsonwebtoken historical issues), npm supply-chain
- Express/Fastify/NestJS route and middleware analysis
- GraphQL Yoga / Apollo specific misconfigs (see GraphQL Threat Matrix)

## Practical Leverage Rules

When code is present:
- Never rely only on dynamic testing — read the authorization logic
- Search first for the absence of checks, not the presence of vulns
- Map every user-controlled input to every sink
- Treat “nonce only” as almost always vulnerable in WP
- For SaaS, find every place tenant_id is read from the request and verify server-side enforcement

When code is absent:
- Fall back to classic elite recon + systematic parameter/tenant/ID mutation + business logic races
- Use JS analysis heavily to discover hidden endpoints and secrets
