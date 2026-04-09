# FedRAMP Authorization by FedRAMP

> **Scope:** Enforce FedRAMP 20x Authorization by FedRAMP KSIs. Covers minimum assessment scope, authorization data sharing, cryptographic module selection (FIPS 140), vulnerability detection and response, significant change notifications, persistent validation, secure configuration guides, and FedRAMP process documentation. Apply when preparing authorization packages, selecting cryptographic modules, documenting FIPS compliance, or generating FedRAMP process documents.

KSI Theme: A secure cloud service provider seeking FedRAMP authorization will address all FedRAMP 20x requirements and recommendations, including government-specific requirements for maintaining a secure system and reporting on activities to government customers.

## Code Generation Guardrails

### Minimum Assessment Scope (KSI-AFR-MAS)

- All components handling federal data must be identified and documented
- Scope includes: application code, infrastructure, third-party services, data flows
- Boundary diagram must be maintained as code (Mermaid, PlantUML, or similar)
- Changes to scope trigger significant change notification process

### Using Cryptographic Modules (KSI-AFR-UCM)

When writing code that protects federal customer data:

- Use ONLY FIPS 140-2 or FIPS 140-3 validated cryptographic modules
- Verify validation status at CMVP: https://csrc.nist.gov/projects/cryptographic-module-validation-program
- Approved algorithms: AES (128/256), RSA (2048+), ECDSA (P-256+), SHA-2 (256+), HMAC
- Prohibited: MD5, SHA-1 (for signatures), DES, 3DES, RC4, custom crypto
- Document the CMVP certificate number for each cryptographic module in use
- Enable FIPS mode in runtime/OS when available (e.g., Go `GOFLAGS=-tags=boringcrypto`, OpenSSL FIPS provider)

### Vulnerability Detection and Response (KSI-AFR-VDR)

- Document the full vulnerability management methodology
- Include: scanning tools, scan frequency, severity classification, response SLAs, exception process
- Align with FedRAMP VDR process requirements

### Significant Change Notifications (KSI-AFR-SCN)

- Define what constitutes a significant change for this CSO
- Implement automated detection of significant changes (boundary changes, new services, major version upgrades)
- Notification workflow must route to all necessary parties

### Persistent Validation and Assessment (KSI-AFR-PVA)

- Security controls must be continuously validated, not just at authorization time
- Implement automated evidence collection for each KSI
- Assessment results fed into Ongoing Authorization Reports

### Secure Configuration Guide (KSI-AFR-SCG)

- Publish customer-facing secure configuration guidance
- Default configurations must be secure by default
- Document all configurable security settings with recommended values
- Provide configuration validation tools for customers

### Authorization Data Sharing (KSI-AFR-ADS)

- Define format and mechanism for sharing authorization data
- Authorization data must be accessible to all necessary parties
- Keep authorization data current as system changes

### Collaborative Continuous Monitoring (KSI-AFR-CCM)

- Produce Ongoing Authorization Reports per FedRAMP schedule
- Include: vulnerability status, incident summary, change summary, risk posture
- Support Quarterly Reviews with agencies

### FedRAMP Security Inbox (KSI-AFR-FSI)

- Maintain monitored inbox for FedRAMP and government communications
- Response time SLAs defined and tracked

### Incident Communications Procedures (KSI-AFR-ICP)

- FedRAMP ICP integrated into incident response workflow (see [fedramp-incident-response](../fedramp-incident-response/README.md))
- Federal agencies notified within required timeframes

## Compliance Review Checklist

- [ ] **KSI-AFR-MAS**: Assessment scope documented; all components handling federal data identified
- [ ] **KSI-AFR-UCM**: All crypto modules FIPS 140-validated; CMVP certificate numbers documented
- [ ] **KSI-AFR-VDR**: Vulnerability management methodology documented per VDR process
- [ ] **KSI-AFR-SCN**: Significant change criteria defined; notification workflow operational
- [ ] **KSI-AFR-PVA**: Automated evidence collection active; assessment results current
- [ ] **KSI-AFR-SCG**: Secure config guide published; defaults are secure
- [ ] **KSI-AFR-ADS**: Authorization data shared with all necessary parties
- [ ] **KSI-AFR-CCM**: Ongoing Authorization Reports produced on schedule
- [ ] **KSI-AFR-FSI**: Security inbox operational; response SLAs defined
- [ ] **KSI-AFR-ICP**: ICP integrated into IR workflow; notification timelines met

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| Using non-FIPS crypto (e.g., libsodium without FIPS mode) | KSI-AFR-UCM | Switch to FIPS-validated module |
| No CMVP certificate documented | KSI-AFR-UCM | Identify and record certificate number |
| System boundary not documented | KSI-AFR-MAS | Create and maintain boundary diagram |
| Security controls only assessed at initial auth | KSI-AFR-PVA | Implement continuous validation |
| No customer-facing security config guide | KSI-AFR-SCG | Create and publish SCG |
| SHA-1 used for any cryptographic purpose | KSI-AFR-UCM | Migrate to SHA-256+ |

## Documentation Generation

For process document templates and KSI-CSX-SUM summaries, see [processes.md](processes.md).

## Additional Resources

- For FedRAMP process templates and FIPS guidance, see [processes.md](processes.md)
- [NIST CMVP Validated Modules](https://csrc.nist.gov/projects/cryptographic-module-validation-program/validated-modules)
- [FedRAMP Minimum Assessment Scope](https://fedramp.gov/docs/20x/minimum-assessment-scope)
- [FedRAMP Using Cryptographic Modules](https://fedramp.gov/docs/20x/using-cryptographic-modules)
