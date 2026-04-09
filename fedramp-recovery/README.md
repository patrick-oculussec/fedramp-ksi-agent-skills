# FedRAMP Recovery Planning

> **Scope:** Enforce FedRAMP 20x Recovery Planning KSIs in code and IaC. Covers backup configuration, RTO/RPO definitions, recovery plan alignment, and recovery testing automation. Apply when implementing backup strategies, disaster recovery, failover, RTO, RPO, business continuity, or recovery testing.

KSI Theme: A secure cloud service offering will define, maintain, and test incident response plan(s) and recovery capabilities to ensure minimal service disruption and data loss during incidents and contingencies.

## Code Generation Guardrails

### Recovery Objectives (KSI-RPL-RRO)

- Define explicit RTO (Recovery Time Objective) and RPO (Recovery Point Objective) for every data store and service
- RTO/RPO must be documented in IaC or service configuration alongside the resource
- Recovery objectives must be reviewed and approved by stakeholders persistently

### Recovery Plan Alignment (KSI-RPL-ARP)

- Recovery procedures must be codified (runbooks-as-code or automated recovery scripts)
- Recovery plans must align with defined RTO/RPO targets
- Cross-region or cross-zone failover must be automated where RTO < 1 hour
- Recovery plans must cover: data corruption, region failure, credential compromise, ransomware

### Backup Alignment (KSI-RPL-ABO)

- Backup frequency must support the defined RPO (e.g., RPO=1h requires backups at least hourly)
- Backups must be encrypted at rest with keys separate from the backed-up system
- Backups must be stored in a different failure domain (region, account, provider)
- Backup retention aligned with compliance and recovery requirements
- Point-in-time recovery (PITR) enabled for databases where RPO < 24h
- Backup integrity validated automatically (periodic test restores)

### Testing Recovery (KSI-RPL-TRC)

- Automated recovery tests must run on a persistent schedule
- Test full restore from backup, not just backup creation
- Measure actual RTO/RPO against targets during tests
- Document test results and gaps
- Conduct tabletop exercises and game days for complex failure scenarios

## Compliance Review Checklist

- [ ] **KSI-RPL-RRO**: RTO/RPO defined for every data store and service; reviewed within last quarter
- [ ] **KSI-RPL-ARP**: Recovery plans exist and align with RTO/RPO; automated failover where RTO < 1h
- [ ] **KSI-RPL-ABO**: Backup frequency meets RPO; backups encrypted and cross-domain; integrity tested
- [ ] **KSI-RPL-TRC**: Recovery tests run on schedule; actual RTO/RPO measured against targets

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| No defined RTO/RPO | KSI-RPL-RRO | Define and document recovery objectives |
| Backup in same region/account as primary | KSI-RPL-ABO | Replicate to separate failure domain |
| Daily backup with RPO < 1 hour | KSI-RPL-ABO | Increase backup frequency or enable PITR |
| Unencrypted backups | KSI-RPL-ABO | Enable encryption; use separate key |
| No recovery testing | KSI-RPL-TRC | Schedule automated test restores |
| Manual failover for critical services | KSI-RPL-ARP | Automate failover with health-based triggers |
| Backups never test-restored | KSI-RPL-ABO | Add periodic restore-and-verify job |

## Documentation Generation

For KSI-CSX-SUM implementation summaries, see [patterns.md](patterns.md).

## Additional Resources

- For backup configuration patterns and recovery testing examples, see [patterns.md](patterns.md)
