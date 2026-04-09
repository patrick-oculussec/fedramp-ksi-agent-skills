# Policy and Inventory Templates

## Automated Inventory Pattern (KSI-PIY-GIV)

### Inventory Schema

```yaml
inventory_resource:
  id: "unique-resource-identifier"
  type: "compute | storage | network | database | service | integration"
  name: "human-readable-name"
  provider: "aws | azure | gcp | on-prem | saas"
  region: "us-east-1"
  environment: "production | staging | development"
  owner: "team-name"
  classification: "public | internal | confidential | restricted"
  service: "service-name-this-belongs-to"
  iac_reference: "path/to/terraform/resource.tf:line"
  discovered_at: "2026-04-09T12:00:00Z"
  last_seen: "2026-04-09T14:00:00Z"
  tags:
    cost_center: "engineering"
    compliance_scope: "fedramp"
```

### Inventory Reconciliation Pipeline

```yaml
inventory_reconciliation:
  schedule: "hourly"
  steps:
    - name: "discover"
      action: "query cloud provider APIs for all resources"
      output: "discovered_resources"

    - name: "reconcile_against_iac"
      action: "compare discovered resources against IaC state"
      flag:
        - unmanaged_resources: "resource exists but not in IaC"
        - missing_resources: "resource in IaC but not discovered"
        - drift: "resource state differs from IaC declaration"

    - name: "check_tagging"
      action: "verify all resources have required tags"
      required_tags: ["owner", "environment", "classification", "service"]
      flag: "resources missing required tags"

    - name: "report"
      action: "generate inventory report and alert on anomalies"
      alerts:
        - unmanaged_resources: "notify platform team"
        - missing_tags: "notify resource owner"
        - new_resources: "log for audit"
```

## Vulnerability Disclosure Policy Template (KSI-PIY-RVD)

```markdown
# [Organization] Vulnerability Disclosure Policy

## Scope
This policy covers vulnerabilities in [CSO Name] and related services.

## Reporting
Report vulnerabilities to: security@example.com
Or use our security.txt: https://example.com/.well-known/security.txt

## What We Ask
- Provide detailed reproduction steps
- Do not access, modify, or delete data belonging to others
- Do not disclose the vulnerability publicly until we have addressed it
- Act in good faith

## What We Commit To
- **Acknowledgment**: Within 2 business days
- **Triage**: Within 5 business days
- **Status update**: Within 15 business days
- **Resolution timeline**: Based on severity
  - Critical: 72 hours
  - High: 7 days
  - Medium: 30 days
  - Low: 90 days

## Safe Harbor
We will not pursue legal action against researchers who:
- Follow this policy
- Act in good faith
- Report findings responsibly

## Recognition
We recognize researchers who report valid vulnerabilities [in our hall of fame / with bounties].
```

## security.txt Template

```
# https://example.com/.well-known/security.txt
Contact: mailto:security@example.com
Expires: 2027-04-09T00:00:00.000Z
Encryption: https://example.com/.well-known/pgp-key.txt
Preferred-Languages: en
Canonical: https://example.com/.well-known/security.txt
Policy: https://example.com/security/vulnerability-disclosure-policy
```

## SDLC Security Integration Checklist (KSI-PIY-RSD)

```yaml
sdlc_security:
  design:
    - threat_model_for_new_features
    - security_requirements_in_user_stories
    - architecture_security_review_for_significant_changes

  development:
    - secure_coding_standards_followed
    - pre_commit_hooks_for_secrets_detection
    - ide_security_plugins_recommended

  build:
    - sast_scan_in_ci
    - dependency_vulnerability_scan
    - container_image_scan
    - iac_security_scan
    - sbom_generation

  test:
    - security_focused_unit_tests
    - dast_scan_on_staging
    - penetration_test_schedule

  deploy:
    - security_gate_approval
    - signed_artifacts_only
    - immutable_deployment

  operate:
    - runtime_monitoring
    - vulnerability_scanning_continuous
    - incident_response_integration

  review:
    - quarterly_sdlc_effectiveness_review
    - vulnerability_trend_analysis
    - process_improvement_actions
```

## KSI-CSX-SUM Templates for PIY

### KSI-PIY-GIV: Generating Inventories

**1. Implementation Goals**
- Goal: Real-time automated inventory of all information resources from authoritative sources
- Pass criteria: 100% of resources inventoried; zero unmanaged resources; all resources tagged
- Fail criteria: Any resource not in inventory; any resource missing required tags
- Traceability: Maps to KSI-PIY-GIV

**2. Information Resources**
- Cloud provider resource APIs
- IaC state files
- Service mesh / container orchestrator APIs

**3. Machine-Based Validation**
- Process: Hourly reconciliation of discovered resources against IaC; alert on anomalies
- Cycle: Persistent (hourly discovery + reconciliation)
- Tools: [Cloud inventory tool, IaC state comparator, tagging policy enforcer]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of inventory completeness and tagging compliance
- Cycle: Quarterly
- Responsible party: Platform engineering and security teams

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any resource types not discoverable via API and how they are tracked]

---

### KSI-PIY-RSD: Reviewing Security in the SDLC

**1. Implementation Goals**
- Goal: Security and privacy integrated into every phase of the SDLC per Secure By Design principles
- Pass criteria: All phases have security activities; SAST/DAST in CI; threat modeling for new features
- Fail criteria: Any SDLC phase without security integration; code deployed without security scan
- Traceability: Maps to KSI-PIY-RSD

**2. Information Resources**
- SDLC documentation with security activities per phase
- CI/CD pipeline configurations showing security gates
- Threat model repository

**3. Machine-Based Validation**
- Process: CI pipeline enforces security scan completion; merge blocked without passing scans
- Cycle: Persistent (every merge request)
- Tools: [CI platform, SAST/DAST/SCA scanners, merge gate policies]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of SDLC security effectiveness, vulnerability trends, and process gaps
- Cycle: Quarterly
- Responsible party: Engineering management and security team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any SDLC exceptions for emergency changes and their compensating controls]
