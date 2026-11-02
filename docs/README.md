# Engineering Documentation

This directory contains the technical specifications and engineering policies for The Nally Project.

## Foundational specifications

- [Architecture Principles](ARCHITECTURE_PRINCIPLES.md) — cross-project design constraints.
- [Identity & Authority](IDENTITY_AND_AUTHORITY.md) — principals, delegation, permissions, approval and revocation.
- [Memory Architecture](MEMORY_ARCHITECTURE.md) — evidence, structured memory, retrieval, lifecycle and user control.
- [Intelligence Architecture](INTELLIGENCE_ARCHITECTURE.md) — models, orchestration, planning and execution boundaries.
- [Model Research Plan](MODEL_RESEARCH_PLAN.md) — staged provider evaluation, Nally Mini, model adaptation and long-term pretraining feasibility.
- [Nally Mini](NALLY_MINI_SPEC.md) — scope and evaluation plan for a small in-house model.
- [Nally Mini v0.1 Technical Specification](NALLY_MINI_V0_1_TECHNICAL_SPEC.md) — input/output contracts, taxonomy, annotation, architecture experiments, benchmark and release gates.
- [Capability System](CAPABILITY_SYSTEM.md) — discoverable, typed and permissioned actions.
- [Data Governance & Privacy](DATA_GOVERNANCE.md) — data classification, lifecycle, privacy and portability.
- [Interoperability](INTEROPERABILITY.md) — cross-platform integration and protocol strategy.
- [Threat Model](THREAT_MODEL.md) — assets, trust boundaries, threats and mitigations.
- [Quality & Testing](QUALITY_AND_TESTING.md) — verification, evaluation and release principles.
- [Decision Records](DECISIONS.md) — how important architecture decisions are recorded.
- [ADR-0001: Model Strategy](ADR-0001-model-strategy.md) — independent long-term model ambition and hybrid initial providers.

## Project-level documents

- [Project Charter](../PROJECT_CHARTER.md)
- [Governance](../GOVERNANCE.md)
- [Contributing](../CONTRIBUTING.md)
- [Security Policy](../SECURITY.md)
- [Code of Conduct](../CODE_OF_CONDUCT.md)
- [Roadmap](ROADMAP.md)

## Status labels

Foundational means a project-level direction has been agreed. Draft means a design is proposed and may change. Research specification defines a question and evaluation method, not a completed implementation. Active means a process is in use.

A specification is not evidence that a feature exists. Implementation status must be tracked separately in the relevant repository, issue, test report or release.

## Change process

Substantial changes should be proposed, reviewed for security, privacy, accessibility, interoperability, operational cost and user autonomy, then recorded in Decision Records when they affect long-term architecture.
