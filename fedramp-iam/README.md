# FedRAMP IAM -- Identity and Access Management

> **Scope:** Enforce FedRAMP 20x Identity and Access Management KSIs in code and IaC. Covers MFA, phishing-resistant authentication, passwordless auth, RBAC, ABAC, least privilege, just-in-time access, service account security, and suspicious activity response. Apply when writing authentication, authorization, user management, role management, or access control code.

KSI Theme: A secure cloud service offering will protect user data, control access, and apply zero trust principles.

## Code Generation Guardrails

When generating authentication or authorization code, apply these defaults:

### Authentication (KSI-IAM-MFA, KSI-IAM-APM)

- Always implement MFA; prefer phishing-resistant methods (WebAuthn/FIDO2, PIV/CAC) over TOTP/SMS
- Default to passwordless authentication (passkeys, certificate-based) when feasible
- If passwords are required: enforce minimum 16 characters, check against breach databases, require MFA
- Never store passwords in plaintext; use bcrypt/scrypt/argon2 with appropriate work factors
- Session tokens must be cryptographically random, expire on inactivity, and be invalidated on logout

### Non-User Authentication (KSI-IAM-SNU)

- Service-to-service: use mTLS, OAuth2 client credentials with short-lived tokens, or workload identity
- Never use long-lived API keys or shared secrets embedded in code
- Machine identities must be rotated automatically on a defined schedule
- CI/CD pipelines must use ephemeral credentials (OIDC federation, workload identity)

### Authorization (KSI-IAM-JIT, KSI-IAM-ELP)

- Default-deny: no access unless explicitly granted
- Implement role-based (RBAC) or attribute-based (ABAC) access control
- Prefer just-in-time (JIT) privilege elevation over standing access for administrative operations
- Time-bound elevated privileges with automatic revocation
- Separate duties: no single role should have both approval and execution permissions

### Account Management (KSI-IAM-AAM)

- Automate account provisioning and deprovisioning via identity provider integration
- Enforce access reviews on a persistent cycle
- Disable dormant accounts automatically after a defined inactivity period
- Maintain audit trail for all account lifecycle events (create, modify, disable, delete)

### Suspicious Activity Response (KSI-IAM-SUS)

- Implement rate limiting and account lockout on repeated failed authentication
- Automatically disable or flag privileged accounts when anomalous behavior is detected
- Generate alerts on: impossible travel, credential stuffing patterns, privilege escalation attempts
- Log all authentication events with sufficient detail for forensic analysis

## Compliance Review Checklist

When reviewing existing code, check for:

- [ ] **KSI-IAM-MFA**: MFA enforced for all user auth; phishing-resistant method available
- [ ] **KSI-IAM-APM**: Passwordless supported where feasible; password policy meets minimum requirements
- [ ] **KSI-IAM-SNU**: No hardcoded credentials; service accounts use short-lived tokens or mTLS
- [ ] **KSI-IAM-JIT**: Admin access is JIT with automatic expiration, not standing
- [ ] **KSI-IAM-ELP**: Permissions follow least privilege; no wildcard grants (e.g., `*` policies)
- [ ] **KSI-IAM-SUS**: Anomaly detection configured for privileged accounts; auto-disable on suspicious activity
- [ ] **KSI-IAM-AAM**: Account lifecycle automated; deprovisioning tested; audit trail exists

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| Hardcoded API keys or passwords | KSI-IAM-SNU | Move to secrets manager; use workload identity |
| SMS-only MFA | KSI-IAM-MFA | Add phishing-resistant option (WebAuthn) |
| `admin` role with no expiration | KSI-IAM-JIT | Implement JIT elevation with TTL |
| Wildcard permissions (`Action: *`) | KSI-IAM-ELP | Scope to specific actions and resources |
| No account deprovisioning flow | KSI-IAM-AAM | Integrate with IdP; automate offboarding |
| Password stored as MD5/SHA1 hash | KSI-IAM-APM | Migrate to argon2/bcrypt |
| Shared service account credentials | KSI-IAM-SNU | Unique identity per service; rotate automatically |

## Documentation Generation

For KSI-CSX-SUM implementation summaries, see [patterns.md](patterns.md) for the IAM-specific template with pre-filled validation approaches.

## Additional Resources

- For detailed code patterns and IaC examples, see [patterns.md](patterns.md)
- [NIST SP 800-63B Digital Identity Guidelines](https://pages.nist.gov/800-63-4/sp800-63b.html)
- [CISA Zero Trust Maturity Model](https://www.cisa.gov/zero-trust-maturity-model)

---

*Copyright (c) 2026 Patrick Clark / Oculus Security, LLC. [MIT License](../LICENSE).*
