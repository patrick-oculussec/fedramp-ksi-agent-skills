# Change Management Patterns

## CI/CD Pipeline Pattern (KSI-CMT-VTD)

### Deployment Pipeline Stages

```yaml
pipeline:
  stages:
    - name: validate
      steps:
        - lint_code
        - lint_iac
        - run_unit_tests
        - sast_scan
        - dependency_vulnerability_scan
      gate: all_must_pass

    - name: build
      steps:
        - build_artifacts
        - build_container_images
        - sign_artifacts
        - push_to_registry
      gate: all_must_pass

    - name: deploy_staging
      steps:
        - iac_plan_diff
        - require_approval_if_destructive
        - apply_iac
        - deploy_application
      gate: all_must_pass

    - name: verify_staging
      steps:
        - health_check
        - smoke_tests
        - integration_tests
        - security_scan_deployed
      gate: all_must_pass
      on_failure: rollback_staging

    - name: deploy_production
      steps:
        - require_approval
        - iac_plan_diff
        - apply_iac
        - rolling_deploy_application
      gate: all_must_pass

    - name: verify_production
      steps:
        - health_check
        - smoke_tests
        - canary_metrics_check
      gate: all_must_pass
      on_failure: rollback_production

  audit:
    log_all_stages: true
    retain_artifacts_days: 365
    format: structured_json
```

## Immutable Deployment Patterns (KSI-CMT-RMV)

### Container Image Tagging

```yaml
image_tagging:
  # Immutable tags derived from source
  tag_format: "${git_sha_short}-${build_number}"
  # Example: "a1b2c3d-142"

  prohibited_tags:
    - "latest"
    - "stable"
    - "current"

  # Tag with semver for releases
  release_tag: "v${major}.${minor}.${patch}"

  # Never overwrite an existing tag
  overwrite_policy: "reject"
```

### Database Migration Pattern

```yaml
database_migrations:
  strategy: "forward-only"
  steps:
    - generate_migration_file_with_timestamp
    - review_migration_in_pr
    - test_migration_against_staging_snapshot
    - apply_migration_before_deploy
    - verify_schema_matches_expected
  rollback:
    method: "new-forward-migration"
    # Never use destructive rollback (DROP, ALTER to remove)
```

## Change Audit Record Format (KSI-CMT-LMC)

```json
{
  "change_id": "CHG-2026-001234",
  "timestamp": "2026-04-09T12:00:00Z",
  "actor": {
    "identity": "deploy-pipeline@ci.example.com",
    "triggered_by": "jane.doe@example.com",
    "approval_ref": "PR-4567"
  },
  "change": {
    "type": "deployment",
    "component": "api-service",
    "from_version": "a1b2c3d-141",
    "to_version": "d4e5f6g-142",
    "iac_diff_ref": "plan-output-142.json"
  },
  "validation": {
    "ci_passed": true,
    "tests_passed": true,
    "security_scan_passed": true,
    "smoke_test_passed": true
  },
  "result": "success"
}
```

## KSI-CSX-SUM Templates for CMT

### KSI-CMT-RMV: Redeploying vs Modifying

**1. Implementation Goals**
- Goal: All infrastructure changes executed through redeployment of immutable, version-controlled resources
- Pass criteria: Zero manual changes to production; all resources traceable to IaC commit
- Fail criteria: Any direct modification to running infrastructure outside the pipeline
- Traceability: Maps to KSI-CMT-RMV

**2. Information Resources**
- IaC repositories and their deployment configurations
- Container image registry and tag policies
- Pipeline definitions

**3. Machine-Based Validation**
- Process: Drift detection comparing running state to IaC; alert on any divergence
- Cycle: Persistent (hourly drift detection) + on every deployment
- Tools: [IaC drift detector, container registry policy enforcer]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of deployment logs for any out-of-band changes
- Cycle: Quarterly
- Responsible party: Platform engineering team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document emergency change procedure and how it maintains immutability]

---

### KSI-CMT-VTD: Validating Throughout Deployment

**1. Implementation Goals**
- Goal: Automated testing and validation at every stage of the deployment pipeline
- Pass criteria: Every deployment passes SAST, unit tests, integration tests, and post-deploy smoke tests
- Fail criteria: Any deployment reaching production without passing all validation stages
- Traceability: Maps to KSI-CMT-VTD

**2. Information Resources**
- CI/CD pipeline definitions
- Test suites and coverage reports
- Security scan configurations and results

**3. Machine-Based Validation**
- Process: Pipeline enforcement: deployments blocked if any stage fails; audit trail of all results
- Cycle: Persistent (every deployment)
- Tools: [CI/CD platform, test frameworks, security scanners]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of pipeline effectiveness, test coverage trends, and false positive rates
- Cycle: Quarterly
- Responsible party: Engineering and security teams

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any stages where manual approval supplements automated validation]
