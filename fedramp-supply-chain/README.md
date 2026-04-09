# FedRAMP Supply Chain Risk

> **Scope:** Enforce FedRAMP 20x Supply Chain Risk KSIs in code and IaC. Covers dependency scanning, SBOM generation, lockfile enforcement, upstream vulnerability monitoring, and third-party risk management. Apply when managing dependencies, third-party libraries, package managers, container base images, or supply chain security.

KSI Theme: A secure cloud service offering will understand, monitor, and manage supply chain risks from third-party information resources.

## Code Generation Guardrails

### Mitigating Supply Chain Risk (KSI-SCR-MIT)

- Pin all dependencies to exact versions in lockfiles (no floating ranges in production)
- Verify dependency integrity using checksums/hashes in lockfiles
- Review new dependencies before adding: check maintainer reputation, license, known vulnerabilities
- Minimize dependency count; prefer standard library solutions when feasible
- Use private registries/mirrors to control what packages are available
- Generate Software Bill of Materials (SBOM) for every build
- Scan container base images for vulnerabilities before use
- Prefer well-maintained, widely-used libraries over obscure alternatives

### Monitoring Supply Chain Risk (KSI-SCR-MON)

- Enable automated vulnerability scanning for all dependencies in CI/CD
- Subscribe to security advisories for critical dependencies
- Configure automated alerts when new CVEs affect dependencies
- Define SLAs for patching dependency vulnerabilities:
  - Critical: 24 hours
  - High: 7 days
  - Medium: 30 days
  - Low: 90 days
- Monitor for dependency confusion and typosquatting attacks
- Track dependency freshness: flag dependencies more than 2 major versions behind

## Compliance Review Checklist

- [ ] **KSI-SCR-MIT**: All dependencies pinned to exact versions with integrity hashes
- [ ] **KSI-SCR-MIT**: SBOM generated for every build artifact
- [ ] **KSI-SCR-MIT**: Private registry or mirror in use for dependency resolution
- [ ] **KSI-SCR-MON**: Automated vulnerability scanning in CI/CD pipeline
- [ ] **KSI-SCR-MON**: Security advisory subscriptions for critical dependencies
- [ ] **KSI-SCR-MON**: Patch SLAs defined and tracked

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| `"dependency": "^1.0.0"` (floating range) | KSI-SCR-MIT | Pin to exact version; use lockfile |
| No lockfile committed | KSI-SCR-MIT | Generate and commit lockfile |
| No dependency vulnerability scanning | KSI-SCR-MON | Add scanner to CI pipeline |
| Using `latest` tag for base images | KSI-SCR-MIT | Pin to digest or specific version |
| No SBOM generation | KSI-SCR-MIT | Add SBOM generation to build pipeline |
| Dependencies from untrusted registries | KSI-SCR-MIT | Use private registry/mirror |
| `npm install` without `--ignore-scripts` review | KSI-SCR-MIT | Audit install scripts of new deps |
| Dependency > 2 years without update | KSI-SCR-MON | Evaluate for replacement or update |

## Documentation Generation

For KSI-CSX-SUM implementation summaries, see [patterns.md](patterns.md).

## Additional Resources

- For SBOM generation patterns and scanning configurations, see [patterns.md](patterns.md)
- [CISA SBOM Guidance](https://www.cisa.gov/sbom)
- [NIST SP 800-161: Supply Chain Risk Management](https://csrc.nist.gov/publications/detail/sp/800-161/rev-1/final)

---

*Copyright (c) 2026 Patrick Clark / Oculus Security, LLC. [MIT License](../LICENSE).*
