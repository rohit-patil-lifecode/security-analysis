# GitHub Bug Bounty Ecosystem — Weaponised, Verified Tooling Catalogue

Living external knowledge bases and tooling for **authorised** hunting. Consult and apply — then prove every finding yourself under the skill's 200%-confidence rule. **Tool output is a lead, never a confirmed finding.**

## How to read this file (provenance)

- Every repo was **fetched live from GitHub and then adversarially re-checked** (status challenged in both directions) — **snapshot: 2026-07-30**.
- **Status badges are point-in-time.** Re-check a repo's last commit before you lean on it; the ecosystem moves.
- Where a tool is demoted, the **verified last-activity date is given as the justification** — not a bare label.
- Soft, drift-prone numbers (star counts, "N CVEs", exact patch versions) are deliberately omitted; the load-bearing facts are *language · alive-or-dead · the use · the command*.

**Legend:** ✅ active · 🟡 stable / feature-complete (low churn, still the standard — not abandoned) · 🟠 works but a better option exists · ⛔ dead/archived → use replacement · ★ high-ROI for this skill's paid classes.

**Contents:** [0 Pipeline](#0-the-pipeline-at-a-glance) · [1 Knowledge bases](#1--knowledge-bases--real-report-intelligence) · [2 Scope data](#2-scope--program-data) · [3 Recon](#3-recon--attack-surface-mapping) · [4 tomnomnom belt](#4-the-tomnomnom-belt-recon-to-lead-triage) · [5 Content/param discovery](#5-content--parameter-discovery) · [6 IDOR/authz](#6--access-control--idor--bola--the-1-paid-class) · [7 Business logic/race](#7--business-logic--race-conditions) · [8 JWT/ATO](#8--auth--jwt--account-takeover) · [9 GraphQL](#9--graphql-resolver-level-authz--schema-recovery) · [10 SSRF](#10--ssrf--cloud-metadata) · [11 XSS](#11-xss-favour-stored-in-admin-context) · [12 Smuggling/upload](#12-request-smuggling--file-upload--rce) · [13 Secrets/JS](#13--secrets--javascript-analysis) · [14 WordPress](#14--wordpress-arsenal-primary-specialism) · [15 Burp load-out](#15-suggested-burp-extension-load-out) · [16 Deprecated table](#16-deprecated--replaced--quick-reference) · [17 Usage rules](#how-this-skill-uses-these-resources)

---

## 0. The pipeline at a glance

```
SCOPE          bounty-targets-data · public-bugbounty-programs · disclose/platforms
  ↓
RECON          subfinder · amass · bbot · assetfinder → dnsx → httpx/httprobe → naabu
  ↓ (history)  gau · waymore · waybackurls → urless/uro (dedupe)
  ↓ (crawl)    katana (auth, headless) · JS: jsluice · xnLinkFinder
  ↓
DISCOVER       ffuf · feroxbuster · dirsearch (paths) · Arjun · x8 · ParamSpider (params)
  ↓ (triage)   gf patterns → qsreplace → nuclei (known-CVE oracle)
  ↓
EXPLOIT by class (§6-§14) → VERIFY 200% → REPORT
```
Glue: `anew` (dedupe/monitor) · `notify` (alerting) · `interactsh` (OOB) · `meg` (mass save-to-disk).

---

## 1. ★ Knowledge bases & real-report intelligence

Highest-signal first stop. Read *real disclosed reports* for the target's bug class before touching the target.

- **★ [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)** ✅ Markdown — canonical per-vuln payload/bypass library (70+ folders). **Use:** the `Insecure Direct Object References`, `GraphQL Injection`, `Business Logic Errors`, `Server Side Request Forgery` folders for ready ID-tampering, batch-query and cloud-metadata payloads. `git clone … && ls 'Insecure Direct Object References' 'Server Side Request Forgery'`
- **★ [HackTricks](https://github.com/HackTricks-wiki/hacktricks)** ✅ (live: book.hacktricks.xyz) — most complete methodology wiki. **Use:** `pentesting-web/` chapters (ssrf-to-metadata, oauth-to-account-takeover, jwt) as step-by-step escalation playbooks. Under the `HackTricks-wiki` org (old `carlospolop` repo redirects here).
- **★ [reddelexc/hackerone-reports](https://github.com/reddelexc/hackerone-reports)** ✅ — **the canonical, auto-updated index of top disclosed H1 reports**, sorted into per-bug-type files. **Use:** read `tops/idor.md`, `tops/business_logic.md`, `tops/ssrf.md` to mine real-world patterns (tenant-ID swaps, negative-quantity checkout, coupon-race) that map to this skill's top-paid classes. *Replacement for the low-signal aggregators below.*
- **[SecLists](https://github.com/danielmiessler/SecLists)** ✅ — the wordlist companion. **Use:** `Discovery/Web-Content/CMS/wordpress.fuzz.txt` + `raft-*-directories.txt` into ffuf to enumerate hidden WP plugins/AJAX actions/REST routes; `Pattern-Matching/` for leaked-secret regexes.
- **[KathanP19/HowToHunt](https://github.com/KathanP19/HowToHunt)** ✅ — step-by-step per-class hunting playbooks. **Use:** `RaceCondition/` and `IDOR/` folders as a checklist on a multi-tenant billing flow.
- **[six2dez/pentest-book](https://github.com/six2dez/pentest-book)** ✅ — actively-updated pentest wiki with a **Web Pentest Checklist**. **Use:** run it end-to-end on a SaaS app so no authz/logic surface goes untested.
- **[daffainfo/AllAboutBugBounty](https://github.com/daffainfo/AllAboutBugBounty)** ✅ — per-class notes **plus tech-specific pages** (WordPress, Laravel, Jira) and bypass guides (2FA, 403/429). **Use:** the WordPress page + 403-bypass tricks when the obvious path is blocked.
- **[vavkamil/awesome-bugbounty-tools](https://github.com/vavkamil/awesome-bugbounty-tools)** ✅ & **[Hack-with-Github/Awesome-Hacking](https://github.com/Hack-with-Github/Awesome-Hacking)** ✅ — tool menus / jump-tables, not techniques. Use to reach the right tool/sub-list; verify each linked tool's own maintenance.
- **[EdOverflow/bugbounty-cheatsheet](https://github.com/EdOverflow/bugbounty-cheatsheet)** 🟠 (last commit ~2023) — payload cheatsheet by class; solid but pair with PortSwigger/HackTricks for current bypasses.

**Low-signal aggregators — skip on quality grounds → use `reddelexc/hackerone-reports`:** `codebygk/hackerone-bug-bounty-reports` (dormant since 2024), `alexbieber/Bug_Bounty_writeups` (dormant since 2022), `tikam02/Bug-Bounty-Resources` (dormant since 2018), `AnLoMinus/Bug-Bounty` (still updated but re-hosts other people's lists) — link dumps of uneven/rotted external blogs with no original content.

---

## 2. Scope & program data (stay in-scope, hit fresh scope first)

- **★ [arkadiyt/bounty-targets-data](https://github.com/arkadiyt/bounty-targets-data)** ✅ Ruby — machine-readable in/out-of-scope dumps for H1/Bugcrowd/Intigriti/YesWeHack, **refreshed ~every 30 min**. **Use:** diff the git history to catch *newly added scope the day it lands* — brand-new domains have zero prior attention and still carry easy IDOR/logic bugs. `git diff HEAD~10 -- data/domains.txt data/wildcards.txt | grep '^+[^+]' | cut -c2- | httpx -silent`. Schema keys: `offers_bounties`, `targets.in_scope[].asset_identifier/asset_type` (no `.attributes` wrapper).
- **[projectdiscovery/public-bugbounty-programs](https://github.com/projectdiscovery/public-bugbounty-programs)** ✅ — source list behind the Chaos recon dataset. `jq -r '.programs[]|select(.bounty)|.domains[]' dist/data.json` → hydrate subdomains via `chaos -d <domain>`.
- **[disclose/bug-bounty-platforms](https://github.com/disclose/bug-bounty-platforms)** ✅ — platform catalogue grouped (ecosystem-specific, AI/LLM, Web3…). **Use:** the ecosystem-specific/AI tables to reach niche WordPress/e-commerce/AI-SaaS programs with under-tested scope.
- **⛔ [ARPSyndicate/bug-bounty-domains](https://github.com/ARPSyndicate/bug-bounty-domains)** — archived read-only (banner: Nov 13 2024) → use bounty-targets-data / Chaos. **🟠 [trickest/inventory](https://github.com/trickest/inventory)** — automation stopped (last commit Feb 2025, "Delete the data") → run your own subfinder+httpx.

---

## 3. Recon & attack-surface mapping

**ProjectDiscovery suite (all ✅ Go):**
- **[subfinder](https://github.com/projectdiscovery/subfinder)** — passive subdomain enum. **Use:** find forgotten tenant/staging/`wp-`/`admin-`/`api-` hosts with the weakest authz. `subfinder -d target.com -all -recursive -silent`. Add API keys in `provider-config.yaml`; pair with dnsx bruteforce (passive-only alone).
- **[dnsx](https://github.com/projectdiscovery/dnsx)** — bulk resolve + bruteforce + **CNAME→subdomain-takeover** flagging. `subfinder -d t -silent | dnsx -resp -cname -a -silent`.
- **[httpx](https://github.com/projectdiscovery/httpx)** — the triage funnel: status/title/tech-detect/TLS/CDN. `subfinder -d t -silent | httpx -td -title -sc -server -cdn -json` then grep `WordPress` to isolate the WP estate.
- **★ [katana](https://github.com/projectdiscovery/katana)** — crawler with **authenticated headless** mode + JS parsing. **Use:** crawl as a low-priv user to harvest every endpoint/param (incl. JS-built API routes) → the raw inventory you fuzz for IDOR/BOLA. `katana -u https://app.t -headless -jc -kf all -H 'Cookie: <low-priv>'`. Avoid `-aff` form-fill on prod billing/checkout.
- **[naabu](https://github.com/projectdiscovery/naabu)** — port scan → off-port admin/staging panels that skip the WAF. `naabu -list ips.txt -top-ports 1000 -exclude-cdn | httpx`. (SYN needs root; check scope.)
- **★ [interactsh](https://github.com/projectdiscovery/interactsh)** — OOB DNS/HTTP(S)/SMTP callback capture. **Use:** inject its `*.oast` host into every SSRF sink (webhooks, image/PDF import, XML-RPC pingback, WP oEmbed, avatar-from-URL) to confirm blind SSRF; **self-host** — public servers are WAF-blocklisted.
- **[uncover](https://github.com/projectdiscovery/uncover)** — one CLI over Shodan/Censys/FOFA/etc. `uncover -q 'html:"wp-content" ssl:"target.com"' -e shodan,fofa | httpx | nuclei -tags wordpress` finds shadow WP outside the subdomain tree. (Needs engine API keys.)
- **[notify](https://github.com/projectdiscovery/notify)** — pipe findings to Slack/Discord for first-to-report on freshly spun-up assets.

**Broader recon frameworks:**
- **[owasp-amass/amass](https://github.com/owasp-amass/amass)** ✅ Go — deep asset/DNS mapping. `amass enum -active -d target.com`. ⚠ heavy CLI churn across majors: `intel`/`viz`/`db` subcommands removed/relocated (`db`→`oam_subs`) — old scripts break; check current docs.
- **[bbot](https://github.com/blacklanternsecurity/bbot)** ✅ Python — recursive modular scanner with an event graph. **Use:** `bbot -t target.com -p subdomain-enum spider -m badsecrets` to turn recon straight into ATO/secret findings (badsecrets flags known Rails/Django/ASP.NET machine keys). ⚠ v3 broke v2 configs.
- **[reconftw](https://github.com/six2dez/reconftw)** ✅ — bash orchestrator wiring the whole pipeline. `./reconftw.sh -d target.com -r` (passive+recon). Use the Docker image to avoid dependency drift.

**Historical URLs (deprecated-but-live endpoints are IDOR gold):**
- **[gau](https://github.com/lc/gau)** ✅ — Wayback+CommonCrawl+OTX+URLScan URL harvest. `gau --subs target.com | grep -E '(id|user|account|order|invoice)=' | uro`.
- **★ [waymore](https://github.com/xnl-h4ck3r/waymore)** ✅ — thorough archive collector that also **downloads archived response bodies**. **Use:** `waymore -i target.com -mode B` then grep old HTML/JS for leaked keys, internal endpoints and dev comments no longer in the live app.
- **[urless](https://github.com/xnl-h4ck3r/urless)** ✅ — collapses a URL list to unique endpoints, deduping by GUID/int-ID pattern. `cat gau.txt | urless -iq` → the minimal deduped surface you hand-audit for IDOR (one shape per ID pattern).
- **[ParamSpider](https://github.com/devanshbatham/ParamSpider)** ✅ — mines archived parameterised URLs. `paramspider -d target.com --stream` → seed for IDOR/open-redirect/LFI.

**Crawlers (note maintenance):**
- **[hakrawler](https://github.com/hakluke/hakrawler)** 🟡 (feature-complete; last commit Dec 2024) — fast pipe-stage crawler, still fine. **⛔ [gospider](https://github.com/jaeles-project/gospider)** (last commit Mar 2024) → **katana** is the maintained replacement; keep gospider only for its S3/sitemap extraction.

---

## 4. The tomnomnom belt (recon-to-lead triage)

Small, single-purpose Unix-pipe tools. Low commit counts = **feature-complete, not abandoned** (verified: none archived/renamed; all still install and run). The classic triage chain:

```
waybackurls/gau → anew → gf <class> → qsreplace <payload> → kxss/Gxss/httpx
```

- **[gf](https://github.com/tomnomnom/gf)** 🟡 — named grep patterns by bug class. `cat allurls.txt | gf ssrf | qsreplace 'http://169.254.169.254/latest/meta-data/' | httpx`. ⚠ ships almost no patterns — clone **[1ndianl33t/Gf-Patterns](https://github.com/1ndianl33t/Gf-Patterns)** into `~/.gf` first.
- **[qsreplace](https://github.com/tomnomnom/qsreplace)** 🟡 — replace all query values with a payload + dedupe by shape. The injection stage after `gf`.
- **[unfurl](https://github.com/tomnomnom/unfurl)** 🟡 — slice URLs. `cat allurls.txt | unfurl keys | sort | uniq -c | sort -rn` ranks distinct param names → spot `account_id`/`tenant`/`invoice`/`is_admin` authz candidates.
- **[waybackurls](https://github.com/tomnomnom/waybackurls)** 🟡 — Wayback-only URL harvest (pair with gau for CommonCrawl/OTX).
- **[assetfinder](https://github.com/tomnomnom/assetfinder)** 🟠 — quick CT/Wayback subdomain source; several backends degraded → supplement with subfinder/amass, don't rely.
- **[meg](https://github.com/tomnomnom/meg)** 🟡 — fetch one path across all hosts, **save raw responses to disk** for offline grep. `meg -c 50 paths.txt live-hosts.txt ./out` then grep for `/wp-json/wp/v2/users`, `.env`, `wp-config.php.bak`, stack traces.
- **[anew](https://github.com/tomnomnom/anew)** 🟡 — append-if-new (dedupe tee). The glue for **continuous monitoring**: cron `assetfinder … | anew subs.txt | notify` for new-asset alerts.
- **[httprobe](https://github.com/tomnomnom/httprobe)** 🟠 — liveness gate → **httpx** is the current standard (adds title/status/tech); keep httprobe as a light fallback.

---

## 5. Content & parameter discovery

**Paths / forced browsing:**
- **★ [ffuf](https://github.com/ffuf/ffuf)** ✅ Go — fuzzer with rich matchers. **Use (IDOR):** fuzz the object ID in an authed REST route — `ffuf -w ids.txt -u 'https://t/wp-json/wc/v3/orders/FUZZ' -H 'Cookie: <lowpriv>' -mc 200 -fr 'cannot|forbidden'` → any 200 leaking another tenant's order is a payout.
- **[feroxbuster](https://github.com/epi052/feroxbuster)** ✅ Rust — auto-recursive forced browsing (faster than dirsearch, deeper recursion than ffuf). `feroxbuster -u https://t -w raft-large-directories.txt -x php,bak,zip --auto-tune`. ⚠ download only from GitHub — `feroxbuster.com` is NOT the project.
- **[dirsearch](https://github.com/maurosoria/dirsearch)** ✅ — path brute-forcer with extension tags/recursion. `dirsearch -u https://t -e php,bak,sql,zip,log -r` to surface `debug.log`, backup dumps, exposed plugin admin pages.

**Parameters (hidden params → IDOR / mass-assignment / price-tampering):**
- **★ [x8](https://github.com/sh1yo/x8)** ✅ Rust — hidden-param miner via response diffing, scales to thousands of URLs. **Use:** brute GET/POST/JSON params on REST endpoints to uncover `account_id`/`is_admin`/`role`. `x8 -u https://t/api/ep -w params.txt -X POST --body '{}'`.
- **[Arjun](https://github.com/s0md3v/Arjun)** ✅ — param discovery via chunked binary search. `arjun -u '…/admin-ajax.php?action=x' -m GET`. Hand results to x8/ffuf/dalfox.
- **[kiterunner](https://github.com/assetnote/kiterunner)** ✅ Go — context-aware API route discovery from OpenAPI (`.kite`), replaying correct method+params to hit routes generic dir-busters miss. **Use:** discover unlinked API routes to test for BOLA. `kr scan targets.txt -A=apiroutes-210228:20000`. (Tool is maintained; the bundled `apiroutes` wordlist is older — regenerate from fresh specs where possible.)

**Wordlists:**
- **[assetnote/wordlists](https://github.com/assetnote/wordlists)** ✅ — CDN-served, **regenerated monthly**; fresher hit-rate than static lists. `curl -s https://wordlists-cdn.assetnote.io/data/manifest.json | jq` → `technologies/wordpress.txt`, `params/parameters_top_1m.txt`.
- **[six2dez/OneListForAll](https://github.com/six2dez/OneListForAll)** ✅ — aggregated "rockyou for web fuzzing" in micro/short/long tiers. Start with `onelistforallshort.txt`.
- **[trickest/wordlists](https://github.com/trickest/wordlists)** ✅ — auto-generated CMS paths cloned from actual WP/Joomla/Magento repos → higher-fidelity CMS content discovery.
- **[Bo0oM/fuzz.txt](https://github.com/Bo0oM/fuzz.txt)** ✅ — compact sensitive-file list (backups/configs/leftovers). `ffuf -w fuzz.txt -u https://target.com/FUZZ`.

---

## 6. ★ Access control / IDOR / BOLA — the #1 paid class

Where this skill earns. Automate the swap; verify manually.

- **★ [Autorize](https://github.com/PortSwigger/autorize)** ✅ (Burp/Jython) — replays every request from a high-priv session using a low-priv/unauth session and **diffs responses to flag broken access control automatically**. **Use:** load a low-priv cookie, browse as admin, read the green/red "Authz Enforced" column on order/subscription/invoice/user endpoints. Needs the Jython jar configured.
- **★ [Auth Analyzer](https://github.com/simioni87/auth_analyzer)** ✅ (Burp) — like Autorize but with **auto-extraction/replacement of dynamic values** (CSRF/bearer/JSON fields) across sessions. **Use:** catches IDOR that Autorize misses when each request needs a fresh anti-CSRF token — define auto-extract rules, then confirm cross-tenant read/write.
- **★ Custom [nuclei](https://github.com/projectdiscovery/nuclei) templates** ✅ — community templates rarely find novel IDOR; **author a raw-HTTP template** that swaps a victim `tenant_id`/`order_id` and asserts another user's data leaks, making the bug reproducible and mass-testable. `nuclei -t ./my-idor.yaml -u https://app.t -H 'Authorization: Bearer <low-priv>'`.
- **ffuf ID-sweep** (§5) for numeric/UUID object references on REST routes.

---

## 7. ★ Business logic & race conditions

- **★ [Turbo Intruder](https://github.com/PortSwigger/turbo-intruder)** ✅ (Burp) — high-speed HTTP + Python scripting; ships the **single-packet-attack** race engine. **Use:** `resources/examples/race-single-packet-attack.py` to fire 20-50 concurrent redeem/checkout/withdraw requests in one packet → double-spend coupon, redeem gift card twice, bypass subscription/credit limit. Use HTTP/2 single-packet for the tightest window.
- **GraphQL batching** (§9): `CrackQL` packs thousands of OTP/credential guesses per request to bypass rate limits.

---

## 8. ★ Auth / JWT / account takeover

- **★ [jwt_tool](https://github.com/ticarpi/jwt_tool)** ✅ — the de-facto JWT toolkit: automates `alg=none`, RS256→HS256 key confusion, weak-key crack, `kid`/`jku`/`x5u` injection, fires tampered tokens live. **Use:** `jwt_tool -t https://api.t/me -rh 'Authorization: Bearer <JWT>' -M pb` then forge a token swapping tenant/user/role → full ATO. (Live-attack mode sends real requests — scope-check.)
- **[jwt-hack](https://github.com/hahwul/jwt-hack)** ✅ Rust — encode/decode, fast dictionary/brute secret cracking + auto vuln scan (alg-none, confusion). Complements jwt_tool for large secret cracks.

---

## 9. ★ GraphQL (resolver-level authz + schema recovery)

- **★ [InQL](https://github.com/doyensec/inql)** ✅ (Burp, v6+) — introspection-driven scanner that **auto-generates every query/mutation with filled args** into Repeater, plus a Batch tab and schema bruteforce when introspection is off. **Use:** replay generated mutations cross-account to hunt resolver-level BOLA/IDOR; use Batch for rate-limit/logic bypass.
- **★ [clairvoyance](https://github.com/nikitastupin/clairvoyance)** ✅ — **recovers the schema by field brute-force when introspection is disabled**, emitting JSON for Voyager/InQL. **Use:** reconstruct a SaaS schema to expose hidden `deleteUser`/`updateRole`/`refundOrder` mutations no one else can see. `clairvoyance -w wordlist.txt https://t/graphql -o schema.json`.
- **[graphql-cop](https://github.com/dolevf/graphql-cop)** ✅ — quick checks (batch/alias DoS, introspection, GraphiQL exposure, CSRF) with copy-paste curl PoCs. First-pass triage.
- **[graphw00f](https://github.com/dolevf/graphw00f)** ✅ — fingerprints the engine (Apollo/Hasura/Graphene…) → maps to the GraphQL Threat Matrix. Detecting Hasura → target role-header/permission bypass.
- **[graphql-voyager](https://github.com/graphql-kit/graphql-voyager)** ✅ — visualises a schema as a graph. **Use:** paste introspection JSON to spot which mutations touch other users' objects (`updateOrder(id)`, `node()` global-ID) → IDOR candidates.
- **[CrackQL](https://github.com/nicholasaleks/CrackQL)** ✅ — batches many alias ops/request from CSV for password-spray, OTP/2FA bypass, user-enum, IDOR sweeps. **⛔ [BatchQL](https://github.com/assetnote/batchql)** dormant (last commit ~2021) → CrackQL / InQL Batch replace it.

---

## 10. ★ SSRF → cloud metadata

- **[interactsh](https://github.com/projectdiscovery/interactsh)** (§3) — confirm blind SSRF via OOB callback.
- **[SSRFmap](https://github.com/swisskyrepo/SSRFmap)** ✅ — automated SSRF exploitation (20+ modules) from a Burp request file. **Use:** `ssrfmap -r request.txt -p url -m readfiles,portscan,aws,gce` → steal IAM creds from `169.254.169.254` (the SSRF-to-cloud-takeover chain that pays top dollar on AWS-hosted SaaS).
- **[Gopherus](https://github.com/tarunkant/Gopherus)** 🟠 (Py2, last commit ~2020) — generates `gopher://` payloads to pivot SSRF→RCE against internal Redis/MySQL/FastCGI. Pair with SSRFmap for HTTP/cloud modules.

---

## 11. XSS (favour stored, in admin context)

- **[dalfox](https://github.com/hahwul/dalfox)** ✅ Rust — reflected/stored/DOM XSS scanner with WAF bypass and **blind-XSS callbacks**. **Use:** `cat params.txt | dalfox pipe -b https://your.xss.ht` to catch stored XSS in comment/profile/support-ticket fields that only fire in an admin's browser — the highest-value XSS class. Feed it URLs from ffuf/gau/katana.
- **kxss / Gxss** — fast reflected-XSS reflection check after `qsreplace`.
- **[Hackvertor](https://github.com/hackvertor/hackvertor)** ✅ (Burp) — nested tag-based encoding to **bypass WAFs/filters** live in Repeater; auto-signs/encodes JWT/HMAC params for authz testing.
- **[Param Miner](https://github.com/PortSwigger/param-miner)** ✅ (Burp) — brute hidden params/headers; classic **web-cache-poisoning** discovery (unkeyed `X-Forwarded-Host`/`X-Original-URL` → cache-poison-to-stored-XSS on CDN-fronted SaaS).

---

## 12. Request smuggling & file upload → RCE

- **★ [HTTP Request Smuggler](https://github.com/PortSwigger/http-request-smuggler)** ✅ (Burp, v3) — James Kettle's desync detector: H2-downgrade, client-side desync, CL.0 parser-discrepancy, wired into Turbo Intruder. **Use:** "Smuggle probe" a CDN/reverse-proxy-fronted target (Cloudflare/Nginx/ALB) to find CL.TE/CL.0 that prefixes another user's request → credential theft / cache poisoning / admin access.
- **[UploadScanner](https://github.com/PortSwigger/upload-scanner)** ✅ (Burp Pro) — automates file-upload attacks (webshell, XXE, SSRF, XSS via polyglots/mismatched content-types). **Use:** hammer a WP media/plugin upload or SaaS avatar/import feature to land a webshell (RCE) or SVG/PDF-triggered XXE/SSRF. (Needs Burp Pro + Collaborator.)

---

## 13. ★ Secrets & JavaScript analysis

Modern JS is minified/webpacked — **AST-based tools beat regex**. The old regex trio is deprecated:

- **★ [BishopFox/jsluice](https://github.com/BishopFox/jsluice)** 🟡 (Go, tree-sitter AST; feature-complete, last commit 2024) — extracts URLs, paths, **query params (incl. concatenated, marked EXPR)** and secrets *with context*. **Use:** `cat app.js | jsluice urls` recovers API endpoints WITH their `tenant_id`/`order_id` params → direct BOLA/IDOR leads; `jsluice secrets` in the same pass. Still the best free AST JS extractor (replaces JSParser + SecretFinder).
- **★ [xnl-h4ck3r/xnLinkFinder](https://github.com/xnl-h4ck3r/xnLinkFinder)** ✅ — actively-maintained LinkFinder successor; extracts endpoints + params + a **target-specific param wordlist** + secrets across live domains, folders, Burp/ZAP/HAR. `xnLinkFinder -i target.com -sp https://target.com -op params.txt`.
- **★ [trufflehog](https://github.com/trufflesecurity/trufflehog)** ✅ — many detectors that **live-verify** credentials against real APIs. **Use:** `trufflehog filesystem ./js_dump --results=verified` or `trufflehog github --org=target --only-verified` → only report *working* Stripe/AWS/OpenAI keys, zero false-positive noise. **Report `--only-verified` only.**
- **[gitleaks](https://github.com/gitleaks/gitleaks)** ✅ — regex+entropy git-history scanner. **Use:** `gitleaks detect --source ./repo` on a target's open-source plugin/SDK repo to recover secrets committed-then-"deleted" in old commits but still valid.
- **[Mantra](https://github.com/MrEmpy/Mantra)** ✅ (Go) — fast JS API-key grep for recon chains: `cat js-urls.txt | mantra`. Pattern-match only → verify hits with trufflehog. (Canonical repo owner has moved between `MrEmpy`/`brosck` — confirm current source before install.)
- **[JS Miner](https://github.com/PortSwigger/js-miner)** ✅ (Burp) — passively mines JS/JSON for secrets, endpoints, dependency-confusion candidates, and **reconstructs source from `.map` sourcemaps** to read server-side logic.

**⛔ Deprecated JS/secret tools → replacements:** `nahamsec/JSParser` (Py2.7, last commit ~2019) → jsluice/xnLinkFinder · `GerbenJavado/LinkFinder` (regex-only, dormant) → xnLinkFinder · `m4ll0k/SecretFinder` (no verification, dormant) → trufflehog/jsluice.

---

## 14. ★ WordPress arsenal (primary specialism)

- **★ [wpscan](https://github.com/wpscanteam/wpscan)** ✅ Ruby — black-box scanner: fingerprints core/plugin/theme versions, enumerates users, cross-references the WPScan vuln DB. **Use:** `wpscan --url https://t/ -e ap,vp,u,cb,dbe --plugins-detection aggressive --api-token $TOKEN` → surface an outdated plugin with a known unauth privesc/object-injection/IDOR CVE. (Free tier limited req/day; aggressive detection is noisy.)
- **★ [Chocapikk/wpprobe](https://github.com/Chocapikk/wpprobe)** ✅ Go — fast plugin/theme fingerprinting via **stealthy REST-API enumeration**, mapping versions to bundled **Wordfence + WPScan data offline (no API key)**. `wpprobe scan -u https://t --mode stealthy`. The no-token alternative to wpscan's vuln DB.
- **★ [kazet/wpgarlic](https://github.com/kazet/wpgarlic)** 🟡 (research PoC, last commit 2024) — **dynamic plugin fuzzer** injecting a marker payload to surface arbitrary file read, XSS, SQLi, unauth option-update; credited with many plugin CVEs. **Use:** `python3 fuzz_plugin.py <slug>` → the highest-ROI path to an *original* WP 0-day. Noisy — manually verify every hit. (Not actively developed, but still works on current plugins.)
- **[nuclei-templates → WordPress](https://github.com/projectdiscovery/nuclei-templates)** ✅ — large WP template set. `nuclei -u https://t -tags wordpress -severity critical,high`. Known-CVE oracle; clone the nearest template as a scaffold for a custom check.
- **[WordPress-Coding-Standards](https://github.com/WordPress/WordPress-Coding-Standards)** ✅ (PHPCS/WPCS) — **when you have plugin source**, run the security sniffs to flag exact lines using `$_POST`/`$_GET` without sanitize/nonce or echoed without `esc_*`. `phpcs --standard=WordPress --sniffs=WordPress.Security.NonceVerification,WordPress.Security.EscapeOutput,WordPress.Security.ValidatedSanitizedInput /path/plugin`. ⚠ misses capability checks (`current_user_can`) and object injection → leads, not vulns.
- **[wordpress-develop](https://github.com/WordPress/wordpress-develop)** ✅ — core source as the **reference oracle**. `grep -rn "permission_callback" src/wp-includes/rest-api/` and read `map_meta_cap` in `capabilities.php` to prove a plugin's handler skips a check core would enforce. Pin to your target's release tag (src = trunk/unreleased).
- **[Ostorlab/KEV](https://github.com/Ostorlab/KEV)** ✅ — continuously-updated Known-Exploited-Vulnerabilities catalogue + CVE→nuclei-template map. **Use:** cross-reference fingerprinted WP plugins against KEV to prioritise *actively-exploited* CVEs and strengthen severity/triage.
- Intel DBs (not repos, query live): **Wordfence Intelligence**, **Patchstack DB** — see `intelligence-sources.md`.

---

## 15. Suggested Burp extension load-out

For a WordPress/SaaS/billing engagement (all §-referenced, all verified active): **Autorize** + **Auth Analyzer** (authz/IDOR) · **Turbo Intruder** (races) · **InQL** (GraphQL) · **HTTP Request Smuggler** + **Param Miner** (desync/cache) · **Hackvertor** (WAF bypass) · **[Logger++](https://github.com/nccgroup/BurpSuiteLoggerPlusPlus)** (global regex grep across all traffic for `authorization|api_key|_id` to retroactively catch leaked tokens + sequential IDs) · **JS Miner** + **UploadScanner** (Pro).

---

## 16. Deprecated / replaced — quick reference

| Repo | Status (verified 2026-07-30) | Use instead |
|---|---|---|
| `nahamsec/JSParser` | ⛔ Py2.7, dormant ~2019 | `BishopFox/jsluice`, `xnLinkFinder` |
| `GerbenJavado/LinkFinder` | 🟠 regex-only, dormant | `xnl-h4ck3r/xnLinkFinder` |
| `m4ll0k/SecretFinder` | 🟠 no verify, dormant | `trufflehog` (`--only-verified`), `jsluice` |
| `jaeles-project/gospider` | ⛔ last commit 2024 | `projectdiscovery/katana` |
| `assetnote/batchql` | ⛔ dormant ~2021 | `CrackQL`, InQL Batch tab |
| `ARPSyndicate/bug-bounty-domains` | ⛔ archived Nov 2024 | `arkadiyt/bounty-targets-data`, Chaos |
| `s0md3v/Corsy` | 🟠 dormant ~2021 | still usable; verify hits manually |
| `codebygk/…`, `alexbieber/…`, `tikam02/…` | ⛔ low-signal, dormant | `reddelexc/hackerone-reports` |
| `tomnomnom/httprobe`, `assetfinder` | 🟠 degraded / superseded | `httpx`, `subfinder`/`amass` |

*(Not deprecated — verified **active** despite common assumptions: `assetnote/kiterunner`, `PortSwigger/param-miner`, `Bo0oM/fuzz.txt`, `vavkamil/awesome-bugbounty-tools`. The tomnomnom belt is 🟡 feature-complete, not dead.)*

---

## How this skill uses these resources

1. **Read real disclosed reports first** (`reddelexc/hackerone-reports`) for the target's bug class — model the attack chain on what actually paid.
2. **Recon within authorised scope only.** Confirm scope from `bounty-targets-data`; never scan out-of-scope assets or off-port services that exceed program rules.
3. **Tools produce leads, never findings.** Scanner/fuzzer output (nuclei, wpscan, Autorize, ffuf) is a *candidate* — every Confirmed report still needs self-reproduced numbered steps, demonstrated impact, and a legitimate root-cause fix.
4. **Prefer non-destructive PoCs.** No `-aff` form-fill / race attacks on production billing/checkout without care; use test accounts; self-host OOB (interactsh) to avoid leaking client data to public infra.
5. **Re-verify status before relying.** Badges here are a 2026-07-30 snapshot; check a repo's last commit and confirm CVEs against Wordfence/Patchstack/NVD (`intelligence-sources.md`) before finalising severity.
