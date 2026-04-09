# Service Configuration Patterns

## TLS Configuration (KSI-SVC-SNT)

### Secure TLS Server Configuration

```yaml
tls_config:
  min_version: "TLS1.2"
  preferred_version: "TLS1.3"
  cipher_suites_tls12:
    - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
  # TLS 1.3 cipher suites are fixed by the spec
  hsts:
    max_age: 31536000
    include_subdomains: true
    preload: true
  certificate_transparency: required
```

### FIPS Mode Considerations

When protecting federal customer data:
- Use only FIPS 140-2/140-3 validated cryptographic modules
- AES-128 or AES-256 for symmetric encryption
- RSA-2048+ or ECDSA P-256+ for asymmetric operations
- SHA-256+ for hashing
- Verify module validation status at https://csrc.nist.gov/projects/cryptographic-module-validation-program

## Secrets Management (KSI-SVC-ASM)

### Secret Injection Pattern

```yaml
# Secrets referenced by name, resolved at runtime
service_config:
  database:
    host: "${CONFIG:db-host}"
    credentials:
      source: "secrets-manager"
      path: "/prod/app/db-credentials"
      rotation_days: 90
  api_keys:
    source: "secrets-manager"
    path: "/prod/app/external-api"
    rotation_days: 90
```

### Secret Rotation Automation

```yaml
rotation_policy:
  database_credentials:
    schedule: "every 90 days"
    method: "dual-credential"
    steps:
      - create_new_credential
      - deploy_new_credential_to_app
      - verify_connectivity
      - revoke_old_credential
    on_failure:
      - rollback_to_old_credential
      - alert_security_team
  tls_certificates:
    schedule: "30 days before expiration"
    method: "ACME"
    steps:
      - request_new_certificate
      - validate_certificate
      - deploy_certificate
      - verify_tls_handshake
    on_failure:
      - retain_current_certificate
      - alert_security_team
```

## Integrity Validation (KSI-SVC-VRI)

### Container Image Signing and Verification

```yaml
image_policy:
  verify_signatures: true
  allowed_registries:
    - "registry.internal.example.com"
  signature_verification:
    method: "cosign"
    public_key: "cosign.pub"
  reject_unsigned: true
```

### Artifact Verification Pipeline

```
1. Build artifact (binary, container image, package)
2. Generate SHA-256 hash of artifact
3. Sign hash with release signing key
4. Store artifact + hash + signature in artifact registry
5. At deployment: verify signature, then verify hash matches artifact
6. Reject deployment if either verification fails
```

## KSI-CSX-SUM Templates for SVC

### KSI-SVC-SNT: Securing Network Traffic

**1. Implementation Goals**
- Goal: All network traffic encrypted with TLS 1.2+ using FIPS-validated modules
- Pass criteria: Zero unencrypted network flows; all endpoints enforce TLS 1.2+; no weak ciphers
- Fail criteria: Any plaintext HTTP endpoint; any TLS < 1.2; any weak cipher suite in use
- Traceability: Maps to KSI-SVC-SNT

**2. Information Resources**
- TLS configurations for all endpoints (ingress controllers, load balancers, services)
- Certificate inventory
- Internal service mesh configuration

**3. Machine-Based Validation**
- Process: TLS scanner checks all endpoints for version/cipher compliance; certificate expiry monitoring
- Cycle: Persistent (every deployment) + daily endpoint scan
- Tools: [TLS scanner, certificate monitor]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of TLS policy and exception list
- Cycle: Quarterly
- Responsible party: Security engineering team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any internal services with TLS termination at mesh layer]

---

### KSI-SVC-ASM: Automating Secret Management

**1. Implementation Goals**
- Goal: All secrets managed by a dedicated secrets manager with automated rotation
- Pass criteria: Zero hardcoded secrets; all secrets rotated within defined schedule; access audited
- Fail criteria: Any secret in source code, config files, or environment files; any expired/unrotated secret
- Traceability: Maps to KSI-SVC-ASM

**2. Information Resources**
- Secrets manager inventory
- Secret rotation schedules and logs
- Source code scan results for credential patterns

**3. Machine-Based Validation**
- Process: Pre-commit hooks and CI scan for credential patterns; secrets manager reports on rotation compliance
- Cycle: Persistent (every commit) + weekly rotation compliance report
- Tools: [Secret scanner, secrets manager audit log]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of secret inventory, rotation policies, and access logs
- Cycle: Quarterly
- Responsible party: Security engineering team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any secrets with rotation schedules longer than 90 days with justification]
