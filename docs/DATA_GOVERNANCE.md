# Data Governance & Privacy Specification

**Status:** Draft v0.1  
**Scope:** Personal data handled by Nally services, clients, models and integrations.

## 1. Purpose and commitment

Nally's cloud-first direction does not transfer ownership or unrestricted control of personal data to the project. The user remains in control of personal memories and settings. Nally acts only within disclosed purposes, permissions and retention rules.

This is an engineering baseline, not legal advice or a claim of compliance with any particular law.

## 2. Governance principles

- Purpose limitation and data minimisation.
- Clear notice and meaningful user control.
- Privacy and security by design and default.
- Separation of user data from project telemetry.
- Least-privilege access and tenant isolation.
- Traceable processors and data flows.
- Export, correction and deletion pathways.
- No default use of personal memory for shared-model training.
- Documented retention and incident response.

## 3. Data classification

At minimum, classify data as:
- **Public:** intended for public release.
- **Internal:** project operational information not intended for public release.
- **Personal:** information associated with an identifiable user.
- **Sensitive personal:** data requiring heightened controls due to nature or potential impact.
- **Secret/credential:** passwords, tokens, private keys and authentication material.

Classification should drive storage, logging, access, encryption, retention and model-routing rules. When uncertain, use the more protective classification until reviewed.

## 4. Data inventory and flow mapping

Every production component should document data collected or generated, source and purpose, user notice and control, storage location, processors and subprocessors, access roles, retention and deletion rules, encryption and key management, export path and cross-border transfers where applicable.

Do not connect a new processor or model provider without reviewing its data handling and contractual terms.

## 5. Collection and consent

Collect only information needed for a clearly stated feature. Separate essential service processing from optional features such as long-term memory, analytics and model improvement.

Consent must be specific, informed, revocable where applicable and recorded in an auditable form. A user action for one purpose is not blanket permission for unrelated purposes.

## 6. Model and provider processing

Before sending data to an external model or service:
- Determine whether the task requires the data.
- Apply user settings and data classification.
- Minimise or redact identifiers where feasible.
- Check provider retention, training, location and access terms.
- Disclose relevant processing choices.
- Provide local or alternative routes when supported and appropriate.

Provider selection must not silently override user privacy settings.

## 7. Retention and deletion

Define retention by data category and purpose. Avoid indefinite retention by default. Deletion must cover canonical records, caches, search indexes, embeddings, derived summaries and connected processors to the extent technically and contractually possible.

Document backup expiry windows and exceptions. Restrict deleted data from ordinary retrieval immediately, even while backup expiry is pending.

## 8. User controls

The product should progressively support viewing and searching stored data, correcting records, disabling memory or categories, restricting memory by task or integration, exporting data in a documented format, deleting records or accounts, reviewing connected providers and revoking access, and understanding retention and processing choices.

## 9. Security controls

Use encryption in transit and at rest, managed secrets, access reviews, tenant isolation, secure backups, vulnerability management, audit trails and tested recovery. Never place secrets in prompts, source control, analytics or ordinary application logs.

## 10. Children and vulnerable users

The product must account for age, capacity, local legal requirements and safeguarding needs. Avoid profiling, manipulative engagement and unnecessary collection. Age-appropriate defaults and guardian controls require legal and user research before launch. Do not assume one global age or consent rule.

## 11. Incident management

Maintain a documented process to detect, triage, contain, investigate, remediate and learn from incidents. Preserve evidence securely, limit internal access, assess affected data and users, and meet applicable notification duties. Do not make public claims before facts are verified.

## 12. Legal review

Before public deployment, obtain jurisdiction-specific review of applicable privacy, data protection, consumer, child-safety, cybersecurity and cross-border transfer obligations. Do not claim certification or legal compliance without evidence and a defined scope.

## 13. Open decisions

- Initial hosting and data-region strategy.
- Retention schedule for each data category.
- Consent and age-appropriate account design.
- Processor approval and review process.
- Export format and deletion service-level targets.
