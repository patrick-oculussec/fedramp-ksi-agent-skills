# FedRAMP 20x Compliance

> **Scope:** Master orchestrator for FedRAMP 20x Key Security Indicator compliance. Guides code generation, reviews code and IaC, and generates FedRAMP documentation. Apply when working with FedRAMP, 20x, KSI, compliance review, authorization package, or security assessment.

## Overview

These instructions orchestrate FedRAMP 20x Key Security Indicator (KSI) compliance across code generation, compliance review, and documentation. They delegate to theme-specific instruction sets based on context.

## Determining Mode

Activate the appropriate mode based on user intent:

**Code Generation** -- User is writing new code or IaC
- Apply all relevant KSI guardrails as defaults
- Reference theme-specific instructions for detailed patterns

**Compliance Review** -- User wants to check existing code
- Walk through each relevant KSI theme's checklist
- Flag non-compliant patterns with specific KSI IDs

**Documentation Generation** -- User needs FedRAMP artifacts
- Generate KSI-CSX-SUM implementation summaries
- Produce evidence templates and process documentation

## Theme Routing

Match the current task to the appropriate instruction set(s):

| If the task involves... | Read |
|---|---|
| Authentication, authorization, MFA, accounts, roles, permissions | [fedramp-iam](../fedramp-iam/README.md) |
| Network architecture, containers, HA, load balancing, attack surface | [fedramp-cloud-native](../fedramp-cloud-native/README.md) |
| Encryption, TLS, secrets, certificates, config management | [fedramp-service-config](../fedramp-service-config/README.md) |
| Deployments, CI/CD, version control, change tracking | [fedramp-change-mgmt](../fedramp-change-mgmt/README.md) |
| Logging, monitoring, auditing, SIEM, alerts | [fedramp-monitoring](../fedramp-monitoring/README.md) |
| Dependencies, third-party libraries, SBOMs, supply chain | [fedramp-supply-chain](../fedramp-supply-chain/README.md) |
| Backups, disaster recovery, RTO/RPO, failover | [fedramp-recovery](../fedramp-recovery/README.md) |
| Incident handling, forensics, after-action reports | [fedramp-incident-response](../fedramp-incident-response/README.md) |
| FedRAMP process docs, authorization packages, crypto modules | [fedramp-authorization](../fedramp-authorization/README.md) |
| Security training, cybersecurity awareness | [fedramp-cybersecurity-ed](../fedramp-cybersecurity-ed/README.md) |
| Security policy, asset inventory, SDLC, vulnerability disclosure | [fedramp-policy-inventory](../fedramp-policy-inventory/README.md) |

For full compliance reviews, iterate through all 11 themes.

## Full Compliance Review Workflow

When asked for a comprehensive review:

1. Read [ksi-reference.md](ksi-reference.md) for the complete KSI index
2. For each theme, read the corresponding README.md
3. Evaluate the code/system against each applicable KSI
4. Report findings organized by theme, referencing specific KSI IDs
5. Classify findings as:
   - **FAIL** -- Requirement violated, must fix
   - **WARN** -- Recommendation not followed or partial implementation
   - **PASS** -- Requirement met
   - **N/A** -- Not applicable to this code/system

## KSI-CSX-SUM Implementation Summary Template

Every KSI requires a provider implementation summary (per KSI-CSX-SUM). When generating documentation, use this template for each KSI:

```markdown
## [KSI-ID]: [KSI Title]

### 1. Implementation Goals
- Goal: [What will be implemented]
- Pass criteria: [Specific measurable criteria]
- Fail criteria: [What constitutes failure]
- Traceability: [How this maps to the KSI requirement]

### 2. Information Resources
- [List consolidated resources to be validated]
- [e.g., "All IAM policies in /infrastructure/iam/"]
- [e.g., "All users with privileged access in the Admin group"]

### 3. Machine-Based Validation
- Process: [Automated validation method]
- Cycle: [How often -- persistent/daily/weekly]
- Tools: [What tools perform the validation]

### 4. Non-Machine-Based Validation
- Process: [Manual review method]
- Cycle: [How often -- quarterly/annually]
- Responsible party: [Who performs this]

### 5. Current Status
- [ ] Implemented
- [ ] Validated
- [ ] Documented

### 6. Clarifications
[Any notes or responses to assessment findings]
```

## Cross-Cutting Requirements

These apply across ALL KSI themes:

- **Persistent validation**: Automated checks must run continuously or on a defined recurring cycle, not just at initial deployment
- **Immutable infrastructure**: Prefer redeployment over modification (KSI-CMT-RMV)
- **FIPS-validated cryptography**: All crypto protecting federal data must use FIPS 140-validated modules (KSI-AFR-UCM)
- **Least privilege**: Every access grant follows least-privilege principles (KSI-IAM-ELP)
- **Encryption in transit**: All network traffic must be encrypted (KSI-SVC-SNT)

## Additional Resources

- For the complete KSI index with all IDs and descriptions, see [ksi-reference.md](ksi-reference.md)
- [FedRAMP 20x KSI Documentation](https://www.fedramp.gov/docs/20x/key-security-indicators/)
- [FedRAMP 20x Definitions](https://www.fedramp.gov/docs/20x/fedramp-definitions/)

---

*Copyright (c) 2026 Patrick Clark / Oculus Security, LLC. [MIT License](../LICENSE).*
