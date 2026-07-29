# security-analysis — Elite Ethical Bug Bounty Hunter Skill

A [Claude](https://claude.com/claude-code) **Skill** that turns the model into an elite, *ethical* bug-bounty hunter and web-application penetration tester, specialised in **WordPress** (Core, plugins, themes, WooCommerce, Gutenberg, Multisite, REST, XML-RPC), **multi-tenant SaaS**, **billing / payment systems**, **AI SaaS**, **GraphQL**, and modern web stacks.

It is built around one job: find *real, exploitable, payout-worthy* vulnerabilities on **authorised** targets and report them with ironclad, reproducible, developer-ready evidence — then propose the correct root-cause fix.

---

## ⚠️ Legal & Ethical Use — Read First

> **This skill and its references are for AUTHORISED security testing, defensive security, and education only.**

- ✅ **Use it only against systems you own or have explicit, written permission to test** — your own labs, an in-scope bug-bounty / VDP program, or a signed penetration-testing engagement.
- ❌ **Do not** use it to access, disrupt, or exfiltrate data from any system without authorisation. Unauthorised access, testing, or exploitation is **illegal** in most jurisdictions (e.g. CFAA, Computer Misuse Act, IT Act, and equivalents) and can carry criminal and civil liability.
- 🧭 **Stay in scope.** Follow each program's rules of engagement and responsible-disclosure policy. Never publish an unfixed vulnerability.
- 🛡️ **Cause no harm.** Prefer non-destructive proofs-of-concept, use test accounts, and never destroy data or degrade service.
- 🔗 **Third-party tools:** the tools and repositories referenced here are external projects owned by their respective authors, under their own licenses. This project does not bundle or endorse misuse of them — it points to them for legitimate research.
- 📷 **Report responsibly.** Findings are for the affected vendor / program, not for public exposure or extortion.

**You alone are responsible for how you use this material.** The authors and contributors accept no liability for misuse or for any damage arising from use of this repository. It is provided **"as is", without warranty of any kind.** If you are not authorised to test a target, **stop.**

By using this repository you agree you are acting lawfully and with authorisation.

---

## What's Inside

| Path | Purpose |
|---|---|
| [`SKILL.md`](./SKILL.md) | The skill definition — role, philosophy, hard boundaries, workflow, and the mandatory "200% confidence" reporting rule. |
| [`references/`](./references) | Dense, weaponised knowledge base: OWASP mappings, vuln-class playbooks, WordPress/SaaS/billing specifics, a deeply-researched GitHub tooling ecosystem, real paid-finding data, and reporting templates. |

Highlights in `references/`:

- **`github-bugbounty-ecosystem.md`** — a deeply-researched, *use-tuned* catalogue of the best public tools/knowledge bases: for each, what it actually does, the single highest-ROI use for finding a real bug, an exact command or section, and how it maps to the top paid bug classes. Stale/dead entries are flagged with their modern replacements.
- **`real-paid-findings.md`** — what actually gets paid (HackerOne 2025/2026 data) and the priority order to hunt in.
- **`advanced-techniques.md`**, **`common-vuln-classes.md`** — dense technique references (request smuggling, SSRF, GraphQL, JWT, race conditions, chaining).
- **`wordpress-specialized.md`**, **`saas-specialized.md`**, **`woocommerce-billing-ai.md`** — domain-specific attack surface and logic-flaw playbooks.
- **`owasp-top10-2025.md`**, **`owasp-api-top10-2023.md`**, **`methodology-checklist.md`** — standards and a systematic checklist.
- **`proof-and-reporting.md`**, **`report-template.md`** — evidence discipline and a payout-ready report format.

## Core Principles

1. **Impact over noise** — one confirmed high-impact bug beats fifty theoretical mediums.
2. **Proof is mandatory** — no "Confirmed" without self-reproduced, numbered steps and demonstrated impact.
3. **200% confidence rule** — verify from multiple angles, identities, and tenants before labelling a finding Confirmed.
4. **Attacker mindset, ethical boundaries** — question every developer assumption, but stay strictly in authorised scope.
5. **Root-cause fixes** — recommend real, server-side, enforceable controls, never band-aids.

## Install & Use with Claude

This is a standard [Agent Skill](https://agentskills.io): a `SKILL.md` (with `name` + `description` frontmatter) plus a `references/` folder. Claude loads only the metadata until the skill is triggered, then reads `SKILL.md` and pulls in reference files on demand. It bundles **no executable code** — just markdown guidance — so it's safe to audit before use (and you should: read `SKILL.md` and `references/`).

### Claude Code (recommended — filesystem, no upload)

Clone the repo straight into your skills directory. **Personal** (all projects):

```bash
git clone https://github.com/rohit-patil-lifecode/security-analysis.git \
  ~/.claude/skills/security-analysis
```

**Project-scoped** (checked in for a team, run from the repo root):

```bash
git clone https://github.com/rohit-patil-lifecode/security-analysis.git \
  .claude/skills/security-analysis
```

The folder must be `…/skills/security-analysis/` with `SKILL.md` at its top level (this repo is already laid out that way). Then, in Claude Code:

- **Automatic:** just ask — "hunt for IDOR in this WooCommerce plugin", "recon this authorised SaaS target", "review this WordPress plugin for auth bypass" — Claude triggers the skill from its `description`.
- **Explicit:** run `/security-analysis`.

Claude Code gives the skill **full local network/shell access**, so the referenced CLI tools (nuclei, subfinder, wpscan, ffuf, …) actually run — install the ones you need first. Share it with a team via [Claude Code Plugins](https://code.claude.com/docs/en/plugins).

### claude.ai (web / desktop apps)

Zip the skill folder (with `SKILL.md` at the zip's top level) and upload it under **Settings → Features → Skills** (Pro, Max, Team, or Enterprise, with file creation / code execution enabled):

```bash
zip -r security-analysis.zip SKILL.md references/
```

Custom skills on claude.ai are per-user (not org-shared). Network access in that sandbox varies by plan/admin settings, so treat it as **methodology/guidance** and run the actual tooling from your own machine.

### Claude API

Upload via the Skills API (`/v1/skills`), then reference the returned `skill_id` in the `container` parameter alongside the [code execution tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool), with the `skills-2025-10-02` beta header. Note: the API container has **no network access and no runtime installs**, so the referenced scanners won't run there — the skill acts as expert guidance; run the tools elsewhere.

> The skill's `name` is `security-analysis` and triggers on: bug hunt, bounty, WordPress, WooCommerce, SaaS, multi-tenant, billing, AI SaaS, GraphQL, recon, or security analysis — **for authorised targets only.**

## Contributing

Improvements to techniques, references, and accuracy are welcome. Keep additions **legitimate, non-destructive, and educational**, tuned to helping defenders and authorised testers find and fix real vulnerabilities.

## License

Unless a referenced third-party project states otherwise, the original content in this repository is provided for educational and authorised-testing use. See individual referenced repositories for their own licenses.
