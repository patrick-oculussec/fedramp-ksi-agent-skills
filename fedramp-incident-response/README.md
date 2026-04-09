# FedRAMP Incident Response

> **Scope:** Enforce FedRAMP 20x Incident Response KSIs in code and IaC. Covers forensic logging, alerting pipelines, incident procedure documentation, post-incident analysis, and after-action report generation. Apply when implementing incident response, alerting, forensic logging, security event handling, or post-incident review processes.

KSI Theme: A secure cloud service offering will document, report, and analyze security incidents to ensure regulatory compliance and continuous security improvement.

## Code Generation Guardrails

### Reviewing Incident Response Procedures (KSI-INR-RIR)

- Incident response procedures must be version-controlled and reviewed persistently
- Runbooks should be codified as automated playbooks where possible
- Procedures must cover: detection, triage, containment, eradication, recovery, post-incident
- Integrate FedRAMP Incident Communications Procedures (ICP) into response workflow
- Define severity levels with corresponding response timelines and escalation paths

### Reviewing Past Incidents (KSI-INR-RPI)

- Implement automated pattern detection across incident history
- Track incident metrics: MTTD (mean time to detect), MTTR (mean time to respond/recover)
- Correlate incidents with root causes; track recurrence of similar incidents
- Feed incident findings into security improvement backlog

### After-Action Reports (KSI-INR-AAR)

- Generate structured after-action reports for every security incident
- Reports must include: timeline, root cause, impact, response actions, lessons learned
- Lessons learned must result in tracked remediation actions
- Share relevant findings with affected parties per FedRAMP ICP requirements

## Code and Infrastructure Requirements

### Forensic Readiness

When writing application or infrastructure code, enable forensic investigation:

- Log sufficient detail for reconstruction of security events (see [fedramp-monitoring](../fedramp-monitoring/README.md))
- Preserve log immutability: application must not be able to alter or delete its own logs
- Include correlation IDs that span request lifecycle across all services
- Timestamp all events in UTC with millisecond precision
- Retain forensic logs for minimum 1 year

### Alerting Pipeline

- Implement tiered alerting based on severity:
  - **P1 (Critical)**: Immediate page + auto-escalation after 15 minutes
  - **P2 (High)**: Page during business hours + escalation after 1 hour
  - **P3 (Medium)**: Ticket creation + SLA tracking
  - **P4 (Low)**: Logged for trend analysis
- Alert fatigue prevention: tune thresholds, deduplicate, suppress during maintenance windows
- Alert routing must have redundant delivery (primary + backup notification channel)

### Containment Automation

- Implement automated containment actions for well-defined attack patterns:
  - Account compromise: auto-disable + session revocation
  - Credential leak: auto-rotate affected credentials
  - DDoS: auto-engage DDoS mitigation
  - Data exfiltration: auto-block suspicious egress
- Manual approval gate for destructive containment actions

## Compliance Review Checklist

- [ ] **KSI-INR-RIR**: IR procedures documented, version-controlled, reviewed within last quarter
- [ ] **KSI-INR-RPI**: Past incidents reviewed for patterns; metrics tracked (MTTD, MTTR)
- [ ] **KSI-INR-AAR**: After-action reports generated for all incidents; lessons learned tracked
- [ ] Forensic logging sufficient for event reconstruction; immutable; 1-year retention
- [ ] Alerting pipeline configured with severity tiers and escalation paths
- [ ] Automated containment actions defined for common attack patterns

## Documentation Generation

For after-action report templates and KSI-CSX-SUM summaries, see [patterns.md](patterns.md).

## Additional Resources

- For incident response templates and playbook patterns, see [patterns.md](patterns.md)
- [FedRAMP Incident Communications Procedures](https://fedramp.gov/docs/20x/incident-communications-procedures)
- [NIST SP 800-61 Rev2: Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
