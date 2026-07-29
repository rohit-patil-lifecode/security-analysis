# HackerOne Platform Reference for Elite Hunters

## Quality Report Standards (Official Guidance)

A high-quality HackerOne report must allow the triage team to:
- Understand the bug immediately
- Reproduce it easily
- Assess coverage and impact

Required elements:
- Clear, specific title (not “XSS found”)
  - Good: “Stored XSS in user profile field allows script execution on profile view”
- Detailed numbered steps to reproduce (screenshots/recordings/code help)
- Clear impact explanation (what an attacker can actually do)
- Concise yet complete structure

Avoid:
- Vague statements
- Missing impact
- Poor formatting
- Theoretical language (“could potentially”)

## Report Writing Rules That Match This Skill

- Never claim impact you have not demonstrated
- Use two test accounts for IDOR/access-control issues
- Include exact HTTP request (copy-paste ready) and the response that proves the bug
- Keep reports focused and readable (triagers skim)
- Severity claimed must match demonstrated impact
- Recommended fix should be short and root-cause oriented

## Common High-Volume CWE Classes on HackerOne

Top reported weakness patterns (platform-wide):
- CWE-79 XSS
- CWE-200 Information Exposure
- CWE-284 Improper Access Control
- CWE-639 Authorization Bypass Through User-Controlled Key (classic IDOR)
- CWE-287 Improper Authentication
- CWE-89 SQL Injection
- CWE-918 SSRF
- CWE-352 CSRF
- CWE-444 HTTP Request Smuggling

Prioritise these classes when hunting.

## Code of Conduct Highlights (Must Follow)

- Professional behaviour only
- Never disclose private program details or report contents without authorization
- Contact security teams only through official channels
- No unsafe testing or service degradation
- No reputation farming or duplicate accounts
- AI use is allowed but human verification + working PoC is required; misuse can impact reputation
- Uncoordinated public disclosure is a violation

## Reputation & Signal

- High-quality, reproducible reports improve reputation and signal
- Low-effort or non-reproducible reports damage standing and can limit private program access
- Many programs now enforce signal thresholds

## WordPress on HackerOne

- WordPress Core, Gutenberg, WP-CLI, BuddyPress, bbPress etc. have official programs
- Plugins not explicitly listed are usually out of scope for the core program (report to Plugin Review team instead)
- Focus on severe, reproducible issues (XSS, CSRF, SSRF, RCE, SQLi, privilege escalation)

## Alignment with This Skill

This skill’s 200% confidence rule, exact developer-ready steps, demonstrated impact, and legitimate root-cause fixes are designed to produce reports that pass HackerOne triage and maximise legitimate payouts.
