# IAM Code Patterns and Documentation Templates

## Authentication Patterns

### WebAuthn/FIDO2 Registration Flow (KSI-IAM-MFA)

```
1. Server generates challenge with user info and relying party ID
2. Client calls navigator.credentials.create() with challenge
3. Authenticator creates key pair, signs challenge
4. Server validates attestation, stores public key
5. Subsequent logins use navigator.credentials.get() with assertion
```

Key requirements:
- Store credential public key, credential ID, sign count, and transports
- Validate sign count is monotonically increasing to detect cloning
- Support multiple credentials per user for recovery

### Service-to-Service Auth (KSI-IAM-SNU)

**mTLS pattern:**
```
1. Each service has unique X.509 certificate from internal CA
2. Both sides validate certificate chain during TLS handshake
3. Application extracts service identity from client certificate CN/SAN
4. Authorization decision based on verified service identity
```

**Workload identity pattern (cloud-agnostic):**
```
1. Workload requests identity token from platform metadata service
2. Platform issues short-lived JWT bound to workload identity
3. Receiving service validates JWT signature against platform JWKS
4. Authorization based on claims in validated JWT
```

### JIT Privilege Elevation (KSI-IAM-JIT)

```
1. User requests elevated access with justification
2. Request routed to approval workflow (or auto-approved for on-call)
3. Elevated role granted with TTL (e.g., 1-4 hours)
4. All actions under elevated role logged with request correlation ID
5. Role automatically revoked at TTL expiration
6. Post-session review of actions taken
```

## IaC Patterns

### Least-Privilege Role Definition

```yaml
# Principle: start with zero permissions, add only what's needed
role:
  name: "app-service-read"
  description: "Read-only access to application data store"
  permissions:
    - resource: "datastore/app-data"
      actions: ["read", "list"]
      conditions:
        - source_ip_range: "10.0.0.0/8"
        - require_mfa: true
  deny:
    - resource: "*"
      actions: ["*"]
```

### Account Lifecycle Automation

```yaml
# Automated deprovisioning trigger
account_lifecycle:
  dormant_threshold_days: 90
  warning_notification_days: [75, 85]
  on_dormant:
    - disable_account
    - revoke_active_sessions
    - notify_manager
    - create_audit_record
  on_offboard:
    - disable_account
    - revoke_all_credentials
    - transfer_data_ownership
    - remove_group_memberships
    - create_audit_record
```

## KSI-CSX-SUM Templates for IAM

### KSI-IAM-MFA: Enforcing Phishing-Resistant MFA

**1. Implementation Goals**
- Goal: All user authentication requires MFA; phishing-resistant methods (WebAuthn/FIDO2) are the default
- Pass criteria: 100% of user logins require MFA; phishing-resistant method available for all users
- Fail criteria: Any user login path exists without MFA requirement
- Traceability: Maps to KSI-IAM-MFA

**2. Information Resources**
- Authentication configuration in identity provider
- MFA enrollment records for all users
- Authentication flow code/configuration

**3. Machine-Based Validation**
- Process: Automated test suite attempts login without MFA; must be rejected. Scan IdP config for MFA enforcement flag.
- Cycle: Persistent (every deployment) + daily configuration scan
- Tools: [Integration test suite, IdP configuration scanner]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of MFA enrollment rates and method distribution
- Cycle: Quarterly
- Responsible party: Security team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Notes on any edge cases, e.g., break-glass accounts with hardware token backup]

---

### KSI-IAM-ELP: Ensuring Least Privilege

**1. Implementation Goals**
- Goal: Every identity (user and non-user) has only the minimum permissions required
- Pass criteria: No identity has wildcard permissions; all roles reviewed within last 90 days
- Fail criteria: Any identity with unused permissions beyond 30-day grace period
- Traceability: Maps to KSI-IAM-ELP

**2. Information Resources**
- All IAM roles and policies
- Permission usage logs
- Service account inventory

**3. Machine-Based Validation**
- Process: Automated scan for wildcard permissions, unused permissions, and overly broad roles
- Cycle: Persistent (on every IaC change) + weekly full scan
- Tools: [IAM policy analyzer, permission usage tracker]

**4. Non-Machine-Based Validation**
- Process: Quarterly access review with role owners certifying necessity of each permission
- Cycle: Quarterly
- Responsible party: Role owners + security team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Notes on any roles requiring broad permissions with compensating controls]
