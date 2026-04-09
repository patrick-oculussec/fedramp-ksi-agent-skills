# FedRAMP 20x Key Security Indicator Skills

Agent instructions that ensure code, infrastructure-as-code, and documentation meet [FedRAMP 20x Key Security Indicators (KSIs)](https://www.fedramp.gov/docs/20x/key-security-indicators/).

These are plain markdown files -- no vendor lock-in. They work with any AI coding agent that accepts markdown instructions.

## What These Instructions Do

When loaded into your AI coding agent, they automatically:

- **Guide code generation** with FedRAMP-compliant patterns, defaults, and guardrails
- **Review code and IaC** against KSI requirements, flagging non-compliant patterns
- **Generate FedRAMP documentation** including KSI-CSX-SUM implementation summaries

All instructions are cloud-agnostic and express requirements as general principles applicable to any provider or framework.

## Installation

### Cursor

Copy the directories you need into your project's `.cursor/rules/` or `.cursor/skills/`:

```bash
git clone <repo-url> fedramp-skills

# All skills
cp -r fedramp-skills/fedramp-*/ /path/to/your-project/.cursor/skills/

# Or specific skills
cp -r fedramp-skills/fedramp-iam /path/to/your-project/.cursor/skills/
```

For personal (cross-project) use, copy into `~/.cursor/skills/` instead. If placing in `.cursor/skills/`, add a YAML frontmatter block to each README.md -- see [Cursor Skills docs](https://docs.cursor.com/agent/skills).

### Claude Code (claude-code / CLAUDE.md)

Reference the instructions in your project's `CLAUDE.md`:

```bash
# Copy all skills into your project
cp -r fedramp-skills/fedramp-*/ /path/to/your-project/

# Then add to your CLAUDE.md:
echo "See fedramp-20x-compliance/README.md for FedRAMP 20x compliance instructions." >> CLAUDE.md
```

Or concatenate the specific instructions you need directly into `CLAUDE.md`.

### GitHub Copilot

Add to your `.github/copilot-instructions.md`:

```bash
# Concatenate the skills you need
cat fedramp-skills/fedramp-20x-compliance/README.md >> .github/copilot-instructions.md
cat fedramp-skills/fedramp-iam/README.md >> .github/copilot-instructions.md
# ... etc
```

### Windsurf / Other Agents

Copy the markdown files into whatever location your agent reads for project-level instructions (e.g., `.windsurfrules`, `rules/`, or equivalent). The content is standard markdown and works anywhere.

### AGENTS.md

This repo includes an [`AGENTS.md`](AGENTS.md) file at the root that serves as a master entry point. Agents that support `AGENTS.md` will pick it up automatically when this repo is cloned as part of a project.

## Skill Index

### Master Orchestrator

| Directory | Description |
|---|---|
| [`fedramp-20x-compliance/`](fedramp-20x-compliance/) | Entry point for full FedRAMP 20x compliance checks, reviews, and documentation generation |

### Tier 1 -- Code-Enforceable

| Directory | KSI Theme | Key Concerns |
|---|---|---|
| [`fedramp-iam/`](fedramp-iam/) | Identity and Access Management | MFA, RBAC/ABAC, least privilege, JIT access, service auth |
| [`fedramp-cloud-native/`](fedramp-cloud-native/) | Cloud Native Architecture | Network segmentation, attack surface, HA, immutable infra |
| [`fedramp-service-config/`](fedramp-service-config/) | Service Configuration | Encryption, secrets management, integrity validation, config-as-code |
| [`fedramp-change-mgmt/`](fedramp-change-mgmt/) | Change Management | Immutable deployments, CI/CD validation, change logging |
| [`fedramp-monitoring/`](fedramp-monitoring/) | Monitoring, Logging, Auditing | Structured logging, audit events, SIEM, log access controls |
| [`fedramp-supply-chain/`](fedramp-supply-chain/) | Supply Chain Risk | Dependency scanning, SBOMs, lockfiles, upstream monitoring |

### Tier 2 -- Infrastructure and Operations

| Directory | KSI Theme | Key Concerns |
|---|---|---|
| [`fedramp-recovery/`](fedramp-recovery/) | Recovery Planning | Backup IaC, RTO/RPO, recovery testing |
| [`fedramp-incident-response/`](fedramp-incident-response/) | Incident Response | Forensic logging, alerting, after-action reports |

### Tier 3 -- Documentation and Process

| Directory | KSI Theme | Key Concerns |
|---|---|---|
| [`fedramp-authorization/`](fedramp-authorization/) | Authorization by FedRAMP | Process docs, implementation summaries, cryptographic modules |
| [`fedramp-cybersecurity-ed/`](fedramp-cybersecurity-ed/) | Cybersecurity Education | Training documentation and review templates |
| [`fedramp-policy-inventory/`](fedramp-policy-inventory/) | Policy and Inventory | SDLC security, inventory automation, vulnerability disclosure |

## FedRAMP 20x KSI Coverage

These instructions cover all 11 KSI themes and ~60 individual indicators defined in FedRAMP 20x v0.9.0-beta. Each directory maps directly to a KSI theme and references specific KSI IDs (e.g., `KSI-IAM-MFA`).

Every instruction set supports generating the **KSI-CSX-SUM implementation summaries** required by FedRAMP 20x, which include:

1. Goals for implementation and validation with pass/fail criteria
2. Consolidated information resources to be validated
3. Machine-based validation processes and cycle
4. Non-machine-based validation processes and cycle
5. Current implementation status
6. Clarifications or responses to assessment summary

## Reference

- [FedRAMP 20x Key Security Indicators](https://www.fedramp.gov/docs/20x/key-security-indicators/)
- [FedRAMP 20x Definitions](https://www.fedramp.gov/docs/20x/fedramp-definitions/)
