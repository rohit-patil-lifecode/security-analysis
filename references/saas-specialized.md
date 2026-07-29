# Multi-Tenant SaaS Elite Hunter — Dense Reference

Cross-tenant isolation failure is one of the highest-paying classes. Treat every tenant-scoped object as hostile.

## 1. Tenant Context Discovery

Map every place tenant/org/workspace/account ID appears:
- URL path or subdomain
- Query parameter
- Custom header (X-Tenant-ID, Tenantid, X-Org-Id, etc.)
- Cookie
- JWT / session claim
- Request body field (including nested filters, includes, expand)

If the server trusts any client-supplied value for tenant context → high probability of break.

## 2. Core Isolation Tests

For every API that returns or modifies data:
1. Authenticate as Tenant A user
2. Replace tenant/org/workspace ID with Tenant B’s ID
3. Observe whether Tenant B data is returned or modified
4. Also test nested structures: filters[tenantId], where[org_id], include[], etc.
5. Test “me” / “current” aliases — can they be replaced with real foreign IDs?

## 3. Object-Level (BOLA / IDOR) Across Tenants

- Sequential or predictable IDs are ideal
- Even UUIDs can be leaked via other endpoints, exports, webhooks, logs, or emails
- Test every resource type: users, projects, files, invoices, reports, messages, API keys, webhooks, integrations
- Export / download / report endpoints frequently forget tenant filters
- Search and autocomplete endpoints often leak cross-tenant data

## 4. Function-Level + Tenant

- Tenant-admin can reach platform-admin endpoints?
- Features gated only by frontend flags or plan claims that can be edited?
- Invitation acceptance that joins the wrong tenant or grants elevated role
- Support / impersonation endpoints reachable by ordinary tenants

## 5. Shared Resource Leaks

- Caches (Redis, CDN, application cache) without tenant prefix
- Background jobs / queues that process another tenant’s data
- Webhooks or email notifications that contain foreign data
- Logging / error messages / support tickets that leak across tenants
- File storage / S3 / GCS paths that are globally readable or guessable

## 6. Business Logic + Tenant Intersection

- Coupon / credit / seat limit races that affect other tenants
- Ability to apply another tenant’s promo or wallet
- Trial or plan limits bypassed across tenants
- Multi-account or self-referral that crosses tenant boundaries

## 7. Token & Session Issues

- JWT contains tenant_id or org_id claim that can be edited (alg confusion, weak secret, missing audience)
- Same email in two tenants → session fixation or token leakage
- OAuth / SAML / SSO misconfiguration allowing cross-tenant linking
- Refresh tokens not bound to tenant

## 8. Practical Workflow

1. Create two tenants (or use free trial + second account)
2. Map every identifier that appears in traffic
3. Systematically replay every request from Tenant A with Tenant B identifiers
4. Diff responses carefully — UI may hide fields that raw JSON still returns
5. Test nested and secondary parameters
6. Abuse invite, share, export, webhook, integration flows
7. Race any limited resource
8. Inspect tokens for editable claims

## 9. Impact Language for Reports

- “Any authenticated user can read/write every organization’s data”
- “Tenant A can enumerate and take over Tenant B admin”
- “Export endpoint returns all tenants’ invoices”
- Quantify (number of tenants, sensitivity of data, financial impact)

## 10. Quick Isolation Checklist

- [ ] Tenant ID source of truth is server-side only
- [ ] Every query filters by current tenant
- [ ] Nested filters cannot override tenant scope
- [ ] Exports, reports, searches, webhooks, jobs are tenant-scoped
- [ ] JWT / session claims cannot be used to switch tenant
- [ ] Invitation and ownership transfer flows are tightly controlled
- [ ] No shared caches or storage without tenant isolation
