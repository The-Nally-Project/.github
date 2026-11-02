# Initial Threat Model

**Status:** Initial model v0.1; update as architecture and deployments become concrete.  
**Scope:** Nally clients, cloud services, memory, models, integrations and connected devices.

## 1. Security objectives

Protect user accounts and sessions, personal memory and documents, credentials and provider connections, device and service identities, capability execution and data integrity, availability, audit records and recovery systems.

Preserve confidentiality, integrity, availability, user control and truthful reporting.

## 2. Trust boundaries

Identify and enforce boundaries between:
- User device and Nally cloud.
- One user/tenant and another.
- Client, API, worker and database.
- Orchestrator and model provider.
- Model output and policy engine.
- Nally and external tool/provider.
- Local network and connected device.
- Production systems and development/test environments.
- Primary storage, replicas, backups and analytics.

Every boundary needs explicit authentication, authorisation, validation and logging.

## 3. Threat actors and failure sources

Consider malicious external users, compromised accounts or devices, malicious providers, compromised dependencies, insider misuse, prompt injection, accidental misconfiguration, data leakage, model errors and infrastructure failure.

Include mistakes and unsafe defaults, not only deliberate attackers.

## 4. Important threat scenarios

| Threat | Potential impact | Required mitigations |
|---|---|---|
| Account/session compromise | Unauthorised memory or tool access | Strong authentication, expiry, revocation, anomaly detection |
| Cross-tenant access flaw | Disclosure or modification of another user's data | Server-side tenant checks, isolation tests, scoped queries |
| Prompt injection in documents or tool output | Unauthorised proposed actions | Treat content as untrusted, separate data from instructions, policy checks |
| Excessive agent permissions | Unintended high-impact actions | Least privilege, scoped delegation, approvals, short-lived credentials |
| Malicious integration | Data theft or unauthorised operations | Provider verification, sandboxing, scopes, output validation |
| Secret leakage | Account or service compromise | Secret manager, redaction, rotation, scanning |
| Memory poisoning or false recall | Incorrect decisions | Provenance, confidence, correction, conflict handling, retrieval tests |
| Insecure model/provider processing | Personal data exposure | Minimisation, provider review, user controls, routing restrictions |
| Dependency/build compromise | Supply-chain compromise | Lockfiles, provenance, dependency review, signed releases where feasible |
| Deletion failure | Retained or reintroduced personal data | Derivative inventory, deletion workflows, tombstones, restore tests |
| Denial of service or cost abuse | Interruption or unexpected expense | Rate limits, quotas, budgets, backpressure and monitoring |
| Unsafe device action | Physical or property impact | Platform controls, approvals, state checks, independent interlocks |

## 5. Prompt injection and untrusted content

Treat user-provided documents, websites, emails, tool responses, model outputs and capability manifests as data, not trusted control instructions. Retrieved content cannot change system policy, grant permissions or create authority. Validate proposed actions independently.

## 6. Identity and authorisation

Use unique identities for users, agents, devices and services. Enforce authorisation at the service performing the action, not only in the UI or model prompt. Delegated authority must be scoped, time-limited and revocable.

## 7. Memory security

Protect canonical records and all derivatives. Enforce tenant boundaries on every retrieval path, including vector search, caches, graph queries, backups and exports. Audit privileged access. Prevent model-generated content from silently becoming trusted memory.

## 8. Application and infrastructure security

Use secure development lifecycle and peer review, dependency and secret scanning, transport security and encryption at rest, managed secrets and key rotation, network and service isolation, rate limiting and quotas, secure configuration and environment separation, backup/restore tests, vulnerability disclosure and incident response.

## 9. Abuse cases and red-team tests

Test cross-user and cross-device access, revoked credentials, malicious instructions in retrieved content, model attempts to invoke unlisted capabilities, invalid schemas and oversized output, replayed writes, ambiguous timeouts, memory deletion reintroduction, leaked test secrets and cancellation during multi-step work.

Testing must be authorised and performed in isolated environments.

## 10. Risk ownership and acceptance

Every risk should have an owner, severity, likelihood, mitigation, residual risk and review date. High or critical residual risks require explicit steward review and may block release. Model confidence is not risk acceptance.

## 11. Limitations

This is a starting threat model, not a penetration test, formal security audit or proof of safety. Update it when data flows, providers, privileges, platforms or deployment topology change.
