# FedRAMP Cloud Native Architecture

> **Scope:** Enforce FedRAMP 20x Cloud Native Architecture KSIs in code and IaC. Covers network segmentation, attack surface minimization, logical networking, high availability, immutable infrastructure, and DoS protection. Apply when writing infrastructure-as-code, container configurations, network policies, load balancer configs, or cloud architecture definitions.

KSI Theme: A secure cloud service offering will use cloud native architecture and design principles to enforce and enhance the confidentiality, integrity and availability of the system.

## Code Generation Guardrails

### Network Traffic Restriction (KSI-CNA-RNT)

- Default-deny network policies: block all traffic, then allow only required flows
- Every resource must have explicit inbound AND outbound rules
- No `0.0.0.0/0` ingress except for public-facing load balancers
- Egress must be restricted to known-good destinations
- Document every allowed network flow with justification

### Minimal Attack Surface (KSI-CNA-MAT)

- Containers: use minimal base images (distroless, scratch, Alpine); no shell in production
- VMs: remove unnecessary packages, disable unused services and ports
- Disable debug endpoints, admin consoles, and default accounts in production
- Enforce read-only root filesystems for containers
- No privileged containers; drop all capabilities, add back only what's required
- Network segmentation must prevent lateral movement between tiers

### Logical Networking (KSI-CNA-ULN)

- Separate workloads into distinct network segments (VPCs, subnets, namespaces)
- Use service mesh or network policies to enforce east-west traffic controls
- Isolate data stores in private subnets with no direct internet access
- Management/admin traffic on separate network segment from application traffic

### Functionality and Privileges (KSI-CNA-DFP)

- Every service must have a documented purpose and allowed capabilities
- Infrastructure manifests must declare explicit resource limits (CPU, memory, storage)
- No catch-all service definitions; each component scoped to its function
- Container security contexts must be explicit (runAsNonRoot, readOnlyRootFilesystem)

### DoS Protection (KSI-CNA-RVP)

- Deploy rate limiting at ingress points
- Configure auto-scaling with defined upper bounds
- Use CDN/WAF/DDoS protection for public-facing services
- Implement circuit breakers for downstream service calls
- Set request size limits and timeouts on all endpoints

### High Availability (KSI-CNA-OFA)

- Deploy across multiple availability zones minimum
- Implement health checks (liveness, readiness) on all services
- Configure appropriate replica counts (minimum 2 for stateless services)
- Use rolling deployments with rollback capabilities
- Design for graceful degradation under partial failure

### Provider Best Practices (KSI-CNA-IBP)

- Follow cloud provider hardening guides for all managed services
- Use provider-recommended instance types and configurations
- Enable provider security features (guard duty, security center, etc.)
- Stay current with provider security advisories

### Enforcing Intended State (KSI-CNA-EIS) -- Moderate

- Use declarative IaC (Terraform, CloudFormation, Pulumi) as single source of truth
- Deploy drift detection to alert on unauthorized changes
- Auto-remediate configuration drift back to declared state
- Infrastructure changes only through IaC pipeline, never manual

## Compliance Review Checklist

- [ ] **KSI-CNA-RNT**: All resources have explicit inbound/outbound rules; no open `0.0.0.0/0`
- [ ] **KSI-CNA-MAT**: Minimal base images; no privileged containers; no unnecessary services
- [ ] **KSI-CNA-ULN**: Workloads segmented into separate network zones; east-west traffic controlled
- [ ] **KSI-CNA-DFP**: All services have defined functionality, resource limits, and security contexts
- [ ] **KSI-CNA-RVP**: Rate limiting, WAF, and DDoS protection configured for public endpoints
- [ ] **KSI-CNA-OFA**: Multi-AZ deployment; health checks; minimum replica counts set
- [ ] **KSI-CNA-IBP**: Provider best practices followed; hardening guides applied
- [ ] **KSI-CNA-EIS**: IaC drift detection enabled; no manual infrastructure changes (Moderate)

### Anti-Patterns to Flag

| Anti-Pattern | KSI Violated | Remediation |
|---|---|---|
| Security group with `0.0.0.0/0` ingress on non-443 port | KSI-CNA-RNT | Restrict to known CIDR ranges |
| Container running as root | KSI-CNA-MAT | Set `runAsNonRoot: true` in security context |
| All services in single flat network | KSI-CNA-ULN | Segment into tiers with network policies |
| No resource limits on containers | KSI-CNA-DFP | Add CPU/memory requests and limits |
| Public endpoint without rate limiting | KSI-CNA-RVP | Add WAF/rate limiting at ingress |
| Single-AZ deployment | KSI-CNA-OFA | Deploy across 2+ availability zones |
| Manual infrastructure changes alongside IaC | KSI-CNA-EIS | Enforce IaC-only changes; add drift detection |
| Full OS base image (ubuntu, debian) | KSI-CNA-MAT | Switch to distroless or Alpine |

## Documentation Generation

For KSI-CSX-SUM implementation summaries, see [patterns.md](patterns.md) for CNA-specific templates.

## Additional Resources

- For detailed IaC patterns and examples, see [patterns.md](patterns.md)

---

*Copyright (c) 2026 Patrick Clark / Oculus Security, LLC. [MIT License](../LICENSE).*
