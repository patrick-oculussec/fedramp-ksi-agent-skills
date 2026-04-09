# FedRAMP Monitoring, Logging, and Auditing

> **Scope:** Enforce FedRAMP 20x Monitoring, Logging, and Auditing KSIs in code and IaC. Covers structured logging, audit events, SIEM integration, log access controls, and IaC configuration evaluation. Apply when implementing logging, monitoring, alerting, audit trails, SIEM, observability, or log management.

KSI Theme: A secure cloud service offering will monitor, log, and audit all important events, activity, and changes.

## Code Generation Guardrails

### SIEM / Centralized Logging (KSI-MLA-OSM)

- All services must emit logs to a centralized, tamper-resistant log store
- Logs must be immutable once written (append-only, no deletion by application)
- Log storage must be in a separate security boundary from application infrastructure
- Retention policy: minimum 1 year for security-relevant logs
- Log transport must be encrypted and authenticated

### Log Content and Format (KSI-MLA-LET)

Every log entry must include as a minimum:
- Timestamp (ISO 8601, UTC)
- Event type / category
- Actor identity (user, service, or system)
- Action performed
- Resource affected
- Outcome (success/failure)
- Source IP / origin
- Correlation ID for request tracing

Event types that MUST be logged:
- Authentication events (login, logout, MFA challenge, failure)
- Authorization decisions (access granted, denied)
- Data access (read, create, update, delete of sensitive data)
- Configuration changes
- Administrative actions
- System lifecycle events (start, stop, deploy, scale)
- Security events (anomaly detected, rule triggered)

### Log Review (KSI-MLA-RVL)

- Configure automated alerting rules for critical security events
- Define alert escalation paths with response time SLAs
- Implement dashboards for security-relevant metrics
- Automated correlation of events across services

### Configuration Evaluation (KSI-MLA-EVC)

- Implement automated configuration scanning for all IaC
- Compare deployed configuration against declared baseline
- Alert on configuration drift from intended state
- Scan IaC templates for security misconfigurations before deployment

### Log Access Controls (KSI-MLA-ALA) -- Moderate

- Log data access requires authentication and authorization
- Least-privilege: analysts see only logs relevant to their scope
- Privileged log access (full access, deletion capability) requires JIT approval
- All log access must itself be logged for audit

## Compliance Review Checklist

- [ ] **KSI-MLA-OSM**: Centralized tamper-resistant log store; encrypted transport; separate security boundary
- [ ] **KSI-MLA-LET**: All required event types logged with mandatory fields
- [ ] **KSI-MLA-RVL**: Alert rules configured for security events; dashboards operational
- [ ] **KSI-MLA-EVC**: IaC scanned for misconfigurations; drift detection active
- [ ] **KSI-MLA-ALA**: Log access controlled by RBAC; privileged access requires JIT (Moderate)

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| Logging to local disk only | KSI-MLA-OSM | Ship to centralized log store |
| No correlation ID in logs | KSI-MLA-LET | Add request/trace ID to all log entries |
| Sensitive data in log messages (PII, secrets) | KSI-MLA-LET | Implement log scrubbing/redaction |
| Application can delete its own logs | KSI-MLA-OSM | Separate log store with append-only access |
| No alerts on authentication failures | KSI-MLA-RVL | Add alert rules for auth anomalies |
| IaC deployed without security scan | KSI-MLA-EVC | Add IaC scanning to CI pipeline |
| Everyone has full log access | KSI-MLA-ALA | Implement RBAC for log data |

## Documentation Generation

For KSI-CSX-SUM implementation summaries, see [patterns.md](patterns.md).

## Additional Resources

- For logging format examples and SIEM patterns, see [patterns.md](patterns.md)

---

*Copyright (c) 2026 Patrick Clark / Oculus Security, LLC. [MIT License](../LICENSE).*
