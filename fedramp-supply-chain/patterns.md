# Supply Chain Risk Patterns

## Dependency Management (KSI-SCR-MIT)

### Lockfile Requirements by Ecosystem

```yaml
lockfile_policy:
  node:
    lockfile: "package-lock.json"
    install_command: "npm ci"  # Uses lockfile exactly, fails if mismatch
    integrity: "sha512 hashes in lockfile"
  python:
    lockfile: "requirements.txt with hashes OR poetry.lock"
    install_command: "pip install --require-hashes -r requirements.txt"
    integrity: "sha256 hashes per package"
  go:
    lockfile: "go.sum"
    install_command: "go mod download"
    integrity: "h1: hashes in go.sum"
  rust:
    lockfile: "Cargo.lock"
    install_command: "cargo install --locked"
    integrity: "checksum field in Cargo.lock"
  java:
    lockfile: "gradle.lockfile OR maven dependency-check"
    integrity: "SHA-256 verification plugin"
```

### Private Registry Configuration

```yaml
registry_policy:
  internal_mirror:
    url: "https://registry.internal.example.com"
    sync_from: ["https://registry.npmjs.org", "https://pypi.org"]
    sync_policy: "approved-only"
    vulnerability_gate: "block critical and high"
  container_registry:
    url: "https://containers.internal.example.com"
    allowed_base_images:
      - "registry.internal.example.com/base/alpine:*"
      - "registry.internal.example.com/base/distroless:*"
    signature_required: true
```

## SBOM Generation (KSI-SCR-MIT)

### Build Pipeline SBOM Step

```yaml
sbom_generation:
  format: "CycloneDX"  # or SPDX
  trigger: "every build"
  include:
    - application_dependencies
    - container_base_image_packages
    - os_packages
    - build_tools
  output:
    - artifact_registry  # stored alongside build artifact
    - vulnerability_scanner  # fed directly to scanner
  retention: "lifetime of deployed artifact + 1 year"
```

### SBOM Validation Checks

```
1. SBOM generated for every build artifact
2. SBOM includes all transitive dependencies
3. SBOM format validates against schema (CycloneDX/SPDX)
4. Every component in SBOM has version and license
5. SBOM stored in immutable artifact registry
6. SBOM available for customer/agency review on request
```

## Vulnerability Monitoring (KSI-SCR-MON)

### Scanning Pipeline

```yaml
vulnerability_scanning:
  stages:
    - name: pre_commit
      tool: "dependency-check-cli"
      scope: "direct dependencies"
      block_on: "critical"

    - name: ci_pipeline
      tool: "trivy, snyk, or grype"
      scope: "all dependencies + container image"
      block_on: "critical, high"
      generate: "vulnerability report"

    - name: continuous
      tool: "dependency monitoring service"
      scope: "all deployed artifacts"
      schedule: "daily"
      alert_on: "new CVE affecting any deployed dependency"

  sla:
    critical: "24 hours"
    high: "7 days"
    medium: "30 days"
    low: "90 days"

  exceptions:
    require: "documented risk acceptance with expiration date"
    max_duration: "90 days"
    review_cycle: "every renewal"
```

## KSI-CSX-SUM Templates for SCR

### KSI-SCR-MIT: Mitigating Supply Chain Risk

**1. Implementation Goals**
- Goal: All third-party dependencies pinned, verified, and inventoried via SBOM
- Pass criteria: 100% of dependencies have integrity verification; SBOM generated for every build
- Fail criteria: Any dependency without version pin or integrity hash; any build without SBOM
- Traceability: Maps to KSI-SCR-MIT

**2. Information Resources**
- Lockfiles for all application components
- SBOM artifacts for all builds
- Private registry configuration and approval list

**3. Machine-Based Validation**
- Process: CI enforces lockfile presence, integrity hashes, and SBOM generation
- Cycle: Persistent (every build)
- Tools: [Package manager lockfile enforcement, SBOM generator, registry policy engine]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of dependency inventory, new dependency approvals, and SBOM completeness
- Cycle: Quarterly
- Responsible party: Engineering and security teams

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any dependencies without integrity verification and mitigation plan]

---

### KSI-SCR-MON: Monitoring Supply Chain Risk

**1. Implementation Goals**
- Goal: Automated continuous monitoring of all dependencies for upstream vulnerabilities
- Pass criteria: All deployed artifacts scanned daily; CVE alerts within 1 hour of disclosure
- Fail criteria: Any deployed artifact not scanned within 24 hours; critical CVE without alert
- Traceability: Maps to KSI-SCR-MON

**2. Information Resources**
- Vulnerability scanner configuration and results
- SLA compliance reports
- Exception/risk acceptance records

**3. Machine-Based Validation**
- Process: Daily automated scan of all deployed artifacts; real-time CVE feed monitoring
- Cycle: Persistent (daily scan + real-time CVE alerts)
- Tools: [Vulnerability scanner, CVE feed monitor, SLA tracker]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of scan coverage, SLA compliance, and exception backlog
- Cycle: Quarterly
- Responsible party: Security team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any dependencies with known vulnerabilities under risk acceptance]
