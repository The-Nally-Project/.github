# Nally Mini — Research Specification

**Status:** Research specification; no model has been trained or released.  
**Scope:** A small Nally-developed model for narrow routing and structured-understanding tasks.

## 1. Research question

Can a compact model reliably convert user requests into structured intent, context and capability requirements, while abstaining when uncertain, at useful latency and resource cost?

Nally Mini is not a general-purpose assistant or an authority system.

## 2. Proposed responsibilities

- Intent classification.
- Entity and parameter extraction.
- Capability-category mapping.
- Ambiguity detection.
- Structured task representation.
- Identification of missing information.
- Escalation to a stronger model or human clarification.

## 3. Explicit exclusions

Nally Mini must not grant or expand permissions, decide that a sensitive action is authorised, execute tools, modify canonical memory, make high-impact professional or safety decisions, or act as a source of truth. Its outputs are proposals for downstream validation.

## 4. Architectures to compare

1. Compact encoder with classification and extraction heads.
2. Small generative model constrained to a structured output schema.
3. Hybrid pipeline using deterministic parsing for simple patterns and a model for ambiguous cases.

Do not assume a generative transformer is necessary. Choose the smallest approach that meets measured requirements.

## 5. Parameter-size experiments

Initial research bands may include approximately 10–30M, 30–100M and 100–300M parameters, subject to architecture and hardware feasibility. These are experiment bands, not a committed size or performance claim.

Measure model size, memory, latency, throughput, energy where measurable, and quality together.

## 6. Dataset design

Build a documented, licensed dataset covering general intent, capability routing, entity extraction, multi-intent requests, ambiguity, incomplete requests, changing context, unsupported tasks, adversarial content and abstention.

Later phases may evaluate Nigerian English, Nigerian Pidgin and selected African languages. Do not include private conversations without explicit informed permission and a documented lawful basis.

Use synthetic examples only with quality review and clear provenance. Split train, validation and test data by source and scenario to reduce leakage. Maintain a held-out challenge set.

## 7. Annotation

Each example should record input and locale, expected intent and fields, capability category, missing information, abstention/escalation label, source, licence, annotation guidance and quality status. Adjudicate disagreements and measure agreement where practical.

## 8. Training and reproducibility

Document architecture, tokenizer, data sources and licences, preprocessing, deduplication, split methodology, training configuration, seeds, evaluation code, checkpoints, dependencies and model limitations.

A reproducible training recipe is a deliverable. Do not publish unlicensed or personal data.

## 9. Metrics and release gates

Measure intent macro-F1 and per-class results, entity/slot precision and recall, exact-match accuracy, structured-output validity, calibration, abstention quality, false-positive routing, out-of-distribution robustness, language-group performance, latency, peak memory and model size.

Predeclare thresholds on held-out tests. False routing into consequential capabilities is a critical failure mode. The model must never be used as a permission or safety classifier.

## 10. Baselines

Compare deterministic rules, a small pretrained model with a suitable licence, a stronger external model as a benchmark, and human annotation on a representative sample.

Training from scratch is an experiment, not a requirement if it performs materially worse or adds unjustified cost.

## 11. Phased plan

1. Define task taxonomy and annotation guide.
2. Assemble a small licensed dataset.
3. Establish deterministic and pretrained baselines.
4. Train a minimal candidate.
5. Evaluate generalisation, abstention and resource use.
6. Red-team routing errors and untrusted-content inputs.
7. Publish a model card and reproducible results.
8. Decide whether to continue, distil, redesign or discontinue.

## 12. Decision criteria

Proceed only if the model provides measurable value over simpler alternatives in quality, privacy, cost, latency, independence or offline capability. A self-developed model is not inherently better merely because it is owned by the project.


## Detailed v0.1 technical specification

The experiment-ready contract, intent and entity taxonomy, annotation protocol, architecture comparisons, benchmark methodology, integration boundary and release gates are defined in [Nally Mini v0.1 Technical Specification](NALLY_MINI_V0_1_TECHNICAL_SPEC.md). The parameter bands above remain provisional; do not treat them as a selected model size.
