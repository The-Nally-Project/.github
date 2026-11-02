# Capability System Specification

**Status:** Draft v0.1  
**Scope:** How Nally discovers, describes, authorises and invokes actions across software and devices.

## 1. Purpose

The capability system is the controlled boundary between Nally's intelligence and the external world. It turns integrations into discoverable, typed operations with explicit permissions and observable outcomes.

A connected service is not automatically authorised for every action it exposes.

## 2. Core concepts

- **Provider:** An application, service, operating system or device exposing functionality.
- **Capability:** A named operation or resource access with a defined contract.
- **Manifest:** Machine-readable description of a provider and its capabilities.
- **Adapter:** Implementation translating a standard Nally request into provider-specific calls.
- **Policy check:** Decision evaluating whether the current principal may invoke a capability.
- **Execution:** A bounded invocation with an auditable result.

## 3. Capability manifest

A descriptor should include stable provider and capability IDs, version, purpose, input/output schemas, read/write/send/delete or physical-effect classification, required scopes, data categories, approval requirements, execution location, network needs, timeout, rate limits, retry and idempotency properties, error model, verification method, provider identity and provenance.

Manifests describe functionality; they do not grant authority.

## 4. Discovery and lifecycle

Support discovery or registration, provider verification, permission review, scoped connection, task-specific capability exposure, health and compatibility monitoring, re-authorisation when scopes change, disconnection, credential revocation and removal of cached descriptions.

Discovery must never trigger execution.

## 5. Permission model

Check authenticated user and agent, provider and resource, requested action and scope, context and purpose, duration and expiry, risk classification, approval requirements, revocation and connection state.

Delegation must not broaden the delegator's authority. A manifest cannot override Identity & Authority policy.

## 6. Risk and approval

Classify operations by effect: read, prepare, write, send/publish, destructive, or physical/financially consequential. Risk depends on context and impact, not just the verb.

Require explicit informed confirmation for consequential actions. Some operations may remain disallowed regardless of confirmation.

## 7. Invocation contract

Each invocation should include unique task and invocation IDs, capability and manifest version, validated input, principal and delegation reference, policy decision, approval reference, deadline, idempotency key where appropriate, execution status, structured output or typed error, verification result and audit reference.

Never execute arbitrary natural-language arguments without schema validation and policy checks.

## 8. Isolation

Adapters should run with least privilege and restricted file, network, credential and host access. Prefer short-lived scoped credentials. Separate untrusted provider output from trusted control instructions. Apply timeouts, quotas and output-size limits.

For local device capabilities, the operating system remains an independent permission boundary. Nally approval does not replace platform consent.

## 9. Failure handling

Support states such as discovered, disconnected, awaiting authorisation, ready, running, awaiting approval, succeeded, failed, timed out, cancelled and revoked.

A timeout does not prove a remote operation failed. For non-idempotent actions, check operation status before retrying. Present uncertainty honestly.

## 10. Interoperability

Use protocol adapters where useful, including MCP and platform-native APIs, but keep a stable Nally capability contract independent of any one protocol. Protocol compatibility does not establish trust or authority.

## 11. Evaluation

Test manifest validation, permission enforcement, least privilege, revocation latency, schema rejection, replay protection, idempotency, timeout handling, provider compromise, cross-tenant isolation and truthful status reporting.

## 12. Open decisions

- Canonical manifest schema and versioning.
- Capability trust and signing model.
- Initial adapter runtime and sandbox boundaries.
- Approval UX and risk taxonomy.
- Representation of physical-device capabilities.
