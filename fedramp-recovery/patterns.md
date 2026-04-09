# Recovery Planning Patterns

## RTO/RPO Documentation Pattern (KSI-RPL-RRO)

### Service Recovery Matrix

```yaml
recovery_objectives:
  services:
    - name: "api-gateway"
      tier: "critical"
      rto: "15 minutes"
      rpo: "0 (no data loss)"
      strategy: "active-active multi-AZ"
      dependencies: ["auth-service", "database"]

    - name: "auth-service"
      tier: "critical"
      rto: "15 minutes"
      rpo: "0"
      strategy: "active-active multi-AZ"
      dependencies: ["identity-provider", "session-store"]

    - name: "database"
      tier: "critical"
      rto: "30 minutes"
      rpo: "5 minutes"
      strategy: "multi-AZ with read replicas + PITR"
      backup_frequency: "continuous (WAL shipping)"
      backup_retention: "30 days"

    - name: "analytics-pipeline"
      tier: "standard"
      rto: "4 hours"
      rpo: "1 hour"
      strategy: "single-region with cross-region backup"
      backup_frequency: "hourly snapshots"
      backup_retention: "90 days"

    - name: "reporting-service"
      tier: "low"
      rto: "24 hours"
      rpo: "24 hours"
      strategy: "daily backups with manual restore"
      backup_frequency: "daily"
      backup_retention: "365 days"
```

## Backup Configuration (KSI-RPL-ABO)

### IaC Backup Pattern

```yaml
backup_configuration:
  database:
    type: "continuous"
    method: "WAL shipping + periodic snapshots"
    snapshot_interval: "6 hours"
    pitr_enabled: true
    pitr_retention: "7 days"
    snapshot_retention: "30 days"
    encryption:
      enabled: true
      key_source: "separate-key-management-account"
    replication:
      target_region: "secondary-region"
      target_account: "backup-account"
    integrity_check:
      schedule: "weekly"
      method: "test-restore-and-query"

  object_storage:
    type: "versioned"
    versioning: true
    replication:
      target_region: "secondary-region"
    lifecycle:
      transition_to_cold: "90 days"
      expiration: "365 days"
    lock: "governance-mode"

  configuration:
    type: "version-controlled"
    method: "git repository"
    backup: "git mirror to secondary location"
```

## Recovery Testing (KSI-RPL-TRC)

### Automated Recovery Test Pipeline

```yaml
recovery_tests:
  schedule: "monthly"

  tests:
    - name: "database_restore_test"
      frequency: "monthly"
      steps:
        - create_test_environment
        - restore_latest_backup_to_test
        - run_data_integrity_checks
        - measure_restore_duration
        - compare_against_rto
        - cleanup_test_environment
      pass_criteria:
        - restore_duration < rto
        - data_integrity_check: pass
        - row_count_within_rpo_window: true

    - name: "service_failover_test"
      frequency: "quarterly"
      steps:
        - simulate_primary_az_failure
        - verify_automatic_failover
        - measure_failover_duration
        - verify_zero_data_loss
        - restore_primary_az
        - verify_failback
      pass_criteria:
        - failover_duration < rto
        - data_loss: 0
        - all_health_checks: pass

    - name: "full_region_recovery"
      frequency: "annually"
      steps:
        - simulate_region_unavailable
        - execute_region_recovery_runbook
        - restore_all_services_in_secondary
        - run_full_integration_tests
        - measure_total_recovery_time
      pass_criteria:
        - total_recovery_time < regional_rto
        - all_services_functional: true
        - data_within_rpo: true
```

## KSI-CSX-SUM Templates for RPL

### KSI-RPL-RRO: Reviewing Recovery Objectives

**1. Implementation Goals**
- Goal: RTO/RPO defined, documented, and reviewed for every service and data store
- Pass criteria: All services have documented RTO/RPO; objectives reviewed within last quarter
- Fail criteria: Any service without defined RTO/RPO; any objective not reviewed in > 6 months
- Traceability: Maps to KSI-RPL-RRO

**2. Information Resources**
- Service recovery matrix with RTO/RPO per service
- Stakeholder approval records

**3. Machine-Based Validation**
- Process: Parse IaC/config for RTO/RPO annotations; alert if any service lacks definition
- Cycle: Persistent (on every service deployment)
- Tools: [Service catalog validator, config parser]

**4. Non-Machine-Based Validation**
- Process: Quarterly stakeholder review of recovery objectives
- Cycle: Quarterly
- Responsible party: Engineering leads + business stakeholders

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any services where RTO/RPO are under negotiation]

---

### KSI-RPL-TRC: Testing Recovery Capabilities

**1. Implementation Goals**
- Goal: Persistent testing of recovery capabilities with measured results against RTO/RPO targets
- Pass criteria: All critical services tested monthly; all tests meet RTO/RPO targets
- Fail criteria: Any critical service untested for > 60 days; any test exceeding RTO/RPO
- Traceability: Maps to KSI-RPL-TRC

**2. Information Resources**
- Recovery test pipeline configurations
- Test result history and trend data
- Remediation records for failed tests

**3. Machine-Based Validation**
- Process: Automated recovery test pipeline with scheduled execution and result tracking
- Cycle: Monthly (critical services) / Quarterly (standard services) / Annually (full region)
- Tools: [Recovery test framework, result dashboard]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of test results, gap analysis, and improvement planning
- Cycle: Quarterly
- Responsible party: SRE and security teams

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any limitations in testing scope and compensating controls]
