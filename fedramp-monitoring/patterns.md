# Monitoring, Logging, and Auditing Patterns

## Structured Log Format (KSI-MLA-LET)

### Standard Log Entry Schema

```json
{
  "timestamp": "2026-04-09T14:23:01.456Z",
  "level": "INFO",
  "event_type": "auth.login.success",
  "correlation_id": "req-abc123-def456",
  "actor": {
    "type": "user",
    "id": "user-789",
    "ip": "203.0.113.42",
    "user_agent": "Mozilla/5.0..."
  },
  "action": "authenticate",
  "resource": {
    "type": "session",
    "id": "sess-xyz789"
  },
  "outcome": "success",
  "metadata": {
    "mfa_method": "webauthn",
    "idp": "okta"
  },
  "service": {
    "name": "auth-service",
    "version": "1.4.2",
    "environment": "production"
  }
}
```

### Required Event Types Catalog

```yaml
event_types:
  authentication:
    - auth.login.success
    - auth.login.failure
    - auth.logout
    - auth.mfa.challenge
    - auth.mfa.success
    - auth.mfa.failure
    - auth.token.issued
    - auth.token.revoked
    - auth.session.expired

  authorization:
    - authz.access.granted
    - authz.access.denied
    - authz.role.assigned
    - authz.role.revoked
    - authz.privilege.elevated
    - authz.privilege.expired

  data_access:
    - data.read
    - data.create
    - data.update
    - data.delete
    - data.export
    - data.bulk_operation

  configuration:
    - config.changed
    - config.deployed
    - config.drift.detected
    - config.rollback

  administrative:
    - admin.user.created
    - admin.user.disabled
    - admin.policy.changed
    - admin.secret.rotated
    - admin.certificate.renewed

  system:
    - system.started
    - system.stopped
    - system.deployed
    - system.scaled
    - system.health.degraded
    - system.health.recovered

  security:
    - security.anomaly.detected
    - security.rule.triggered
    - security.scan.completed
    - security.incident.created
    - security.vulnerability.found
```

## SIEM Integration Pattern (KSI-MLA-OSM)

### Centralized Log Architecture

```
Application Pods/VMs
    |
    v (structured JSON logs to stdout/stderr)
Log Collector (Fluentd, Vector, Filebeat)
    |
    v (encrypted transport, mTLS)
Log Aggregator / Message Queue
    |
    +---> SIEM (correlation, alerting, dashboards)
    +---> Long-term Storage (immutable, encrypted at rest)
    +---> Compliance Archive (retention >= 1 year)
```

### Log Pipeline Configuration

```yaml
log_pipeline:
  collection:
    method: "sidecar-or-daemonset"
    format: "json"
    buffer_size: "100MB"
    flush_interval: "5s"
  transport:
    protocol: "TLS"
    authentication: "mTLS"
    compression: "gzip"
  storage:
    primary: "siem-cluster"
    archive: "object-storage"
    retention:
      hot: "30 days"
      warm: "90 days"
      cold: "365 days"
    immutability: true
    encryption: "AES-256"
  access_control:
    admin: "jit-only"
    analyst: "read-own-scope"
    application: "write-only"
```

## Alert Rules (KSI-MLA-RVL)

### Critical Security Alerts

```yaml
alert_rules:
  - name: brute_force_detected
    condition: "auth.login.failure count > 10 in 5m for same actor.id"
    severity: high
    action: [notify_security, disable_account]

  - name: impossible_travel
    condition: "auth.login.success from locations > 500km apart in < 30m"
    severity: high
    action: [notify_security, require_mfa_reverification]

  - name: privilege_escalation
    condition: "authz.privilege.elevated without prior approval record"
    severity: critical
    action: [notify_security, revoke_privilege, create_incident]

  - name: config_drift
    condition: "config.drift.detected on any production resource"
    severity: medium
    action: [notify_platform, auto_remediate]

  - name: data_exfiltration
    condition: "data.export or data.bulk_operation volume > threshold"
    severity: high
    action: [notify_security, rate_limit]
```

## KSI-CSX-SUM Templates for MLA

### KSI-MLA-OSM: Operating SIEM Capability

**1. Implementation Goals**
- Goal: Centralized, tamper-resistant logging for all security-relevant events
- Pass criteria: All services ship logs to SIEM; log storage is append-only; retention >= 1 year
- Fail criteria: Any service not shipping logs; any log deletion capability from application context
- Traceability: Maps to KSI-MLA-OSM

**2. Information Resources**
- SIEM platform configuration
- Log pipeline configuration (collectors, transport, storage)
- Service inventory and their logging status

**3. Machine-Based Validation**
- Process: Monitor log ingestion rates; alert on log gaps > 5 minutes for any service
- Cycle: Persistent (real-time ingestion monitoring)
- Tools: [SIEM platform, log pipeline health monitor]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of SIEM coverage, retention compliance, and access controls
- Cycle: Quarterly
- Responsible party: Security operations team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document log retention exceptions and archival procedures]

---

### KSI-MLA-EVC: Evaluating Configurations

**1. Implementation Goals**
- Goal: Persistent automated evaluation of IaC and deployed configuration
- Pass criteria: All IaC scanned before deployment; drift detection active on all resources
- Fail criteria: Any IaC deployed without scan; drift undetected for > 1 hour
- Traceability: Maps to KSI-MLA-EVC

**2. Information Resources**
- IaC repositories and scan configurations
- Drift detection tool configuration
- Configuration baselines

**3. Machine-Based Validation**
- Process: IaC scanner in CI pipeline; runtime drift detector comparing state to IaC
- Cycle: Persistent (every commit + hourly drift scan)
- Tools: [IaC scanner, drift detector]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of scan rule coverage and drift remediation metrics
- Cycle: Quarterly
- Responsible party: Platform and security teams

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document known acceptable drift scenarios and their compensating controls]
