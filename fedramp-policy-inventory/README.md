# FedRAMP Policy and Inventory

> **Scope:** Enforce FedRAMP 20x Policy and Inventory KSIs. Covers automated asset inventory generation, SDLC security integration, vulnerability disclosure programs, and security investment reviews. Apply when implementing asset inventory automation, integrating security into SDLC, creating vulnerability disclosure policies, or reviewing security posture.

KSI Theme: A secure cloud service offering will have intentional, organized, universal guidance for how every information resource, including personnel, is secured.

## Code Generation Guardrails

### Generating Inventories (KSI-PIY-GIV)

- Implement automated inventory generation from authoritative sources (cloud APIs, service mesh, container orchestrator)
- Inventory must be real-time or near-real-time, not manually maintained
- Every information resource must be captured: compute, storage, network, services, databases, third-party integrations
- Tag all resources with: owner, classification, environment, service, cost center
- Inventory must reconcile against IaC: any resource not in IaC is flagged

### Security in the SDLC (KSI-PIY-RSD)

When generating code or setting up development workflows:
- Threat modeling integrated into design phase for new features
- Security requirements defined alongside functional requirements
- SAST/DAST/SCA scanning integrated into CI pipeline (see [fedramp-monitoring](../fedramp-monitoring/README.md), [fedramp-supply-chain](../fedramp-supply-chain/README.md))
- Security review gate before production deployment
- Align with CISA Secure By Design principles:
  - Secure defaults
  - Elimination of default passwords
  - Transparency in vulnerability disclosure
  - Memory-safe languages where feasible for new development

### Vulnerability Disclosure (KSI-PIY-RVD)

- Publish a vulnerability disclosure policy (VDP) or bug bounty program
- Provide a clear reporting channel (security.txt, dedicated email)
- Define response SLAs for vulnerability reports
- Track and remediate reported vulnerabilities with the same SLAs as internally discovered ones

### Security Investment Review (KSI-PIY-RIS)

- Track security tooling, training, and personnel investments
- Measure effectiveness: reduced vulnerability counts, faster response times, improved posture scores
- Review ROI quarterly

### Executive Support (KSI-PIY-RES)

- Security objectives approved at executive level
- Security metrics reported to leadership regularly
- Resource allocation aligned with risk priorities

## Compliance Review Checklist

- [ ] **KSI-PIY-GIV**: Automated real-time inventory from authoritative sources; reconciles against IaC
- [ ] **KSI-PIY-RSD**: Security integrated into SDLC; SAST/DAST in CI; threat modeling for new features
- [ ] **KSI-PIY-RVD**: Vulnerability disclosure policy published; reporting channel operational; SLAs defined
- [ ] **KSI-PIY-RIS**: Security investments tracked; effectiveness measured; reviewed quarterly
- [ ] **KSI-PIY-RES**: Executive support documented; security metrics reported to leadership

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| Manually maintained asset spreadsheet | KSI-PIY-GIV | Automate inventory from cloud APIs |
| Resources without owner/classification tags | KSI-PIY-GIV | Enforce tagging policy in IaC |
| No security review in deployment pipeline | KSI-PIY-RSD | Add security gates to CI/CD |
| No vulnerability disclosure policy | KSI-PIY-RVD | Publish VDP and security.txt |
| No threat modeling for new features | KSI-PIY-RSD | Integrate threat modeling into design |
| Security not represented in sprint planning | KSI-PIY-RSD | Include security tasks in backlog |

## Documentation Generation

For policy templates and KSI-CSX-SUM summaries, see [templates.md](templates.md).

## Additional Resources

- For inventory automation patterns and policy templates, see [templates.md](templates.md)
- [CISA Secure By Design](https://www.cisa.gov/securebydesign)
- [ISO 27001 Asset Management](https://www.iso.org/standard/27001)
