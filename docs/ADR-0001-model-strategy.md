# ADR-0001: Independent Long-Term Model Strategy with Hybrid Initial Providers

- **Status:** Accepted
- **Date:** 2026-11-02
- **Decision owners:** Project steward, subject to the project's governance and review process
- **Scope:** Model ownership, provider strategy, model research and intelligence architecture

## Context

The Nally Project intends to build an independent intelligence platform rather than remain a thin interface over another company's model API. At the same time, building a competitive general-purpose foundation model requires substantial data, compute, research, evaluation, safety and operational capacity. Nally needs useful model capabilities during product development without locking its architecture to one external provider.

The project has already established that Nally owns its intelligence platform, while users control their personal memories and settings. Models must not own identity, canonical memory, permissions or privileged execution.

## Options considered

1. **Single external provider:** simplest initial integration, but creates strategic, pricing, availability, policy and capability dependency.
2. **Open-weight-only from the beginning:** offers more deployment control, but may constrain capability, quality, latency or operational feasibility for early prototypes.
3. **Hybrid initial providers, with independent long-term ambition:** use replaceable external and open-weight models while building Nally's own evaluation, data, training and model capabilities.
4. **Build a general-purpose foundation model immediately:** maximises direct model ownership in theory, but creates high cost and execution risk before Nally has sufficient data, infrastructure, expertise or validated product requirements.

## Decision

Nally adopts **independent model ownership as its long-term ambition** and a **hybrid provider strategy initially**.

This means:

- Nally will progressively develop and maintain its own model family where evidence shows a meaningful benefit.
- Nally may initially use multiple external model providers and suitable open-weight models through replaceable adapters.
- No single provider is a permanent architectural dependency or the owner of Nally's identity, memory, permissions, orchestration or user relationship.
- Nally will build its own evaluation harness, task datasets, model registry, routing policy and reproducible research process.
- Nally Mini remains the first narrow in-house model research target. Its proposed role is intent understanding, structured extraction, routing suggestions and ambiguity detection—not general-purpose reasoning, permission granting or tool execution.
- Fine-tuning or post-training an appropriately licensed open-weight foundation model may precede any attempt at independent large-scale pretraining.
- Independent pretraining of a general-purpose model is a later research and investment decision, not an immediate implementation commitment.
- Provider selection must be based on measured task performance, safety, privacy, licence, latency, reliability and total cost—not brand preference.

## Reasons

The hybrid start gives Nally access to capable models while preserving the option to replace them. Independent ownership creates a direction for accumulating research capability, specialised data, model evaluation expertise and differentiated technology. A staged approach reduces the risk of spending scarce resources on large-scale pretraining before Nally knows which capabilities matter.

## Technical consequences

- Define a stable model gateway and provider-neutral request/response contracts.
- Keep provider-specific SDKs and schemas behind adapters.
- Maintain a model registry recording owner/provider, exact version, weights/source, licence, modalities, context limits, deployment location, data handling terms, evaluation results and operational status.
- Route requests using task type, measured capability, sensitivity, user settings, deployment location, latency, reliability, cost and licence constraints.
- Support explicit fallback and graceful degradation; do not silently change data handling or privacy properties during fallback.
- Keep policy enforcement, approval, tool execution, canonical memory and audit outside model authority.
- Build repeatable offline evaluations and production monitoring before automatic model routing is enabled.
- Preserve the ability to run suitable models locally or in Nally-controlled infrastructure when feasible.

## Research and investment consequences

Research proceeds from baselines and narrow models to adaptation of existing weights, and only then to considering larger-scale pretraining. Every stage must demonstrate measurable value over simpler or existing alternatives. Training from scratch is not a goal in itself.

## Security, privacy and governance

- Classify request data before provider routing.
- Send only the minimum context required for the task.
- Record provider data retention, training-use, region and subprocessors in the provider review.
- Do not send private user memories to training pipelines by default.
- Dataset collection requires provenance, licence review, privacy review and documented permitted use.
- Model output is untrusted input to downstream policy and execution systems.
- Users must have understandable controls for model choice and data-processing preferences where supported.
- Publish model cards, limitations, evaluation methodology and known failure modes for Nally-owned releases.

## Risks and mitigations

- **Provider lock-in:** provider-neutral interfaces, exportable evaluation sets and more than one viable model path.
- **Cost or latency escalation:** per-task budgets, caching where appropriate, routing evaluation and usage monitoring.
- **Inconsistent provider behaviour:** shared conformance tests, structured output validation and provider-specific capability declarations.
- **Privacy leakage:** data minimisation, provider due diligence, local processing options and no default training on private memories.
- **Premature model training:** stage gates requiring measurable improvements and realistic resource estimates.
- **Licence or dataset disputes:** provenance records and legal review before training or distribution.
- **Overclaiming independence:** distinguish owned orchestration, adapted weights and independently pretrained weights in all public statements.

## Review triggers

Revisit this decision when:
- Nally's task evaluations show a sustained advantage from a particular provider or deployment mode.
- Provider terms, pricing, availability or data handling materially change.
- Nally has sufficient lawful data, compute, expertise and funding to assess independent pretraining.
- Nally Mini fails to outperform simpler baselines or creates unacceptable routing errors.
- A licensing, privacy, security or regulatory finding changes what can be trained or deployed.

## Related documents

- [Intelligence Architecture](INTELLIGENCE_ARCHITECTURE.md)
- [Model Research Plan](MODEL_RESEARCH_PLAN.md)
- [Nally Mini Research Specification](NALLY_MINI_SPEC.md)
- [Data Governance & Privacy](DATA_GOVERNANCE.md)
- [Quality & Testing](QUALITY_AND_TESTING.md)
