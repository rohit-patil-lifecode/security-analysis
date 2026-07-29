# Proof Requirements, Confidence & Root-Cause Fixes

## Absolute Rules

1. **200% sure before Confirmed**  
   Verify from multiple angles, scenarios, identities, and request methods. If any doubt remains, use High Confidence or Hypothesis and state what is missing.

2. **No confirmed report without**:
   - Working reproduction you performed yourself
   - Exact numbered steps a developer can follow without questions
   - Evidence of actual impact (data returned, privilege gained, money/credits affected, etc.)

3. **Legitimate root-cause fix required**  
   Every confirmed finding must include a real fix that addresses the root cause, not a symptom.

## Confidence Labels

| Label            | When to use |
|------------------|-------------|
| **Confirmed**    | Reproduced multiple ways, zero remaining doubt, exact steps + evidence ready |
| **High Confidence** | Strong evidence but one scenario or environment still unverified |
| **Hypothesis**   | Plausible but not yet fully proven — do not treat as a paid finding |

## What Makes a Good Root-Cause Fix

Good fixes:
- Enforce authorization on the server for every object and action
- Remove client-trusted tenant/user IDs as the source of truth
- Use atomic check-and-act operations for races
- Apply capability + object ownership checks (WordPress)
- Bind tokens to user/tenant and validate claims properly
- Disable dangerous functionality or require proper authentication

Bad / incomplete fixes (do not suggest):
- “Add a rate limit” when the issue is missing authorization
- Client-side only checks
- Hiding an endpoint while the API still works
- Logging without blocking
- Partial patches that leave the same class of bug open

## Developer-Ready Reproduction Steps

Steps must:
- Start from a clean / low-privilege state
- Include exact URLs, methods, parameters, headers, and bodies
- Show expected vs actual result
- Be copy-paste friendly where possible (curl or raw HTTP)
- Work for someone who has never seen the application before

## Report Quality Checklist

- [ ] Scope confirmed
- [ ] Reproduced by you
- [ ] Multiple angles / identities tested
- [ ] Exact numbered steps written
- [ ] Evidence attached / described
- [ ] Impact demonstrated (not theoretical)
- [ ] Confidence correctly labelled
- [ ] Root cause identified
- [ ] Legitimate root-cause fix suggested
- [ ] CVSS / CWE / OWASP mapped
