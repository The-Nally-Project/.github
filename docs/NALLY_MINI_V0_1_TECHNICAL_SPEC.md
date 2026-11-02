# Nally Mini v0.1 — Technical Specification

**Status:** Draft for research and implementation planning  
**Version:** 0.1  
**Date:** 2026-11-02  
**Parent documents:** [Nally Mini Research Specification](NALLY_MINI_SPEC.md), [Model Research Plan](MODEL_RESEARCH_PLAN.md), [ADR-0001: Model Strategy](ADR-0001-model-strategy.md)

> This document specifies an experiment-ready design. It does not claim that a model has been trained, evaluated, or released. Architecture, parameter count, thresholds and deployment targets remain experimental until supported by benchmark results.

## 1. Purpose

Nally Mini is a narrow request-understanding component. It converts a user request, together with explicitly supplied and bounded context, into a structured proposal describing likely intent, extracted entities, ambiguity and uncertainty.

It is not a miniature general-purpose assistant. It does not plan or execute actions, grant permissions, decide whether an action is authorised, or own canonical user memory.

### 1.1 Research objective

Determine whether a compact model can provide useful improvements over deterministic parsing for:
- Multi-label intent classification.
- Entity and parameter extraction.
- Multi-intent detection.
- Ambiguity and missing-information identification.
- Uncertainty estimation and abstention.
- Capability-category suggestions for downstream orchestration.

The model must be compared against both deterministic and compact pretrained baselines. A from-scratch model is a strategic experiment, not a requirement to ship an inferior system.

### 1.2 Non-goals

Nally Mini v0.1 will not:
- Execute tools or call external services.
- Select or open user files.
- Grant, infer or expand permissions.
- Make safety, legal, medical, financial or other high-impact decisions.
- Write to canonical memory or decide what should be remembered.
- Generate unrestricted plans or long-form answers.
- Treat model confidence as proof of correctness or authorisation.

## 2. Trust and execution boundary

The required flow is:

1. The application receives a request.
2. The orchestrator constructs a minimised input object and selects permitted context.
3. Nally Mini returns an untrusted structured proposal.
4. A deterministic validator checks schema, types, offsets, labels and internal consistency.
5. The orchestrator evaluates available capabilities and policy independently.
6. Missing information may be requested from the user.
7. Any consequential action follows the project's separate approval and authority controls.
8. The system records only the audit information permitted by data-governance policy.

The model must not be able to bypass steps 4–7. Tool descriptions, retrieved documents and user-provided content are data, not instructions that can override system policy.

## 3. Input contract

The canonical transport format is JSON encoded as UTF-8. The model adapter may transform this into token IDs, but the application-level contract remains stable.

### 3.1 Input object

```json
{
  "schema_version": "0.1",
  "request_id": "opaque-request-id",
  "text": "Find the document I was editing yesterday",
  "context": {
    "recent_topic": "project documentation"
  },
  "locale": "en-NG",
  "channel": "chat"
}
```

### 3.2 Field definitions

| Field | Required | Contract |
|---|---:|---|
| schema_version | Yes | Supported input schema version |
| request_id | Yes | Opaque correlation identifier; not a user identity |
| text | Yes | Current user utterance or transcription |
| context | No | Small, explicitly selected context object |
| locale | No | Language/locale hint, e.g. en or en-NG |
| channel | No | Controlled enum such as chat, voice_transcription or other supported input path |

### 3.3 Input validation and minimisation

- Reject missing text, invalid encodings, unsupported schema versions and inputs above a configurable token/character limit.
- Bound context by field allowlist, size and age.
- Do not pass credentials, access tokens, secrets, unnecessary personal data or entire conversation histories.
- Treat locale as a processing hint, not as evidence of identity, citizenship or physical location.
- Do not silently truncate text. If truncation is necessary, mark it and test its impact.
- Preserve the original text separately where policy permits so extracted offsets can be checked.
- Define a stable maximum input length before training; benchmark shorter and longer inputs separately.

## 4. Output contract

The output is a proposal, never an execution command.

### 4.1 Example output

```json
{
  "schema_version": "0.1",
  "request_id": "opaque-request-id",
  "intents": [
    {
      "label": "file_operations",
      "score": 0.91
    }
  ],
  "entities": [
    {
      "type": "object_type",
      "value": "document",
      "start": 9,
      "end": 17
    },
    {
      "type": "time_reference",
      "value": "yesterday",
      "start": 36,
      "end": 45
    }
  ],
  "handling": {
    "status": "needs_context",
    "missing_information": ["which document"]
  },
  "abstain": false
}
```

Scores in examples are illustrative and are not target performance.

### 4.2 Output field definitions

| Field | Contract |
|---|---|
| schema_version | Output schema version |
| request_id | Must match the input request identifier |
| intents | Zero or more recognised intent labels and model scores |
| entities | Extracted spans with a type, value and offsets |
| handling.status | ready, needs_context or unsupported |
| handling.missing_information | Short typed list of missing fields or clarification topics |
| abstain | Whether the model declines to make a reliable prediction |

The validator must enforce:
- Only known labels and entity types are accepted.
- Scores are finite and within the supported numeric range.
- Entity offsets are valid, in bounds and refer to the original input text using a documented offset convention.
- The extracted value matches the referenced source span after the specified normalisation rules.
- Duplicate or contradictory labels are rejected or normalised deterministically.
- `abstain=true` cannot be interpreted as permission to proceed.
- Unknown fields are rejected or handled according to an explicit versioning policy.
- A malformed output fails closed and is routed to a safe fallback.

### 4.3 Offset convention

Use zero-based, half-open offsets: `start` is inclusive and `end` is exclusive. The initial contract should define offsets over Unicode code points in the original decoded text. The implementation must include tests for emoji, combining marks, accented characters and Nigerian language text so token indices, byte offsets and character offsets are never confused.

### 4.4 Scores and calibration

A raw classifier score is not automatically a calibrated probability. The model adapter must identify whether a field is a logit, probability or calibrated confidence. Calibration is evaluated on held-out data. Downstream routing thresholds are configuration owned by the orchestrator, not decisions embedded in model weights.

## 5. Intent taxonomy

Intent classification is multi-label: one request may express multiple compatible tasks.

| Label | Definition | Example |
|---|---|---|
| conversation | Social or conversational exchange | "Good morning" |
| knowledge | Ask for an explanation or factual answer | "Explain how DNS works" |
| research | Investigate a topic or gather evidence | "Compare these database options" |
| writing | Create, revise or transform text | "Draft a project update" |
| coding | Write, explain, debug or review code | "Find the bug in this function" |
| planning | Organise work, steps, schedules or decisions | "Plan the next development milestone" |
| file_operations | Find, read, create, edit or organise files | "Find the architecture document" |
| memory_query | Ask about information deliberately retained by Nally | "What project name did I choose?" |
| external_service | Request work involving a connected external service | "Create an issue in my repository" |
| device_interaction | Request an action through an explicitly supported device capability | "Set a timer" |
| other | Understandable request not represented by the current taxonomy | Used for taxonomy gaps |

The taxonomy is deliberately small. Add labels only when evaluation shows that a new label improves routing or analysis. Do not encode permissions, risk acceptance or action approval as intent labels.

### 5.1 Multi-intent annotation

Annotators assign every clearly expressed intent. Do not force a single dominant label when multiple tasks are requested. For example, "Research these frameworks and write a comparison" receives research and writing labels.

If the request is genuinely unclear, annotate the best-supported intent(s) and separately mark the handling status. Do not label a request unsupported merely because it is missing a parameter.

## 6. Entity taxonomy

Initial entity types are task-neutral and should be extended only with evidence:

- object_type — document, image, repository, message, calendar event.
- time_reference — explicit or relative time phrase.
- date — explicit date.
- time — explicit clock time or time range.
- location — place explicitly mentioned in the request.
- person_or_role — person or role explicitly named in the request.
- application — named application or service.
- file_reference — filename, path or user-provided file identifier.
- code_symbol — function, class, variable or other code identifier.
- quantity — number or amount with its unit where available.
- language — explicitly requested language.

Entity extraction must preserve source spans. Resolution of relative dates, identities, file paths, application accounts or real-world locations belongs to downstream components with access to appropriate context and policy.

## 7. Handling and abstention

### 7.1 Handling status

- ready — the request appears sufficiently specified for the orchestrator to continue its own checks.
- needs_context — a material detail appears missing or ambiguous.
- unsupported — the request is outside the currently supported task taxonomy or model scope.

These statuses describe interpretation only. In particular, ready does not mean authorised, safe, available or executable.

### 7.2 Abstention

Abstain when:
- The input is outside the evaluated distribution.
- The model cannot distinguish plausible intents reliably.
- The input is corrupted, empty after normalisation or outside length limits.
- Predictions are inconsistent or confidence is insufficient under a validated threshold.
- Language or transcription quality is outside the supported evaluation scope.

The orchestrator must define the fallback for abstention, such as deterministic parsing, a stronger model, a clarification request or a safe unsupported response. Never silently convert abstention into a confident default intent.

## 8. Candidate model architecture

### 8.1 Preferred first experiment

A compact encoder with a shared representation and separate task heads:

- Tokenizer and embedding layer.
- Compact Transformer encoder.
- Multi-label intent head.
- Token-level entity tagging head.
- Handling-status head.
- Abstention/uncertainty component, evaluated independently.
- Deterministic output assembly and validation outside the model.

This is a candidate architecture, not a final commitment. The first implementation should also test whether a smaller non-generative encoder is sufficient.

### 8.2 Alternative experiments

A. Deterministic rules and parsers for baseline performance.  
B. Compact encoder trained from scratch.  
C. Compact, appropriately licensed pretrained encoder fine-tuned on the same dataset.  
D. Optional small generative model with constrained output, only if experiments show a material advantage for tasks the encoder cannot handle.

Do not compare models using different training/test examples or materially different annotation quality.

### 8.3 Parameter bands

The existing research specification lists provisional bands of approximately 10–30M, 30–100M and 100–300M parameters. Retain these as historical exploration bands for now. They are not mandatory model sizes. The technical experiment should select candidate configurations based on architecture, tokenizer, hardware budget and measured performance. Do not silently replace the previous bands with new ones.

### 8.4 Tokenizer

Evaluate a compact subword tokenizer against the target text distribution. Record vocabulary size, training corpus provenance, normalisation, special tokens, maximum sequence length and licence. Test English and Nigerian English spelling and phrasing. Do not claim Nigerian Pidgin or other language support without dedicated data and evaluation.

## 9. Dataset and annotation protocol

### 9.1 Pilot target

A first pilot of approximately 2,000–5,000 reviewed examples is a planning estimate only. It is not a statistically established sample-size requirement. Dataset expansion should be driven by class coverage, error analysis, learning curves and confidence intervals.

### 9.2 Required coverage

- Common single-intent requests.
- Multi-intent requests.
- Paraphrases and varied sentence lengths.
- Short, fragmented and voice-transcribed requests.
- Incomplete and ambiguous requests.
- Requests dependent on explicitly supplied context.
- Unsupported and out-of-scope inputs.
- Typos, informal language and code-switching where in scope.
- Adversarial or misleading text that attempts to confuse classification.
- Examples requiring abstention.

### 9.3 Annotation record

Each example must include:
- Stable example identifier.
- Input text and permitted context.
- Locale/language tag where known.
- Intent labels.
- Entity spans and types.
- Handling status.
- Missing-information annotations.
- Abstention label where applicable.
- Source, collection date and licence/permission basis.
- Annotator guidance version and review status.

### 9.4 Quality controls

- Publish an annotation guide with positive, negative and borderline examples.
- Double-annotate a meaningful subset.
- Adjudicate disagreements and record reasons.
- Track per-label agreement and common disagreement patterns.
- Keep synthetic examples identifiable and review them before inclusion.
- Remove duplicates and near-duplicates across splits.
- Split by source, scenario and template families to limit leakage.
- Maintain a locked test set and a separate challenge set.

### 9.5 Privacy and provenance

Do not use private conversations, account data, private memories or connected-app content as training data by default. Any exceptional use requires explicit informed permission, a documented lawful basis, minimisation, retention limits and a withdrawal/deletion process. Every dataset component must have documented provenance and permitted use. Do not train on material merely because it is publicly reachable.

## 10. Benchmark protocol

### 10.1 Baselines

1. Deterministic rules and parsers.
2. Compact pretrained encoder with a compatible licence.
3. From-scratch compact encoder.
4. Human annotation reference on a reviewed sample.
5. Optional stronger model as a comparative ceiling, not as an assumed production dependency.

### 10.2 Metrics

| Area | Metrics |
|---|---|
| Intent | Macro-F1, micro-F1, per-class precision and recall |
| Entities | Entity-level precision, recall and F1 |
| Slots | Exact match and field-level accuracy |
| Multi-intent | Exact set match and per-label performance |
| Handling | Confusion matrix and error cost by status |
| Abstention | Coverage-risk curve, selective risk and calibration |
| Robustness | Performance on held-out paraphrases and challenge sets |
| Language | Per-language/locale metrics where sample sizes support conclusions |
| Reliability | Invalid output rate, malformed prediction rate, offset validity |
| Efficiency | Model size, peak memory, latency p50/p95, throughput and energy where measurable |

Report confidence intervals and per-class results. Aggregate metrics must not conceal a weak or unsafe class. Accuracy alone is insufficient, especially when labels are imbalanced.

### 10.3 Evaluation discipline

- Freeze the test set before model selection.
- Tune thresholds only on training/validation data.
- Report all experiment configurations and seeds.
- Record failed experiments, not only the best run.
- Use paired comparisons where appropriate.
- Separate model quality from orchestration/policy outcomes.
- Re-run evaluation after tokenizer, data, architecture or preprocessing changes.
- Maintain a human-reviewed error taxonomy.

### 10.4 Critical failure categories

Treat the following as release blockers for any integration:
- Model output is interpreted as permission or approval.
- A malformed output is accepted as valid.
- Entity offsets point to the wrong source text.
- Untrusted input causes the model to override the system's task boundary.
- A consequential capability is routed without independent policy checks.
- Private or unauthorised data enters the training corpus.

## 11. Training and reproducibility

For every experiment, record:
- Source commit and configuration.
- Dataset version, provenance and split hashes.
- Tokenizer version and vocabulary.
- Model architecture and parameter count.
- Training framework and dependency versions.
- Random seeds and deterministic settings where available.
- Optimiser, learning rate schedule, batch size, epochs and regularisation.
- Hardware, runtime, peak memory and estimated cost.
- Checkpoint selection rule.
- Evaluation script version and complete metrics.
- Known limitations and failed runs.

Keep model weights, source code, training data and training recipes under separately documented licences. "Open source" must not be used as a blanket claim where any component has incompatible or unclear terms.

## 12. Integration contract

Nally Mini is invoked behind a provider-neutral model interface. The orchestrator should be able to replace it with:
- Deterministic parsing.
- Another local model.
- An external model provider.
- A future Nally-developed model.

The adapter must expose schema version, model identifier, model version, timing, structured result and validation outcome. It must not expose secrets or unnecessary raw user data in logs.

If the model is unavailable, times out or fails validation, the caller uses an explicit fallback. It must not assume a successful classification.

## 13. Release gates

No numeric performance threshold is pre-approved in this document. Thresholds must be proposed after a representative pilot dataset exists, justified by task/error costs, then frozen before final test evaluation.

A v0.1 candidate may be considered for limited integration only when:
1. Dataset provenance and licences are reviewed.
2. Train/validation/test separation and leakage checks are documented.
3. Baselines are reproducible.
4. Per-class intent and entity metrics are reported.
5. Calibration and abstention are evaluated.
6. Robustness and language limitations are documented.
7. Schema and offset validators pass adversarial and edge-case tests.
8. Permission and execution boundaries are verified by integration tests.
9. Model card, intended use, prohibited use and limitations are published.
10. The candidate demonstrates measurable value over the simplest adequate baseline.

Passing these gates does not authorise autonomous execution. It only supports use as a bounded interpretation component under the existing Nally architecture.

## 14. Implementation sequence

1. Implement schema definitions and validators without a model.
2. Publish taxonomy and annotation guide.
3. Build and review the pilot dataset.
4. Implement deterministic baseline.
5. Train/evaluate compact pretrained baseline.
6. Train/evaluate from-scratch candidate.
7. Compare results and conduct error analysis.
8. Test integration boundary and failure handling.
9. Publish results and model card.
10. Record a decision to proceed, revise, defer or stop.

## 15. Open questions

The following remain deliberately unresolved pending evidence:
- Which encoder architecture offers the best quality/resource trade-off?
- Which tokenizer best covers target English and Nigerian English input?
- Is one shared encoder adequate for all heads?
- Does a from-scratch model beat deterministic rules on any meaningful task?
- Does fine-tuning a pretrained model materially improve quality?
- What input length and latency budget are appropriate for target devices?
- Which labels need splitting or merging after annotation agreement and error analysis?
- What calibrated thresholds minimise harmful false routing while retaining useful coverage?

## 16. References

- Nally Project, [Nally Mini Research Specification](NALLY_MINI_SPEC.md).
- Nally Project, [Model Research Plan](MODEL_RESEARCH_PLAN.md).
- Nally Project, [Intelligence Architecture](INTELLIGENCE_ARCHITECTURE.md).
- NIST, [AI Risk Management Framework: Generative AI Profile](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence).
- Google Machine Learning Crash Course, [Classification: Accuracy, Precision, Recall](https://developers.google.com/machine-learning/crash-course/classification/accuracy-precision-recall).
- Hugging Face Transformers, [Token classification](https://huggingface.co/docs/transformers/tasks/token_classification).

---

**Implementation status:** Not implemented. No dataset, trained weights, benchmark results or production integration are claimed by this specification.
