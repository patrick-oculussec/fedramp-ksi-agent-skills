# Cloud Native Architecture Patterns

## Network Policy Patterns (KSI-CNA-RNT, KSI-CNA-ULN)

### Default-Deny Network Policy

```yaml
# Kubernetes-style default deny all ingress and egress
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```

Then explicitly allow only required flows:

```yaml
# Allow specific service-to-service communication
spec:
  podSelector:
    matchLabels:
      app: api-server
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: web-frontend
      ports:
        - port: 8080
          protocol: TCP
  egress:
    - to:
        - podSelector:
            matchLabels:
              app: database
      ports:
        - port: 5432
          protocol: TCP
    - to:  # DNS resolution
        - namespaceSelector: {}
          podSelector:
            matchLabels:
              k8s-app: kube-dns
      ports:
        - port: 53
          protocol: UDP
```

### IaC Network Segmentation

```hcl
# Principle: three-tier network architecture
# Public subnet  -> load balancer only
# Private subnet -> application services
# Data subnet    -> databases, caches (no internet route)

network_tiers:
  public:
    cidr: "10.0.1.0/24"
    internet_access: true
    allowed_services: ["load-balancer"]
  private:
    cidr: "10.0.2.0/24"
    internet_access: "egress-only-via-nat"
    allowed_services: ["api", "worker"]
  data:
    cidr: "10.0.3.0/24"
    internet_access: false
    allowed_services: ["database", "cache"]
```

## Container Security Patterns (KSI-CNA-MAT, KSI-CNA-DFP)

### Hardened Container Spec

```yaml
containers:
  - name: app
    image: gcr.io/distroless/static:nonroot
    securityContext:
      runAsNonRoot: true
      runAsUser: 65534
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
    resources:
      requests:
        cpu: "100m"
        memory: "128Mi"
      limits:
        cpu: "500m"
        memory: "512Mi"
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /readyz
        port: 8080
      periodSeconds: 5
```

### Dockerfile Best Practices

```dockerfile
# Multi-stage build with minimal final image
FROM golang:1.22 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /app/server

FROM gcr.io/distroless/static:nonroot
COPY --from=builder /app/server /server
USER 65534:65534
ENTRYPOINT ["/server"]
```

## High Availability Patterns (KSI-CNA-OFA)

### Multi-AZ Deployment Configuration

```yaml
deployment:
  replicas: 3
  topology_spread:
    max_skew: 1
    topology_key: "topology.kubernetes.io/zone"
    when_unsatisfiable: "DoNotSchedule"
  pod_disruption_budget:
    min_available: 2
  strategy:
    type: RollingUpdate
    max_unavailable: 1
    max_surge: 1
```

## KSI-CSX-SUM Templates for CNA

### KSI-CNA-RNT: Restricting Network Traffic

**1. Implementation Goals**
- Goal: All machine-based resources have explicit inbound and outbound traffic restrictions
- Pass criteria: Zero resources with unrestricted ingress/egress; all flows documented
- Fail criteria: Any resource accepting traffic from 0.0.0.0/0 without justification
- Traceability: Maps to KSI-CNA-RNT

**2. Information Resources**
- Network policies, security groups, firewall rules
- Load balancer configurations
- Service mesh policies

**3. Machine-Based Validation**
- Process: IaC linter checks for open security groups; runtime scan for unexpected listeners
- Cycle: Persistent (every IaC commit) + daily runtime scan
- Tools: [IaC scanner, network policy validator]

**4. Non-Machine-Based Validation**
- Process: Quarterly network architecture review with documented flow diagrams
- Cycle: Quarterly
- Responsible party: Network security team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any justified exceptions, e.g., public load balancer on 443]

---

### KSI-CNA-MAT: Minimizing Attack Surface

**1. Implementation Goals**
- Goal: All resources run minimal software with no unnecessary capabilities
- Pass criteria: All containers use distroless/minimal images; no privileged containers; no root processes
- Fail criteria: Any container with privileged mode, root user, or full OS base image
- Traceability: Maps to KSI-CNA-MAT

**2. Information Resources**
- Container image manifests and Dockerfiles
- VM image build configurations
- Security context configurations

**3. Machine-Based Validation**
- Process: Image scanning for vulnerabilities and unnecessary packages; admission controller enforcing security contexts
- Cycle: Persistent (every build) + weekly full image scan
- Tools: [Container scanner, admission controller, image analysis tool]

**4. Non-Machine-Based Validation**
- Process: Quarterly review of base image selections and security context policies
- Cycle: Quarterly
- Responsible party: Platform engineering team

**5. Current Status**
- [ ] Implemented
- [ ] Validated
- [ ] Documented

**6. Clarifications**
[Document any containers requiring elevated privileges with compensating controls]
