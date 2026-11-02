# Intelligence Architecture Specification

**Status:** Draft v0.1  
**Scope:** Model routing, orchestration, planning, context assembly and tool use.

## 1. Purpose

Nally's intelligence should be model-independent. No single model should own identity, authority, canonical memory or privileged execution.

Models interpret input and propose outputs. Orchestration coordinates models and services. Policy evaluates whether an operation is permitted. Execution adapters perform approved operations. Auditing records relevant outcomes.

## 2. Principles

1. Model replaceability: providers can change without rewriting core policy.
2. Least privilege: models receive only context and tools needed for the task.
3. Structured interfaces: important outputs use validated schemas.
4. Uncertainty: models can ask for clarification or abstain.
5. Provenance: retrieved facts and tool results retain source information.
6. Independent authority: model confidence never grants permission.
7. User choice: model and privacy settings are understandable and controllable.
8. Graceful degradation: outages produce clear limitations, not fabricated results.
9. Independent model ambition: Nally progressively develops and maintains its own model family, but only where evidence justifies the investment.
10. Hybrid initial providers: Nally initially integrates replaceable external and open-weight models rather than depending on a single provider.

## 3. Logical components

- Client interface for text, voice, images and documents.
- Conversation and task manager.
- Context assembler.
- Intent and extraction layer.
- Planner.
- Model gateway.
- Tool and capability registry.
- Policy decision point.
- Constrained execution gateway.
- Result verifier.
- Audit and telemetry.
- Model registry and evaluation harness.

These components may initially be deployed together, but their interfaces and trust boundaries must be explicit.

## 4. Request lifecycle

1. Receive and normalise the request.
2. Determine task type, sensitivity and need for clarification.
3. Establish user identity and applicable policy.
4. Retrieve minimal authorised context.
5. Select a model based on capability, data policy, latency, quality and cost.
6. Generate a structured plan or response.
7. Validate the plan and every proposed action.
8. Obtain user approval when required.
9. Execute through a constrained capability adapter.
10. Verify the result using independent checks where possible.
11. Present a truthful summary and preserve permitted task state.

A model must not bypass policy by generating direct shell commands, API calls or credentials that execute without validation.

## 5. Model strategy

The accepted project direction is recorded in [ADR-0001](ADR-0001-model-strategy.md): independent long-term model ambition with a hybrid initial provider strategy.

- Develop Nally-owned models where measured quality, privacy, latency, cost, offline capability or strategic differentiation justify doing so.
- Integrate multiple external models and suitable open-weight models through replaceable adapters.
- Do not make any single provider a permanent architectural dependency.
- Build Nally-owned task evaluations, datasets with documented provenance, routing policy and model registry.
- Begin with narrow Nally Mini research; do not treat it as a general-purpose assistant or authority system.
- Consider adaptation/post-training of appropriately licensed open weights before considering independent general-purpose pretraining.
- Treat large-scale pretraining as a separate feasibility and investment decision.
- Distinguish source code, model weights, training data and training recipes. Their licences and availability can differ; do not describe a model as fully open without checking each component.

## 6. Model gateway and registry

The model gateway must expose provider-neutral interfaces for task requests, structured outputs, streaming, tool proposals, usage metadata, errors and cancellation. Provider-specific SDKs, authentication and response formats stay inside adapters.

The registry should record at minimum:
- Provider/owner and exact model/version identifier.
- Weight/source availability and licence.
- Supported modalities, context limits and structured-output/tool-use capabilities.
- Hosting/deployment location and hardware requirements where known.
- Data retention, training-use and processing terms.
- Evaluation dataset/version, task results and known limitations.
- Latency, reliability, operational cost and current lifecycle status.

Do not infer equivalent capabilities from a shared API shape. Each adapter must pass conformance tests and declare unsupported features.

## 7. Routing policy

Routing should consider task capability, measured quality, data sensitivity, processing location, user settings, latency, availability, cost, context window, structured-output support and licence constraints. Do not route solely by marketing label or one benchmark.

Fallback must preserve the user's data-handling requirements. If a fallback provider has different retention, processing-region or training-use terms, do not silently send the request there. Fail safely or request the user's choice.

## 8. Planning and tool use

Each plan step should define its goal, expected output, capability, validated input, data sensitivity, dependencies, read/write/send/delete effect, approval requirement, timeout, retry/idempotency behaviour, verification and failure path.

Tool output is untrusted data. It cannot change system policy or grant additional authority.

## 9. Context management

Assemble context just in time: current request, relevant task state, authorised memory, required tool descriptions, relevant workspace state and completion criteria. Avoid placing all tools, memories or documents into every prompt.

## 10. Reliability and safety

Validate outputs against schemas. Bound tool calls, time, retries and task budgets. Detect loops. Use idempotency keys for retryable writes where supported. Require approval for consequential operations. Provide cancellation and revocation paths. Treat provider responses as potentially incorrect or adversarial. Never report an unverified action as completed.

## 11. Observability

Record task IDs, model/provider/version, capability IDs, policy decisions, durations, error classes and verification outcomes as appropriate. Do not log raw personal content, credentials or sensitive memory by default.

User-facing progress should communicate meaningful states such as thinking, searching, reading, writing or awaiting approval. Internal budgets and diagnostics are not user-facing features.

## 12. Evaluation

Evaluate end-to-end task success, factuality, citation and provenance accuracy, tool selection, argument validity, permission violations, clarification quality, recovery, latency, cost and user control. Report by task category and difficulty.

## 13. Open decisions

- Initial orchestration framework and persistence model.
- Initial model gateway contract and provider adapters.
- Single-model or staged planning.
- Tasks requiring independent verification.
- Local inference targets and supported device classes.
- Model evaluation datasets, release thresholds and initial provider shortlist.
