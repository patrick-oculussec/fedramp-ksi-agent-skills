# FedRAMP 20x Compliance Agent Instructions

This repository contains agent instructions for ensuring code, infrastructure-as-code, and documentation meet FedRAMP 20x Key Security Indicators (KSIs).

## How to Use These Instructions

When working on code that must comply with FedRAMP 20x, consult the relevant instruction sets below based on what you are building or reviewing. Each set contains:

1. **Code generation guardrails** -- defaults and patterns to follow
2. **Compliance review checklists** -- anti-patterns to flag in existing code
3. **Documentation templates** -- KSI-CSX-SUM implementation summaries

## Instruction Sets by Topic

| When working on... | Read |
|---|---|
| **Full FedRAMP compliance review** or unsure where to start | [fedramp-20x-compliance/README.md](fedramp-20x-compliance/README.md) |
| Authentication, authorization, MFA, accounts, roles, permissions | [fedramp-iam/README.md](fedramp-iam/README.md) |
| Network architecture, containers, HA, load balancing, attack surface | [fedramp-cloud-native/README.md](fedramp-cloud-native/README.md) |
| Encryption, TLS, secrets, certificates, configuration management | [fedramp-service-config/README.md](fedramp-service-config/README.md) |
| Deployments, CI/CD, version control, change tracking | [fedramp-change-mgmt/README.md](fedramp-change-mgmt/README.md) |
| Logging, monitoring, auditing, SIEM, alerts | [fedramp-monitoring/README.md](fedramp-monitoring/README.md) |
| Dependencies, third-party libraries, SBOMs, supply chain | [fedramp-supply-chain/README.md](fedramp-supply-chain/README.md) |
| Backups, disaster recovery, RTO/RPO, failover | [fedramp-recovery/README.md](fedramp-recovery/README.md) |
| Incident handling, forensics, after-action reports | [fedramp-incident-response/README.md](fedramp-incident-response/README.md) |
| FedRAMP process docs, authorization packages, FIPS crypto modules | [fedramp-authorization/README.md](fedramp-authorization/README.md) |
| Security training, cybersecurity awareness programs | [fedramp-cybersecurity-ed/README.md](fedramp-cybersecurity-ed/README.md) |
| Security policy, asset inventory, SDLC, vulnerability disclosure | [fedramp-policy-inventory/README.md](fedramp-policy-inventory/README.md) |

Each instruction set has a companion file (`patterns.md`, `processes.md`, or `templates.md`) with detailed code patterns, IaC examples, and documentation templates. Read those when you need specific implementation details.

## Cross-Cutting Requirements

These apply to ALL code in FedRAMP scope regardless of topic:

- **Persistent validation**: Automated checks must run continuously or on a defined recurring cycle, not just at initial deployment
- **Immutable infrastructure**: Prefer redeployment over modification (KSI-CMT-RMV)
- **FIPS-validated cryptography**: All crypto protecting federal data must use FIPS 140-validated modules (KSI-AFR-UCM)
- **Least privilege**: Every access grant follows least-privilege principles (KSI-IAM-ELP)
- **Encryption in transit**: All network traffic must be encrypted (KSI-SVC-SNT)
- **Secrets management**: No secrets in code, config files, or environment files (KSI-SVC-ASM)
- **Audit logging**: All security-relevant events must be logged to tamper-resistant storage (KSI-MLA-OSM)

## Complete KSI Reference

For the full list of all ~60 KSI IDs and their one-line descriptions, see [fedramp-20x-compliance/ksi-reference.md](fedramp-20x-compliance/ksi-reference.md).

---

Copyright (c) 2026 Patrick Clark / Oculus Security, LLC. MIT License. See [LICENSE](LICENSE).
