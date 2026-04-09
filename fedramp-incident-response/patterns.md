# Incident Response Patterns

## After-Action Report Template (KSI-INR-AAR)

```markdown
# Incident After-Action Report

## Incident Summary
- **Incident ID**: [INC-YYYY-NNNN]
- **Severity**: [P1/P2/P3/P4]
- **Status**: [Open/Resolved/Closed]
- **Date Detected**: [ISO 8601]
- **Date Resolved**: [ISO 8601]
- **Duration**: [hours/minutes]
- **Affected Systems**: [list]
- **Impact**: [description of impact to users/data/availability]

## Timeline
| Time (UTC) | Event | Actor |
|---|---|---|
| YYYY-MM-DDTHH:MM:SSZ | [event description] | [person/system] |

## Detection
- **How detected**: [monitoring alert / user report / scan finding / external notification]
- **Time to detect (MTTD)**: [duration from incident start to detection]
- **Detection gaps**: [anything that delayed detection]

## Root Cause
- **Primary cause**: [technical root cause]
- **Contributing factors**: [process, people, or technology factors]
- **Category**: [credential compromise / misconfiguration / vulnerability exploitation / etc.]

## Response Actions
1. [Containment action taken and timestamp]
2. [Eradication action taken and timestamp]
3. [Recovery action taken and timestamp]

## Impact Assessment
- **Data affected**: [type and volume of data, if any]
- **Users affected**: [number and type]
- **Service impact**: [downtime, degradation, or none]
- **Regulatory impact**: [notification requirements triggered]

## Lessons Learned
### What went well
- [positive observations]

### What needs improvement
- [gaps identified]

### Action Items
| ID | Action | Owner | Due Date | Status |
|---|---|---|---|---|
| AI-001 | [remediation action] | [owner] | [date] | [status] |

## FedRAMP Notification
- **Notified**: [Yes/No, per ICP requirements]
- **Notification date**: [if applicable]
- **Reference**: [ICP notification ID]
```

## Incident Response Playbook Pattern (KSI-INR-RIR)

### Automated Playbook Structure

```yaml
playbook:
  name: "credential-compromise"
  trigger:
    alert_type: "security.credential.compromised"
    auto_execute: true

  steps:
    - name: "identify_scope"
      action: "query_siem"
      query: "actor.id = '{compromised_identity}' AND timestamp > '{compromise_window}'"
      output: "affected_sessions"

    - name: "contain"
      actions:
        - disable_account: "{compromised_identity}"
        - revoke_sessions: "{affected_sessions}"
        - rotate_credentials: "{compromised_identity}"
      require_approval: false  # auto-execute for containment

    - name: "investigate"
      actions:
        - export_logs: "{compromised_identity}"
        - check_data_access: "{compromised_identity}"
        - identify_lateral_movement: "{affected_sessions}"
      output: "investigation_report"

    - name: "notify"
      condition: "investigation_report.data_accessed == true"
      actions:
        - notify_security_team: "P1"
        - create_incident_ticket: "credential-compromise"
        - initiate_icp_notification: "if_federal_data_affected"

    - name: "recover"
      actions:
        - issue_new_credentials: "{compromised_identity}"
        - verify_no_persistence: "{affected_systems}"
        - re_enable_account: "{compromised_identity}"
      require_approval: true  # manual approval for recovery
```

## Incident Metrics Tracking (KSI-INR-RPI)

```yaml
incident_metrics:
  tracked:
    - mttd: "Mean Time to Detect"
    - mttr: "Mean Time to Respond"
    - mttc: "Mean Time to Contain"
    - mttre: "Mean Time to Recover"
    - incident_count_by_severity: "Monthly count per P1/P2/P3/P4"
    - recurrence_rate: "% incidents with same root cause in 90 days"
    - action_item_completion: "% of post-incident actions completed on time"

  review_cadence: "quarterly"
  trend_analysis: "quarter-over-quarter comparison"
  reporting: "included in Quarterly Review per KSI-AFR-CCM"
```

## KSI-CSX-SUM Templates for INR

### KSI-INR-RIR: Reviewing Incident Response Procedures

**1. Implementation Goals**
- Goal: Incident response procedures documented, automated where possible, and reviewed persistently
- Pass criteria: Procedures cover all phases; runbooks exist for top incident categories; reviewed quarterly
- Fail criteria: Procedures not reviewed in > 6 months; major incident category without runbook
- Traceability: Maps to KSI-INR-RIR

**2. Information Resources**
- Incident response plan document
- Automated playbook configurations
- Tabletop exercise results

**3. Machine-Based Validation**
- Process: Automated playbook testing (dry-run mode) validates execution paths
- Cycle: Monthly (dry-run tests)
- Tools: [Playbook automation platform, SOAR system]

**4. Non-Machine-Based Validation**
- Process: Quarterly tabletop exercise; annual full-scale exercise
- Cycle: Quarterly / Annually
- Responsible party: Security operations and incident response team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any incident categories handled by third-party managed services]

---

### KSI-INR-AAR: Generating After-Action Reports

**1. Implementation Goals**
- Goal: Structured after-action report for every security incident with tracked lessons learned
- Pass criteria: 100% of P1/P2 incidents have AAR within 5 business days; action items tracked
- Fail criteria: Any P1/P2 incident without AAR; action items not tracked to completion
- Traceability: Maps to KSI-INR-AAR

**2. Information Resources**
- After-action report repository
- Action item tracking system
- Incident metrics dashboard

**3. Machine-Based Validation**
- Process: Ticket system enforces AAR creation for P1/P2 incidents; tracks action item completion
- Cycle: Persistent (triggered per incident)
- Tools: [Incident management platform, project tracker]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of AAR quality, action item completion rates, and pattern trends
- Cycle: Quarterly
- Responsible party: Security operations manager

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document AAR sharing procedures per FedRAMP ICP requirements]
