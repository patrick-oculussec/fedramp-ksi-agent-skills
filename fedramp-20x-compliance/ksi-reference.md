# FedRAMP 20x KSI Complete Reference

All Key Security Indicators from FedRAMP 20x v0.9.0-beta, organized by theme.

## Cross-Theme Requirements

| ID | Requirement |
|---|---|
| KSI-CSX-SUM | Maintain implementation summaries for each KSI (goals, resources, validation, status) |
| KSI-CSX-MAS | Apply ALL KSIs to ALL aspects within FedRAMP Minimum Assessment Scope |
| KSI-CSX-ORD | Recommended order of criticality: MAS, ADS, UCM, VDR, SCN, PVA, RSC, CCM, FSI, ICP |

---

## Authorization by FedRAMP (AFR)

| ID | Requirement |
|---|---|
| KSI-AFR-MAS | Apply FedRAMP Minimum Assessment Scope to identify and document the CSO scope |
| KSI-AFR-ADS | Determine how authorization data will be shared with all necessary parties |
| KSI-AFR-UCM | Ensure cryptographic modules protecting federal data are FIPS 140-validated |
| KSI-AFR-VDR | Document vulnerability detection and response methodology |
| KSI-AFR-SCN | Determine how significant changes are tracked and parties notified |
| KSI-AFR-CCM | Maintain plan for Ongoing Authorization Reports and Quarterly Reviews |
| KSI-AFR-SCG | Develop secure-by-default configurations and provide guidance to customers |
| KSI-AFR-FSI | Operate a secure inbox for critical government communication |
| KSI-AFR-PVA | Persistently validate, assess, and report on security effectiveness |
| KSI-AFR-ICP | Integrate FedRAMP Incident Communications Procedures into incident response |

## Change Management (CMT)

| ID | Requirement |
|---|---|
| KSI-CMT-LMC | Log and monitor modifications to the cloud service offering |
| KSI-CMT-RMV | Execute changes through redeployment of version-controlled immutable resources |
| KSI-CMT-VTD | Automate persistent testing and validation of changes throughout deployment |
| KSI-CMT-RVP | Persistently review effectiveness of change management procedures |

## Cloud Native Architecture (CNA)

| ID | Requirement |
|---|---|
| KSI-CNA-RNT | Ensure all resources are configured to limit inbound and outbound network traffic |
| KSI-CNA-MAT | Ensure resources have a minimal attack surface; minimize lateral movement |
| KSI-CNA-ULN | Use logical networking to enforce traffic flow controls |
| KSI-CNA-DFP | Strictly define functionality and privileges for infrastructure and services |
| KSI-CNA-RVP | Review effectiveness of DoS protection and unwanted activity defenses |
| KSI-CNA-OFA | Optimize resources for high availability and rapid recovery |
| KSI-CNA-IBP | Ensure cloud-native resources follow host provider best practices |
| KSI-CNA-EIS | Use automated services to assess security posture and enforce intended state (Moderate) |

## Cybersecurity Education (CED)

| ID | Requirement |
|---|---|
| KSI-CED-RGT | Review effectiveness of general employee security training |
| KSI-CED-RST | Review effectiveness of role-specific training for high-risk roles |
| KSI-CED-DET | Review effectiveness of development/engineering secure software training |
| KSI-CED-RRT | Review effectiveness of incident response/disaster recovery training |

## Identity and Access Management (IAM)

| ID | Requirement |
|---|---|
| KSI-IAM-MFA | Enforce phishing-resistant MFA for all user authentication |
| KSI-IAM-APM | Use secure passwordless methods when feasible; otherwise strong passwords + MFA |
| KSI-IAM-SNU | Enforce secure authentication for non-user accounts and services |
| KSI-IAM-JIT | Use least-privileged, role/attribute-based, just-in-time authorization |
| KSI-IAM-ELP | Ensure each user or device can only access resources they need |
| KSI-IAM-SUS | Automatically disable or secure accounts with privileged access on suspicious activity |
| KSI-IAM-AAM | Securely manage lifecycle and privileges of all accounts, roles, and groups using automation |

## Incident Response (INR)

| ID | Requirement |
|---|---|
| KSI-INR-RIR | Persistently review effectiveness of incident response procedures |
| KSI-INR-RPI | Persistently review past incidents for patterns or vulnerabilities |
| KSI-INR-AAR | Generate incident after-action reports and incorporate lessons learned |

## Monitoring, Logging, and Auditing (MLA)

| ID | Requirement |
|---|---|
| KSI-MLA-OSM | Operate SIEM or similar for centralized, tamper-resistant event logging |
| KSI-MLA-RVL | Persistently review and audit logs |
| KSI-MLA-EVC | Persistently evaluate and test configuration of resources, especially IaC |
| KSI-MLA-LET | Maintain list of resources and event types to log, monitor, and audit |
| KSI-MLA-ALA | Use least-privileged, JIT access for log data (Moderate) |

## Policy and Inventory (PIY)

| ID | Requirement |
|---|---|
| KSI-PIY-GIV | Use authoritative sources to automatically generate real-time inventories |
| KSI-PIY-RVD | Review effectiveness of vulnerability disclosure program |
| KSI-PIY-RSD | Review effectiveness of security/privacy in SDLC aligned with Secure By Design |
| KSI-PIY-RIS | Review effectiveness of security investments |
| KSI-PIY-RES | Review executive support for security objectives |

## Recovery Planning (RPL)

| ID | Requirement |
|---|---|
| KSI-RPL-RRO | Persistently review Recovery Time Objectives (RTO) and Recovery Point Objectives (RPO) |
| KSI-RPL-ARP | Persistently review alignment of recovery plans with recovery objectives |
| KSI-RPL-ABO | Persistently review alignment of backups with recovery objectives |
| KSI-RPL-TRC | Persistently test capability to recover from incidents and contingencies |

## Service Configuration (SVC)

| ID | Requirement |
|---|---|
| KSI-SVC-EIS | Implement improvements based on persistent evaluation for security opportunities |
| KSI-SVC-SNT | Encrypt or otherwise secure network traffic |
| KSI-SVC-ACM | Manage configuration of resources using automation |
| KSI-SVC-VRI | Use cryptographic methods to validate integrity of resources |
| KSI-SVC-ASM | Automate management, protection, and regular rotation of secrets |
| KSI-SVC-PRR | Review state of resources after changes to remove residual risk (Moderate) |
| KSI-SVC-VCM | Validate authenticity/integrity of communications between resources (Moderate) |
| KSI-SVC-RUD | Remove unwanted federal customer data promptly when requested (Moderate) |

## Supply Chain Risk (SCR)

| ID | Requirement |
|---|---|
| KSI-SCR-MIT | Persistently identify, review, and mitigate potential supply chain risks |
| KSI-SCR-MON | Automatically monitor third-party software for upstream vulnerabilities |
