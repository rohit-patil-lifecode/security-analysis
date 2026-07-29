# Payout-Ready Report Template

## Title
[Severity] [Vulnerability Class] in [Component/Endpoint] allows [Specific Demonstrated Impact]

## Summary
2–4 sentences: what the bug is, who can trigger it, and what they gain.

## Asset / Attack Surface
- URL / endpoint / plugin / feature
- Affected versions (if known)
- Authentication / role required

## Root Cause
Exact missing control, wrong trust boundary, or flawed logic.

## Impact
What an attacker can actually do (data access, privilege gain, financial impact, RCE, tenant escape, etc.). Demonstrated, not theoretical.

## Confidence
Confirmed / High Confidence / Hypothesis  
(Only use Confirmed when verified from multiple angles with zero doubt.)

## Steps to Reproduce
1. ...
2. ...
3. ...
(Exact, numbered, developer can follow without questions. Include requests/responses or equivalent evidence.)

## Evidence
Raw requests/responses, observed data, privilege change, or clear description of proof.

## CVSS / CWE / OWASP
- CVSS vector and score
- CWE-ID
- OWASP category

## Business Impact
Why this matters to the organisation (data breach risk, financial loss, compliance, reputation).

## Recommended Fix (Root Cause)
Describe the legitimate server-side control that must be implemented so the same class of issue cannot recur. Be concrete and implementable. Avoid band-aids.

## References
Relevant advisories, CWE, OWASP, or public research.
