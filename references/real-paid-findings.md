# Real Paid Bug Bounty Findings (HackerOne 2025/2026 Data)

Source: HackerOne Hacker-Powered Security Report 2025/2026 + disclosed patterns.

## Top Vulnerability Types by Total Rewards (2025)

| Rank | Type                        | Approx. Rewards | Valid Reports | Trend      | Priority for this skill |
|------|-----------------------------|-----------------|---------------|------------|-------------------------|
| 1    | Improper Access Control     | $8.8M           | 8.8k          | ↑ 19%      | **Highest**             |
| 2    | XSS                         | $8.4M           | 13.2k         | ↓ volume   | High (esp. stored)      |
| 3    | IDOR                        | $7.6M           | 6.0k          | ↑ 23–29%   | **Highest**             |
| 4    | Information Disclosure      | $4.7M           | 8.0k          | stable     | High                    |
| 5    | Misconfiguration            | $2.7M           | 6.1k          | ↑ 22%      | Medium-High             |
| 6    | Improper Authentication     | $2.4M           | 1.7k          | ↓          | High                    |
| 7    | Business Logic Errors       | $2.3M           | 2.0k          | ↑ volume   | **Highest**             |
| 8    | Code Injection              | $1.7M           | 1.0k          | ↑          | High                    |
| 9    | SSRF                        | $1.5M           | 0.8k          | stable     | High                    |
| 10   | Privilege Escalation        | $1.5M           | 1.8k          | ↓          | High                    |
| 11   | SQL Injection               | $1.3M           | 1.2k          | ↓          | Medium-High             |

**Key signal**: Authorization failures (Improper Access Control + IDOR) are the structural growth area. XSS is still large in volume but declining in relative value. Business logic is rising and highly paid when impact is clear.

## What Actually Gets Paid at Higher Tiers

- **Account takeover** (auth bypass, OTP leak, magic-link abuse, session issues)
- **IDOR / BOLA** with sensitive data or state-changing impact (especially cross-tenant)
- **Business logic** with financial or privilege impact (payment bypass, credit abuse, feature unlock, race conditions)
- **SSRF** that reaches cloud metadata or internal services
- **Privilege escalation** (especially WordPress capability/role issues)
- **RCE / Code Injection** (rare but highest individual payouts)
- **Stored XSS** that affects other users or admins
- **Information disclosure** of secrets, PII at scale, or debug data that enables further attacks

## What Gets Rejected or Paid Low

- Self-XSS
- Missing security headers without proven impact
- Theoretical issues without working PoC
- Rate-limiting / DoS without clear security impact
- Out-of-scope assets
- Duplicate or previously known issues
- Reports that cannot be reproduced in < 5 minutes by triage

## Extreme Focus Areas for This Skill

Given the data, prioritise in this order for maximum legitimate bounty success:

1. **Authorization & Object-level access** (IDOR/BOLA/BFLA, missing capability checks, cross-tenant)
2. **Business logic & races** (payments, credits, subscriptions, coupons, feature flags)
3. **Authentication flaws** that lead to ATO
4. **SSRF → internal/cloud**
5. **WordPress-specific** privilege escalation, object injection, missing nonce+capability
6. **Stored XSS** with real impact
7. **Information disclosure** that enables chaining
8. Classic injection only when high-confidence and impactful

Always apply the 200% confidence rule + exact developer-ready steps + root-cause fix.
