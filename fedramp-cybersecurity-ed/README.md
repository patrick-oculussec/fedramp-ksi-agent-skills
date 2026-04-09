# FedRAMP Cybersecurity Education

> **Scope:** Enforce FedRAMP 20x Cybersecurity Education KSIs. Covers security training documentation, role-specific training reviews, development team secure coding training, and incident response training effectiveness. Apply when documenting training programs, reviewing training effectiveness, creating security awareness materials, or generating training compliance reports.

KSI Theme: A secure cloud service provider will educate their employees on cybersecurity measures, testing them persistently to ensure their knowledge is satisfactory.

## Overview

This KSI theme is primarily organizational rather than code-based. These skills help Claude generate documentation, review templates, and training program structures that satisfy the education-related KSIs.

## Code Generation Guardrails

While this theme is documentation-focused, code-adjacent concerns include:

- Security-focused code review checklists (supports KSI-CED-DET)
- Secure coding standards documentation (supports KSI-CED-DET)
- Security testing requirements in PR templates (supports KSI-CED-DET)
- On-call runbook maintenance requirements (supports KSI-CED-RRT)

## Training Program Documentation

### General Security Training (KSI-CED-RGT)

Generate documentation covering:
- Training topics: phishing awareness, password hygiene, data handling, incident reporting
- Delivery method: online modules, instructor-led, or hybrid
- Completion tracking: who completed, when, score
- Effectiveness measurement: phishing simulation results, quiz scores, incident reporting rates
- Review cadence: quarterly review of training content and completion rates

### Role-Specific Training for High-Risk Roles (KSI-CED-RST)

Generate documentation covering:
- Identification of high-risk roles (privileged access, data handlers, security operations)
- Role-specific training curriculum beyond general training
- Advanced topics: social engineering, insider threat, secure administration
- Completion and effectiveness metrics per role
- Review cadence: quarterly

### Development and Engineering Training (KSI-CED-DET)

Generate documentation covering:
- Secure coding practices aligned with OWASP Top 10 and CISA Secure By Design
- Language/framework-specific security training
- Security testing practices (SAST, DAST, dependency scanning)
- Code review security focus areas
- Hands-on exercises: CTF events, secure code review labs
- Review cadence: quarterly

### Incident Response and Recovery Training (KSI-CED-RRT)

Generate documentation covering:
- Tabletop exercise schedule and scenarios
- IR team drill cadence and skill requirements
- Communication procedures training (internal + FedRAMP ICP)
- Recovery procedure walkthroughs
- Post-exercise lessons learned and training gaps
- Review cadence: quarterly

## Compliance Review Checklist

- [ ] **KSI-CED-RGT**: General security training program documented; completion tracked; reviewed quarterly
- [ ] **KSI-CED-RST**: High-risk roles identified; role-specific training exists; effectiveness measured
- [ ] **KSI-CED-DET**: Secure coding training for dev/eng; aligned with Secure By Design; reviewed quarterly
- [ ] **KSI-CED-RRT**: IR/DR training with tabletop exercises; reviewed quarterly

## Documentation Generation

For training program templates and KSI-CSX-SUM summaries, see [templates.md](templates.md).

---

*Copyright (c) 2026 Patrick Clark / Oculus Security, LLC. [MIT License](../LICENSE).*
