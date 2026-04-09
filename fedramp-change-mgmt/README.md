# FedRAMP Change Management

> **Scope:** Enforce FedRAMP 20x Change Management KSIs in code and IaC. Covers immutable infrastructure, version-controlled deployments, CI/CD validation, change logging, and deployment testing. Apply when writing CI/CD pipelines, deployment configurations, infrastructure-as-code, GitOps workflows, or release automation.

KSI Theme: A secure cloud service provider will ensure that all changes are properly documented and configuration baselines are updated accordingly.

## Code Generation Guardrails

### Logging Changes (KSI-CMT-LMC)

- Every change to the cloud service must generate an audit record
- Audit records must include: who, what, when, where, and approval reference
- Use structured log format for machine-parseable change records
- Changes must be traceable from commit to deployment to running state
- Integrate change logging with SIEM (see [fedramp-monitoring](../fedramp-monitoring/README.md))

### Immutable Deployments (KSI-CMT-RMV)

- Deploy by replacing resources, not modifying them in place
- All infrastructure defined as version-controlled IaC
- Container images are built once, tagged immutably (no `latest` tag in production)
- VM/instance changes require new image build and redeployment
- Database schema changes through versioned migrations (forward-only)
- Configuration changes through new deployments, not runtime modification
- Prohibit SSH/remote access to production systems for making changes

### Automated Testing and Validation (KSI-CMT-VTD)

- CI pipeline must run before any deployment:
  - Static analysis (linting, SAST)
  - Unit and integration tests
  - IaC validation (plan/preview showing intended changes)
  - Security scanning (dependency vulnerabilities, container scanning)
- CD pipeline must validate after deployment:
  - Smoke tests against deployed environment
  - Health check verification
  - Automated rollback on failure
- Every stage must produce pass/fail artifacts for audit

### Reviewing Procedures (KSI-CMT-RVP)

- Change management procedures must be documented and version-controlled
- Pipeline definitions are subject to code review like application code
- Periodically test that rollback procedures work (game days)

## Compliance Review Checklist

- [ ] **KSI-CMT-LMC**: All changes produce structured audit records with who/what/when/approval
- [ ] **KSI-CMT-RMV**: Deployments replace resources; no in-place modification; no `latest` tags
- [ ] **KSI-CMT-VTD**: CI runs SAST, tests, IaC validation; CD runs smoke tests; rollback automated
- [ ] **KSI-CMT-RVP**: Change procedures documented; pipeline configs reviewed; rollback tested

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| `docker tag latest` in production | KSI-CMT-RMV | Use immutable tags (git SHA, semver) |
| SSH into prod to apply hotfix | KSI-CMT-RMV | Deploy through pipeline; emergency change via fast-track PR |
| No CI gate before deployment | KSI-CMT-VTD | Add required CI checks before merge/deploy |
| Deployment with no smoke tests | KSI-CMT-VTD | Add post-deploy verification with auto-rollback |
| Changes without audit trail | KSI-CMT-LMC | Integrate change logging into pipeline |
| Manual approval via Slack/email | KSI-CMT-LMC | Use auditable approval workflow in CI/CD system |
| Mutable infrastructure (config applied to running instances) | KSI-CMT-RMV | Rebuild and redeploy; use immutable AMIs/images |

## Documentation Generation

For KSI-CSX-SUM implementation summaries, see [patterns.md](patterns.md).

## Additional Resources

- For CI/CD pipeline patterns and configuration examples, see [patterns.md](patterns.md)
