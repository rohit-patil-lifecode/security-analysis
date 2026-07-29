# WooCommerce, Billing, Payment & AI SaaS Hunter Reference

## WooCommerce / E-commerce Logic

High-value targets:
- Orders, coupons, cart, taxes, shipping, stock, refunds
- Subscriptions, Store API, payments, wallet/credits/points/gift cards
- Referral systems

Hunt for:
- Negative totals / quantities / prices
- Free checkout or zero-total bypass
- Coupon stacking, reuse, state confusion (cancel payment leaves coupon live)
- Race conditions on coupon redemption, stock, seat limits
- Order ownership IDOR / invoice exposure
- Webhook replay or signature bypass
- Payment / subscription / license bypass
- Guest checkout → customer impersonation
- Inventory / stock manipulation
- Double-spend or order replay

## Billing Platforms (Stripe, Paddle, LemonSqueezy, PayPal, Razorpay, Chargebee, Recurly, FastSpring)

- Webhook replay / missing or weak signature verification
- Trial abuse / restart
- Subscription upgrade/downgrade race
- Invoice forgery or modification
- Coupon / promo replay
- Price / currency / tax manipulation
- Negative pricing
- Double refunds
- Credits / AI-credits / usage-limit abuse or reset
- Plan feature unlock via parameter or race

## Multi-Tenant + Billing Intersection

- Cross-tenant invoice / subscription / usage data
- Ability to apply another tenant’s coupon or credits
- Invitation flows that grant billing privileges
- Export of billing data across tenants

## AI SaaS Specific

Inspect:
- Prompt APIs, file/knowledge-base uploads
- Embeddings, vector DBs, memory/context, streaming
- Agents, tools, plugins, model selection
- Usage/credit metering

Hunt for:
- Prompt injection / jailbreak that leaks system prompt or tools
- Cross-user or cross-tenant memory / context leakage
- Embedding / vector DB leakage or poisoning (RAG poisoning)
- Tool / agent privilege escalation
- Secret disclosure via model responses or logs
- Unlimited AI credits / usage-limit bypass / race
- File upload → prompt injection or RCE path
- Knowledge-base IDOR (read another tenant’s documents)

## File Upload Universal Checks (applies to WP, SaaS, AI)

- Filename / extension / Content-Type / magic-byte mismatch
- Polyglot files, SVG XSS/XXE, EXIF, Ghostscript
- ZIP/RAR Slip (path traversal inside archive)
- Decompression bombs, oversized uploads
- Overwrite of critical files
- Upload to executable location or subsequent include
