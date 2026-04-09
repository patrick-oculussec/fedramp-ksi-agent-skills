# Cybersecurity Education Templates

## Training Program Documentation Template

```markdown
# [Organization] Cybersecurity Training Program

## Program Overview
- **Program owner**: [Name/Role]
- **Last reviewed**: [Date]
- **Next review**: [Date]

## Training Tracks

### Track 1: General Security Awareness (All Employees)
**KSI Reference**: KSI-CED-RGT

| Module | Topics | Duration | Frequency | Required |
|---|---|---|---|---|
| Security Fundamentals | Data classification, acceptable use, clean desk | 30 min | Annual | Yes |
| Phishing Awareness | Email phishing, social engineering, reporting | 20 min | Quarterly | Yes |
| Incident Reporting | How to report, what to report, who to contact | 15 min | Annual | Yes |
| Data Handling | Sensitive data procedures, disposal, sharing | 20 min | Annual | Yes |

**Effectiveness Metrics**:
- Training completion rate (target: 100% within 30 days)
- Phishing simulation click rate (target: < 5%)
- Incident self-reporting rate (target: increasing trend)

### Track 2: High-Risk Role Training (Privileged Users)
**KSI Reference**: KSI-CED-RST

| Module | Topics | Duration | Frequency | Required |
|---|---|---|---|---|
| Privileged Access Security | Credential management, JIT access, audit trails | 45 min | Quarterly | Yes |
| Advanced Social Engineering | Targeted attacks, pretexting, vishing | 30 min | Semi-annual | Yes |
| Insider Threat Awareness | Detection, prevention, reporting | 30 min | Annual | Yes |

**Effectiveness Metrics**:
- Security assessment scores (target: > 90%)
- Privileged access policy compliance rate (target: 100%)

### Track 3: Development and Engineering
**KSI Reference**: KSI-CED-DET

| Module | Topics | Duration | Frequency | Required |
|---|---|---|---|---|
| OWASP Top 10 | Injection, XSS, auth failures, SSRF, etc. | 60 min | Annual | Yes |
| Secure Code Review | What to look for, common vulnerabilities | 45 min | Semi-annual | Yes |
| SAST/DAST Practices | Tool usage, interpreting results, remediation | 30 min | Annual | Yes |
| Secure By Design | CISA principles, threat modeling, defense in depth | 45 min | Annual | Yes |
| Hands-On Lab | CTF, secure code review exercises | 120 min | Quarterly | Recommended |

**Effectiveness Metrics**:
- Vulnerability introduction rate in code (target: decreasing trend)
- Security finding remediation time (target: decreasing trend)
- Code review security comment quality (target: increasing trend)

### Track 4: Incident Response and Recovery
**KSI Reference**: KSI-CED-RRT

| Module | Topics | Duration | Frequency | Required |
|---|---|---|---|---|
| IR Procedures | Detection, triage, containment, recovery | 60 min | Annual | Yes |
| Communication Protocols | Internal escalation, FedRAMP ICP, agency notification | 30 min | Annual | Yes |
| Tabletop Exercise | Scenario-based IR walkthrough | 120 min | Quarterly | Yes |
| Recovery Procedures | Backup restore, failover, service restoration | 45 min | Semi-annual | Yes |

**Effectiveness Metrics**:
- Tabletop exercise performance scores (target: improving trend)
- Actual incident response time vs. targets (target: meeting MTTD/MTTR goals)
- Post-exercise action item completion rate (target: 100%)

## Compliance Tracking

| Employee | Role | General | Role-Specific | Dev/Eng | IR/DR | Last Completed |
|---|---|---|---|---|---|---|
| [Name] | [Role] | [date] | [date/N/A] | [date/N/A] | [date/N/A] | [date] |
```

## Training Effectiveness Review Template

```markdown
# Quarterly Training Effectiveness Review

**Period**: [Q1/Q2/Q3/Q4 YYYY]
**Reviewer**: [Name/Role]
**Date**: [Date]

## Completion Metrics
| Track | Enrolled | Completed | Rate | Target | Status |
|---|---|---|---|---|---|
| General Awareness | [n] | [n] | [%] | 100% | [Met/Not Met] |
| High-Risk Roles | [n] | [n] | [%] | 100% | [Met/Not Met] |
| Dev/Engineering | [n] | [n] | [%] | 100% | [Met/Not Met] |
| IR/DR | [n] | [n] | [%] | 100% | [Met/Not Met] |

## Effectiveness Metrics
- Phishing simulation click rate: [%] (target: < 5%)
- Security assessment avg score: [%] (target: > 90%)
- Vulnerability introduction trend: [up/down/flat]
- Tabletop exercise score: [score] (target: improving)

## Findings
1. [Finding and impact]
2. [Finding and impact]

## Recommendations
1. [Action item with owner and timeline]
2. [Action item with owner and timeline]

## Decision
- [ ] Training content adequate for next quarter
- [ ] Training content needs updates (see recommendations)
```

## KSI-CSX-SUM Templates for CED

### KSI-CED-DET: Reviewing Development and Engineering Training

**1. Implementation Goals**
- Goal: All development and engineering staff complete secure coding training aligned with Secure By Design
- Pass criteria: 100% completion; decreasing vulnerability introduction rate; quarterly hands-on exercises
- Fail criteria: Any developer without current training; increasing vulnerability introduction rate
- Traceability: Maps to KSI-CED-DET

**2. Information Resources**
- Training completion records for all dev/eng staff
- Vulnerability trend data from SAST/DAST
- Code review metrics

**3. Machine-Based Validation**
- Process: LMS tracks completion and scores; CI pipeline tracks vulnerability introduction rates
- Cycle: Persistent (LMS tracking) + quarterly analysis
- Tools: [LMS platform, CI security scan trend analyzer]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of training content relevance and effectiveness metrics
- Cycle: Quarterly
- Responsible party: Engineering management + security team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any contractors or external developers and their training requirements]
