# Architecture Decision Records

**Status:** Active process

## 1. Purpose

Important decisions should remain understandable months or years later. This document defines how The Nally Project records decisions that materially affect architecture, security, privacy, governance, compatibility or long-term cost.

## 2. When to create a record

Create a record when a decision changes a foundational principle, selects or removes a major technology, changes identity or permission semantics, changes personal-data storage or retention, creates a compatibility commitment, introduces a significant security or operational trade-off, or is difficult to reverse.

Small implementation details do not need a formal record.

## 3. Record format

Use a numbered file such as ADR-0001-short-title.md with:
- **Status:** Proposed, Accepted, Rejected, Superseded or Deprecated.
- **Date:** Date of the decision.
- **Decision owners:** Responsible maintainers or stewards.
- **Context:** Problem and constraints.
- **Options considered:** Real alternatives, including doing nothing.
- **Decision:** What is selected.
- **Reasons:** Evidence and trade-offs.
- **Consequences:** Benefits, costs, risks and migration impact.
- **Security/privacy implications:** Data, authority and threat-model changes.
- **Review trigger:** Evidence or conditions that would reopen it.

## 4. Process

1. Publish the problem and context.
2. Gather evidence and identify alternatives.
3. Invite review from affected maintainers and contributors.
4. Record unresolved risks and dissent where relevant.
5. The authorised steward or maintainer records the decision under project governance.
6. Update affected specifications and implementation issues.
7. Revisit when the review trigger occurs.

## 5. Principles

Explain reasons, not only outcomes. Distinguish evidence from assumptions. Prefer reversible decisions while uncertainty is high. Do not treat a proposal as accepted. Supersede records rather than silently rewriting history. Never include secrets or personal data.

## 6. Initial project directions

The project charter and architecture principles capture current direction, including open-source ecosystem intent, steward governance, cloud-first hosting direction, and user control over personal memory and settings.

The model strategy is now formally recorded as **independent long-term model ambition with a hybrid initial provider strategy** in [ADR-0001](ADR-0001-model-strategy.md). Its implementation and research implications are detailed in [Model Research Plan](MODEL_RESEARCH_PLAN.md).

A decision record captures a chosen direction; it does not mean the corresponding software, models, training data or infrastructure already exist.
