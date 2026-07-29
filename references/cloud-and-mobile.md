# Cloud and Mobile High-Signal Patterns

## Cloud Infrastructure (AWS / GCP / Azure)

### Storage Misconfigurations
- Public buckets or objects (ListBucket, GetObject, PutObject to *).
- Missing Block Public Access / equivalent.
- Overly permissive bucket policies or ACLs.
- Public snapshots or machine images containing secrets.

### SSRF to Metadata
- AWS IMDS: 169.254.169.254 (prefer IMDSv2 checks; many SSRFs still succeed if headers can be set).
- GCP: metadata.google.internal / 169.254.169.254 with Metadata-Flavor header.
- Azure: 169.254.169.254 with specific headers.
- Retrieval of IAM role / service account credentials → privilege escalation.

### IAM & Identity
- Over-permissive roles attached to compute / Lambda / Cloud Functions.
- Ability to assume roles or escalate privileges once credentials obtained.
- Publicly accessible Cognito / identity pools with weak controls.

### Other Common Cloud Issues
- Publicly reachable databases, Redis, Elasticsearch, Kubernetes dashboards.
- Lambda / Function URLs without auth.
- Subdomain takeover via dangling DNS records pointing to cloud resources.
- Secrets in environment variables, user-data, or publicly readable configs.
- Missing encryption at rest or in transit for sensitive resources.

Always confirm the actual impact (what the obtained credentials or public access actually allow) rather than stopping at the misconfiguration itself.

## Mobile Applications

Reference the OWASP Mobile Application Security project:
- MASVS (verification standard)
- MASWE (weakness enumeration)
- MASTG (testing guide with concrete test cases)

High-value areas:
- Insecure data storage (unencrypted SQLite, SharedPreferences / UserDefaults with sensitive data, external storage).
- Weak or custom cryptography, hardcoded keys, insufficient key lengths or modes.
- Improper certificate validation or easily bypassable pinning.
- Exported components, deep links, or content providers without proper authorization.
- Insecure inter-process communication.
- Debuggable builds or excessive logging in production.
- Reverse-engineering resistance (obfuscation, anti-tampering) when required by threat model.
- Network traffic analysis for sensitive data in cleartext or weak TLS.
- Local authentication (biometric, PIN) bypass or insecure implementation.

When code or APK/IPA is available, perform static analysis focused on the above; dynamic analysis and runtime instrumentation when feasible within the engagement constraints.
