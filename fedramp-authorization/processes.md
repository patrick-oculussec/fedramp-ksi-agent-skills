# Authorization by FedRAMP Process Templates

## FIPS Cryptographic Module Inventory (KSI-AFR-UCM)

### Module Documentation Template

```markdown
# Cryptographic Module Inventory

| Module | CMVP Cert # | Version | Algorithm | Use Case | FIPS Mode |
|---|---|---|---|---|---|
| OpenSSL | [cert#] | 3.x | AES-256-GCM, RSA-2048, SHA-256 | TLS termination | Enabled |
| BoringSSL | [cert#] | [ver] | AES-256-GCM, ECDSA P-256 | Go services TLS | Enabled |
| AWS-LC | [cert#] | [ver] | AES-256-GCM | S3 encryption | Enabled |
| [Module] | [cert#] | [ver] | [algorithms] | [use case] | [Yes/No] |
```

### FIPS Compliance Validation Steps

```
1. Inventory all cryptographic operations in the CSO
2. For each operation, identify the cryptographic module in use
3. Verify each module has a current CMVP validation certificate
4. Confirm FIPS mode is enabled in configuration
5. Verify only approved algorithms are used
6. Document exceptions with compensating controls and migration plan
```

## Significant Change Criteria (KSI-AFR-SCN)

### What Constitutes a Significant Change

```yaml
significant_changes:
  always_significant:
    - new_external_service_integration
    - change_to_authentication_mechanism
    - change_to_authorization_model
    - new_data_flow_crossing_boundary
    - change_to_encryption_at_rest_or_transit
    - change_to_network_architecture
    - new_customer_facing_feature_handling_federal_data
    - change_to_incident_response_procedures
    - migration_to_different_infrastructure_provider

  conditionally_significant:
    - major_version_upgrade_of_framework_or_runtime
    - change_to_backup_or_recovery_procedures
    - significant_change_to_ci_cd_pipeline
    - new_third_party_dependency_with_data_access

  not_significant:
    - minor_version_patch_updates
    - ui_cosmetic_changes
    - bug_fixes_not_affecting_security_controls
    - internal_documentation_updates
```

## Secure Configuration Guide Template (KSI-AFR-SCG)

```markdown
# [CSO Name] Secure Configuration Guide

## Overview
This guide provides secure configuration recommendations for [CSO Name]
for customers processing federal government data.

## Authentication Configuration
| Setting | Recommended Value | Default | Notes |
|---|---|---|---|
| MFA Enforcement | Enabled | Enabled | Required for all users |
| MFA Method | WebAuthn/FIDO2 | TOTP | Phishing-resistant preferred |
| Session Timeout | 15 minutes | 30 minutes | Adjust based on data sensitivity |
| Password Policy | 16+ chars, breach check | 12+ chars | If passwords used alongside MFA |

## Data Protection
| Setting | Recommended Value | Default | Notes |
|---|---|---|---|
| Encryption at Rest | AES-256 | AES-256 | FIPS-validated module |
| Encryption in Transit | TLS 1.3 | TLS 1.2 | TLS 1.2 minimum |
| Data Retention | Per agency policy | 90 days | Configurable per tenant |

## Logging and Monitoring
| Setting | Recommended Value | Default | Notes |
|---|---|---|---|
| Audit Logging | Enabled, all events | Enabled | Cannot be disabled |
| Log Export | SIEM integration | None | Customer configures target |
| Alert Notifications | Email + webhook | Email | Configure per agency requirements |

## Network Security
| Setting | Recommended Value | Default | Notes |
|---|---|---|---|
| IP Allowlisting | Agency CIDR ranges | Disabled | Recommended for admin access |
| API Rate Limiting | Enabled | Enabled | Configurable thresholds |

## Configuration Validation
Customers can validate their configuration against these recommendations
using [validation tool/endpoint/script].
```

## KSI-CSX-SUM Templates for AFR

### KSI-AFR-UCM: Using Cryptographic Modules

**1. Implementation Goals**
- Goal: All cryptographic operations use FIPS 140-validated modules with approved algorithms
- Pass criteria: 100% of crypto modules have current CMVP certificates; FIPS mode enabled everywhere
- Fail criteria: Any non-validated module protecting federal data; any prohibited algorithm in use
- Traceability: Maps to KSI-AFR-UCM

**2. Information Resources**
- Cryptographic module inventory with CMVP certificate numbers
- Configuration showing FIPS mode enabled
- Algorithm usage map per service

**3. Machine-Based Validation**
- Process: Configuration scanner verifies FIPS mode; static analysis detects prohibited algorithms
- Cycle: Persistent (every deployment) + weekly configuration scan
- Tools: [Config scanner, SAST with crypto rules]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of CMVP certificate validity and new crypto usage
- Cycle: Quarterly
- Responsible party: Security engineering team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any modules pending CMVP validation with timeline]

---

### KSI-AFR-MAS: Minimum Assessment Scope

**1. Implementation Goals**
- Goal: Complete identification and documentation of all components within assessment scope
- Pass criteria: All components handling federal data documented; boundary diagram current
- Fail criteria: Any undocumented component handling federal data
- Traceability: Maps to KSI-AFR-MAS

**2. Information Resources**
- System boundary diagram (maintained as code)
- Component inventory
- Data flow documentation

**3. Machine-Based Validation**
- Process: Compare running infrastructure against documented inventory; alert on undocumented components
- Cycle: Persistent (daily infrastructure scan)
- Tools: [Infrastructure discovery tool, inventory reconciliation]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of boundary diagram and component inventory with stakeholders
- Cycle: Quarterly
- Responsible party: System owner and security team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document shared responsibility boundaries with infrastructure provider]
