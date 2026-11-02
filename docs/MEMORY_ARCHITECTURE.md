# Memory Architecture Specification

**Status:** Draft v0.1  
**Scope:** Personal memory across Nally clients and services.  
**Note:** This document defines design requirements; it does not claim an implementation exists.

## 1. Purpose

Nally should provide continuity across conversations, applications and devices without turning a person's history into opaque, uncontrolled model context.

The governing rule is: **the user controls personal memory; Nally processes it only under disclosed and inspectable policy.**

Memory is a security-sensitive data system, not merely a vector database or prompt-building technique.

## 2. Goals

- Preserve source evidence and provenance for important remembered claims.
- Distinguish what was stated, observed, inferred and left uncertain.
- Support time-aware facts, corrections, contradictions and supersession.
- Retrieve only information relevant to the current task.
- Let users inspect, correct, export, restrict and delete memories.
- Propagate corrections and deletion to derived indexes and summaries.
- Support portability across models and compatible deployments.
- Enforce Identity & Authority policy on every access path.

## 3. Non-goals

- Store every conversation forever by default.
- Treat model-generated summaries as unquestionable truth.
- Infer sensitive attributes without a clear, lawful and user-controlled purpose.
- Allow a model to silently rewrite canonical memory.
- Make vector similarity the sole source of truth.
- Promise immediate erasure from immutable backups where technical retention windows prevent it; limits must be disclosed and bounded.

## 4. Logical architecture

1. **Evidence vault:** Original user-approved source records, such as explicit memory statements or selected conversation events.
2. **Canonical memory store:** Structured, versioned facts and events with provenance, confidence, timestamps and policy metadata.
3. **Relationship layer:** Optional graph representation of entities and relationships, derived from canonical records.
4. **Retrieval indexes:** Full-text and vector indexes used to find candidate memories. These are derived and rebuildable.
5. **Memory manager:** Proposes, retrieves, updates, supersedes or forgets memories under policy.
6. **Policy and authority layer:** Enforces user settings, consent, scope, retention, access and deletion.

These are logical boundaries. They may initially share infrastructure, but must remain separable in code and permissions.

## 5. Record model

A canonical record should support:
- Stable identifier and schema version.
- Subject and typed predicate/value, or event type and payload.
- Source reference and provenance category.
- Creation time and source-event time.
- Valid-time interval where applicable.
- Transaction-time interval for when Nally stored or changed the record.
- Confidence and epistemic status: stated, observed, inferred, imported or uncertain.
- Sensitivity classification and purpose limitation.
- User visibility and correction state.
- Retention rule, deletion state and derived-artifact references.
- Supersession or conflict links.

Do not force every memory into a simple triple. Events, preferences, plans and documents may need different schemas.

## 6. Time and change

Use bitemporal semantics where temporal correctness matters:
- **Valid time:** when the fact applies in the user's world.
- **Transaction time:** when Nally learned or recorded it.

A new statement should not silently overwrite an older record. Determine whether it is a correction, changed preference, temporary state, new event or conflict. Preserve history only where user policy and retention rules allow it.

## 7. Retrieval pipeline

1. Identify the task and minimum context required.
2. Check whether memory use is enabled and authorised for this purpose.
3. Search structured, lexical, temporal, graph and vector sources as appropriate.
4. Rank candidates by relevance, recency, confidence and policy.
5. Resolve conflicts or expose uncertainty rather than inventing a single answer.
6. Return the smallest useful context with provenance.
7. Record access metadata according to audit policy.

Similarity is a retrieval signal, not proof of truth.

## 8. Lifecycle

Support propose, confirm, retrieve, correct, supersede, restrict, export, delete and audit operations. Confirmation requirements should be configurable by memory category. User correction takes precedence over unsupported inference.

## 9. Derived data and deletion

Embeddings, summaries, graph edges, caches and model-specific memory files are derivatives. Each derivative must be traceable to its source records or source set.

Deletion should authenticate and authorise the request, immediately exclude the record from ordinary retrieval, then propagate deletion or rebuild operations to registered derivatives and replicas. Apply the documented backup-retention schedule and report bounded exceptions. A deletion tombstone may prevent delayed sync from reintroducing content, but must not retain the deleted content itself.

## 10. Security and privacy

- Encrypt data in transit and at rest.
- Enforce tenant isolation server-side.
- Use least-privilege service identities.
- Keep secrets out of prompts, embeddings and ordinary logs.
- Restrict memory access by purpose and task.
- Test every retrieval path for cross-tenant leakage.
- Support retention limits and data minimisation.
- Treat imported documents and tool output as untrusted.
- Do not train shared models on personal memory by default.

## 11. Storage strategy

The project direction is cloud-first hosting with user control. A practical implementation may use relational storage as the canonical source, with optional vector and graph indexes as derived services.

Choose technologies based on transactional integrity, migrations, backup/restore, deletion, tenant isolation, exportability, operational cost, search quality and measured scale. Do not add a graph or vector database without a demonstrated need.

## 12. Evaluation

Measure retrieval precision and recall, provenance accuracy, temporal accuracy, contradiction handling, false-memory rate, irrelevant-context rate, deletion propagation and reintroduction rate, export completeness, cross-tenant isolation, latency, storage cost and recovery.

## 13. Open decisions

- Which memory categories are opt-in, automatic or confirmation-only?
- What is the initial canonical schema and export format?
- Which database and search components meet requirements with least complexity?
- What backup deletion window can be guaranteed?
- How should device or household sharing work?
- How should minors' accounts and guardian controls work in applicable jurisdictions?

Resolve these through evidence-backed decision records before production deployment.
