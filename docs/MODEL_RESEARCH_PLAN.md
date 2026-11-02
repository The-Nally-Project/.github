# Model Research Plan

**Status:** Active research plan; no model has been trained or selected for production.  
**Decision basis:** ADR-0001 — independent long-term ambition, hybrid initial providers.  
**Last reviewed:** 2026-11-02

## 1. Research mission

Build the evidence, datasets, evaluation systems and engineering capability required for Nally to progressively own its model family, while using a replaceable mix of external and open-weight models during early development.

The plan is deliberately staged. Nally should first learn which model capabilities its product actually needs, then build narrow models and adapt existing weights where that is more effective. General-purpose pretraining is a later decision requiring a separate feasibility case.

## 2. Research questions

1. Which Nally tasks need a model, and which are better handled by deterministic software?
2. Which available external and open-weight models provide the best quality, safety, privacy, latency and cost for each task?
3. Can a small Nally-trained model outperform deterministic and compact pretrained baselines on a narrow, well-defined task?
4. Which capabilities should be handled locally, in Nally-controlled cloud infrastructure or by an external provider?
5. What data, compute, expertise, evaluation and governance would be required for Nally to adapt or eventually pretrain larger models?
6. How can Nally keep model behaviour replaceable without changing memory ownership, permissions or product interfaces?

## 3. Workstreams

### A. Task and capability mapping

- Inventory product tasks: conversation, extraction, classification, planning, coding assistance, document understanding, retrieval, tool selection and multimodal input.
- Assign each task a risk/sensitivity class and identify whether deterministic logic is sufficient.
- Define required input/output schemas, context needs, latency targets and acceptable failure behaviour.
- Identify which tasks are appropriate for a compact specialised model versus a stronger general model.
- Produce a task taxonomy and prioritised evaluation set.

**Deliverables:** task catalogue, risk tiers, schema definitions and prioritised benchmark set.

### B. Provider and open-weight landscape

- Build a shortlist across external APIs and open-weight models.
- Verify model version, modalities, context limits, structured-output support, tool-use behaviour, availability and deployment options.
- Review model-weight, code, dataset and usage licences separately.
- Review data retention, training-use terms, processing regions, subprocessors and deletion controls.
- Estimate cost and latency using Nally-representative workloads rather than advertised prices alone.
- Record whether each model can be used in hosted, self-hosted or local deployment.

**Deliverables:** provider/model comparison matrix, licence register, data-processing review and shortlist.

### C. Nally evaluation harness

- Create versioned prompts, datasets, task definitions and scoring scripts.
- Run identical test cases across candidate models and deterministic baselines.
- Include exact-match and schema validation, task success, factuality, citation/provenance, tool selection, refusal/abstention, robustness and multilingual performance where relevant.
- Measure latency distributions, throughput, error rates, memory/compute needs and estimated cost per task.
- Separate automated scoring from human review; periodically audit model-graded results.
- Keep a held-out challenge set and prevent training/evaluation leakage.
- Track model and provider version changes over time.

**Deliverables:** reproducible benchmark repository, baseline report, evaluation dashboard or reports, regression suite.

### D. Nally Mini — train a minimal model

Start with the existing Nally Mini research specification. Compare:
1. Deterministic rules and parsers.
2. Compact pretrained encoder/classifier or small generative model with a suitable licence.
3. A small model trained from scratch for a narrow task.
4. A hybrid deterministic/model pipeline.

Initial research parameter bands (10–30M, 30–100M and 100–300M) are experimental ranges, not a committed architecture or target size.

Prioritise intent classification, structured extraction, capability-category suggestions, ambiguity detection and abstention. The model must not grant permissions, execute tools or decide whether a consequential action is authorised.

**Deliverables:** dataset card, annotation guide, baseline report, reproducible training recipe, candidate checkpoint if justified, model card and go/no-go report.

### E. Nally Core — adapt an existing model

Only after the task map and evaluation harness exist:
- Identify a suitable open-weight base model and verify its licence and deployment constraints.
- Establish an unmodified baseline before any adaptation.
- Evaluate prompting, retrieval/context engineering and deterministic tools before training.
- If a persistent behaviour gap remains, test parameter-efficient fine-tuning and supervised post-training.
- Consider preference optimisation or distillation only where a validated dataset and measurable objective exist.
- Maintain a clean separation between base weights, Nally adapters, training data and evaluation artefacts.

**Deliverables:** base-model selection report, adaptation experiment, reproducible recipe, model card and comparative evaluation.

### F. Long-term independent model feasibility

Do not begin general-purpose pretraining until the project can produce a credible feasibility case covering:
- Target capability and differentiation.
- Data rights, quality, diversity, provenance and governance.
- Architecture, tokenizer and training objective research.
- Compute, storage, networking, energy and engineering costs.
- Training stability, evaluation, safety and post-training requirements.
- Inference deployment, serving cost and reliability.
- Funding, staffing, release and maintenance obligations.

**Deliverable:** independent-pretraining feasibility study and explicit investment decision. A conclusion not to pretrain is valid if evidence does not justify it.

### G. Safety, privacy and model governance

- Apply a risk-management lifecycle to model selection, training, evaluation, deployment and monitoring.
- Red-team prompt injection, untrusted retrieved content, tool misuse, privacy leakage, overconfidence and unsafe delegation.
- Ensure all model-proposed actions pass independent capability and policy checks.
- Keep private user memories out of shared training data by default.
- Maintain provenance and deletion processes for datasets and derived artefacts where applicable.
- Publish limitations, intended use, excluded use, evaluation scope and known failure modes.

**Deliverables:** model risk register, data provenance register, release checklist, model cards and incident/review process.

## 4. Research phases and stage gates

| Phase | Focus | Exit criteria |
|---|---|---|
| 0 | Task taxonomy and requirements | Prioritised tasks, risk tiers, schemas and success metrics approved |
| 1 | Provider/open-weight comparison | Shortlist with reproducible evaluation and licence/privacy review |
| 2 | Evaluation harness | Repeatable benchmark and deterministic baselines |
| 3 | Nally Mini experiments | Candidate beats baseline on agreed metrics without critical routing failures, or research is stopped/redesigned |
| 4 | Nally Core adaptation | Adaptation produces material improvement over base model and simpler alternatives |
| 5 | Independent pretraining feasibility | Evidence-backed cost, data, compute, capability and governance case reviewed |

Phases may overlap only where doing so does not compromise baseline quality, provenance or evaluation integrity. Passing a research gate does not automatically authorise production deployment.

## 5. Evaluation framework

Evaluate by task category and difficulty, not only by one aggregate score.

- **Task quality:** task success, accuracy, exact match, extraction precision/recall, schema validity.
- **Reliability:** repeatability, error rate, recovery, timeout and fallback behaviour.
- **Calibration:** confidence calibration, uncertainty detection, appropriate abstention and clarification.
- **Safety and authority:** policy violation rate, unsafe tool proposals, permission-boundary compliance and prompt-injection robustness.
- **Grounding:** citation accuracy, provenance preservation and unsupported-claim rate.
- **Efficiency:** latency p50/p95, throughput, memory, compute and cost per successful task.
- **Privacy and deployment:** data sent externally, retention terms, region, local execution feasibility and user controls.
- **Inclusion:** performance across supported languages, accents, dialects and accessibility scenarios as datasets become available.

Pre-register critical failure conditions. A high average score cannot compensate for critical permission-boundary failures.

## 6. Technical research stack candidates

These are candidates for controlled evaluation, not final technology selections:
- PyTorch and Hugging Face Transformers for model experimentation.
- Hugging Face Datasets for versioned dataset workflows.
- PEFT/LoRA-style methods for parameter-efficient adaptation where appropriate.
- Hugging Face TRL for supervised fine-tuning and preference/post-training experiments when justified.
- A provider-neutral model gateway and an independent evaluation harness.
- Reproducible experiment tracking, dataset versioning and model artefact manifests.

Select tools based on maintainability, licence, reproducibility, hardware compatibility and team capacity. The TRL project documents support for SFT, DPO, reward modelling and related methods; use only the methods needed for a defined research question.

## 7. Data governance requirements

Every dataset must have:
- Named source and collection method.
- Licence/permission and permitted-use record.
- Personal-data and sensitivity assessment.
- Collection date, version, transformations and deduplication notes.
- Annotation instructions, quality controls and known biases.
- Train/validation/test split rationale and leakage controls.
- Retention, correction and deletion procedure where applicable.

Do not scrape or ingest data merely because it is publicly reachable. Do not use private user conversations or personal memories for model training by default. Synthetic data must be labelled as synthetic and reviewed for quality, duplication and bias.

## 8. Decision rules

- Prefer deterministic software when it is simpler and sufficiently reliable.
- Prefer a pretrained model when it meets requirements and has acceptable licence, privacy and operational terms.
- Train a narrow model when it offers measurable improvements in cost, latency, privacy, offline capability or task quality.
- Adapt open weights when a persistent, measurable behaviour gap justifies the training cost.
- Consider independent pretraining only when Nally has a differentiated objective and credible data, compute, people, safety and funding plans.
- Do not select models solely on parameter count, benchmark headlines or brand reputation.

## 9. Initial 90-day research sequence

This is a sequence of work, not a delivery promise:
1. Finalise task taxonomy, risk tiers and representative test cases.
2. Define model/provider review template and licence/data-handling register.
3. Implement the first evaluation harness and deterministic baselines.
4. Compare a small set of external and open-weight candidates on identical tasks.
5. Select one narrow Nally Mini experiment and create its licensed dataset plan.
6. Publish a first research report with results, costs, limitations and recommended next gate.

## 10. Research references

- NIST, *AI Risk Management Framework: Generative AI Profile* — lifecycle risk management for generative AI: https://doi.org/10.6028/NIST.AI.600-1
- Hugging Face TRL documentation — post-training methods and trainer capabilities: https://huggingface.co/docs/trl/index
- Stanford CRFM HELM — transparent, reproducible model evaluation framework. HELM entered maintenance mode in June 2026, so assess its current status and alternatives before adopting it as a dependency: https://github.com/stanford-crfm/helm

These references inform the research process; they do not determine Nally's model choice.
