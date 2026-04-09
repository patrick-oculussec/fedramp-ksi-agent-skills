# FedRAMP Service Configuration

> **Scope:** Enforce FedRAMP 20x Service Configuration KSIs in code and IaC. Covers encryption, TLS, FIPS cryptography, secrets management, certificate rotation, configuration-as-code, integrity validation, and data handling. Apply when writing encryption logic, TLS configuration, secrets management, certificate handling, or configuration management code.

KSI Theme: A secure cloud service offering will follow FedRAMP encryption policies, continuously verify information resource integrity, and restrict access to third-party information resources.

## Code Generation Guardrails

### Network Encryption (KSI-SVC-SNT)

- All network traffic must be encrypted: TLS 1.2 minimum, prefer TLS 1.3
- Internal service-to-service traffic must also be encrypted (mTLS or service mesh TLS)
- Disable weak cipher suites: no RC4, DES, 3DES, MD5, SHA1 for signatures
- Redirect HTTP to HTTPS; set HSTS headers with minimum 1-year max-age
- Use FIPS 140-validated cryptographic modules for all encryption protecting federal data

### Resource Integrity Validation (KSI-SVC-VRI)

- Verify checksums/signatures on all downloaded artifacts (container images, packages, binaries)
- Use signed container images with signature verification at deployment
- Implement file integrity monitoring for critical system files
- Store and compare cryptographic hashes of deployed artifacts

### Configuration Management (KSI-SVC-ACM)

- All configuration must be version-controlled and declarative
- No manual configuration changes to running systems
- Environment-specific values injected via environment variables or config maps, never hardcoded
- Configuration changes go through the same review pipeline as code changes

### Secrets Management (KSI-SVC-ASM)

- Store all secrets in a dedicated secrets manager (never in code, config files, or env files)
- Automate rotation of all secrets on a defined schedule:
  - API keys: 90 days maximum
  - Certificates: before expiration with automated renewal
  - Database credentials: 90 days maximum
- Secrets must be injected at runtime, not baked into images or artifacts
- Audit all secret access; alert on unexpected retrieval patterns

### Security Improvement (KSI-SVC-EIS)

- Implement automated security scanning in CI/CD (SAST, DAST, dependency scanning)
- Track and remediate findings with defined SLAs
- Feed scan results back into development workflow

### Communications Validation (KSI-SVC-VCM) -- Moderate

- Validate mTLS certificates for all service-to-service communication
- Verify message integrity using HMACs or digital signatures for critical data flows
- Implement certificate pinning for critical external integrations

### Residual Risk Prevention (KSI-SVC-PRR) -- Moderate

- Sanitize resources after changes: clear temporary files, revoke old credentials
- Review IaC diffs for unintended exposure before applying
- Decommission unused resources; don't leave orphaned infrastructure

### Data Removal (KSI-SVC-RUD) -- Moderate

- Implement data deletion APIs that support customer data removal requests
- Deletion must propagate to replicas and backups per retention policy
- Log all deletion requests and confirmations for audit

## Compliance Review Checklist

- [ ] **KSI-SVC-SNT**: TLS 1.2+ on all endpoints; no weak ciphers; internal traffic encrypted
- [ ] **KSI-SVC-VRI**: Artifact signatures/checksums verified; file integrity monitoring active
- [ ] **KSI-SVC-ACM**: All config in version control; no manual changes to running systems
- [ ] **KSI-SVC-ASM**: No secrets in code/config; automated rotation configured; access audited
- [ ] **KSI-SVC-EIS**: Security scanning in CI/CD; findings tracked with SLAs
- [ ] **KSI-SVC-VCM**: Service-to-service mTLS validated; message integrity verified (Moderate)
- [ ] **KSI-SVC-PRR**: Resources sanitized after changes; no orphaned infrastructure (Moderate)
- [ ] **KSI-SVC-RUD**: Data deletion API exists; propagates to replicas/backups (Moderate)

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| TLS 1.0 or 1.1 enabled | KSI-SVC-SNT | Enforce TLS 1.2 minimum |
| Self-signed certs without verification | KSI-SVC-VRI | Use trusted CA; enable verification |
| Secrets in environment files committed to git | KSI-SVC-ASM | Move to secrets manager; add to .gitignore |
| No hash verification on downloaded dependencies | KSI-SVC-VRI | Enable lockfile integrity checks |
| Manual server configuration via SSH | KSI-SVC-ACM | Replace with IaC and config management |
| Non-FIPS crypto library for federal data | KSI-SVC-SNT | Switch to FIPS-validated module |
| Certificates with no auto-renewal | KSI-SVC-ASM | Implement automated cert renewal |
| `verify=False` or `InsecureSkipVerify` in TLS | KSI-SVC-SNT | Remove; configure proper CA trust |

## Documentation Generation

For KSI-CSX-SUM implementation summaries, see [patterns.md](patterns.md).

## Additional Resources

- For detailed code patterns and configuration examples, see [patterns.md](patterns.md)
- [NIST SP 800-52 Rev2: TLS Implementation Guidelines](https://csrc.nist.gov/publications/detail/sp/800-52/rev-2/final)
- [FedRAMP Using Cryptographic Modules](https://fedramp.gov/docs/20x/using-cryptographic-modules)
