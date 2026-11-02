# Identity and Authority Specification

**Status:** Draft v0.1  
**Scope:** Foundational identity, delegation, and permission model for The Nally Project.

## 1. Purpose

This specification defines how Nally distinguishes people, agents, devices, and services, and how authority is granted, limited, checked, and revoked.

The central rule is:

> Knowing who or what is making a request is not the same as having permission to fulfil it.

Identity establishes an authenticated subject. Authority establishes which actions that subject may perform, on which resources, under which conditions.

## 2. Design principles

1. **User sovereignty:** The user remains the source of authority over their personal environment and data.
2. **Explicit delegation:** Nally receives only the authority a user or authorised administrator deliberately delegates.
3. **Least privilege:** Every grant is limited to the minimum resources and actions needed.
4. **Separation of duties:** Models interpret and propose; policy enforcement decides; execution components perform authorised actions.
5. **Revocability:** Users can withdraw delegated authority. Revocation must be enforced at the point of action, not merely reflected in a user interface.
6. **Traceability:** Sensitive actions produce an understandable record of who or what requested, authorised, and performed them.
7. **No privilege inheritance by assumption:** A connected account, device, integration, or model does not automatically inherit the user's full permissions.

## 3. Identity classes

Nally should distinguish at least these identity classes:

| Identity | Meaning | Example |
|---|---|---|
| User | A person who owns or controls a Nally account | Account holder |
| Agent | A Nally process acting for a user | Personal Nally assistant |
| Device | A registered endpoint | Phone, laptop, home hub |
| Service | An external application or integration | Calendar provider |
| Organisation | A governed project or managed deployment | An enterprise workspace |

Each identity should have a stable internal identifier, issuer, lifecycle state, and authentication method appropriate to its class. Identifiers must not be treated as secrets.

## 4. Authority model

Authority is represented as a scoped relationship between a grantor, a grantee, and permitted actions on specific resources.

A grant should carry:

- **Grant ID:** stable identifier for the delegation.
- **Grantor:** the user or authority permitted to delegate.
- **Grantee:** the agent, device, service, or other subject receiving authority.
- **Resource:** the account, data, device, or capability in scope.
- **Actions:** the permitted operations.
- **Conditions:** contextual constraints, if any.
- **Issued at / expires at:** validity window.
- **Approval basis:** user approval, policy, or another documented authority.
- **Status:** active, expired, revoked, or suspended.
- **Audit reference:** associated decision and execution records.

A grant must not be broader than the grantor's own authority. Delegation cannot create new authority.

## 5. Permission dimensions

Permissions should be evaluated across five dimensions:

1. **Subject:** who or what is requesting the action?
2. **Resource:** what data, account, device, or service is affected?
3. **Action:** what operation is requested?
4. **Context:** under what conditions is it allowed?
5. **Duration:** when does the grant begin and end?

A permission check must fail closed when required information is missing, invalid, expired, or ambiguous.

## 6. User-facing authority levels

The interface may present a simple set of understandable levels. These are UX groupings, not substitutes for the underlying scoped grants.

| Level | Meaning | Example |
|---|---|---|
| Observe | Read or inspect permitted information | Read a selected calendar |
| Prepare | Draft or stage a proposed change without applying it | Prepare an email draft |
| Execute with approval | Perform a specific action after confirmation | Send the approved email |
| Restricted | Action is unavailable or requires a separate protected process | High-impact account or security changes |

The interface must explain what each grant allows, which resources it affects, and how to revoke it.

## 7. Approval and execution flow

For actions requiring approval, use this sequence:

1. Receive the user's request.
2. Authenticate the requesting subject.
3. Resolve the intended resource and action.
4. Prepare a proposed action with a clear description of its effects.
5. Evaluate the applicable policy and existing grants.
6. Obtain explicit user approval when required.
7. Re-check the grant, expiry, and revocation state immediately before execution.
8. Execute through a constrained capability.
9. Record the outcome, including partial failure.
10. Explain the result to the user.

Approval of one action must not silently authorise future actions or a broader category of actions.

## 8. Revocation and lifecycle

Users must be able to inspect and revoke grants. Revocation should invalidate relevant sessions or credentials, prevent new actions, and propagate to connected components as quickly as practical.

Components must not rely indefinitely on cached permission decisions. High-impact actions require an authoritative, current policy check.

If a service cannot confirm whether a grant remains valid, it must not proceed with the protected action.

## 9. Authentication and credential handling

Authentication proves control of an identity; it does not establish unlimited authority.

Implementations should prefer established protocols and platform security mechanisms rather than custom cryptography. Credentials should be scoped, protected in storage and transit, rotated where appropriate, and kept out of logs and model prompts.

Long-lived user credentials should not be exposed to models or general-purpose tool execution environments. Where supported, use short-lived, narrowly scoped tokens and revocable grants.

## 10. Audit records

For sensitive operations, record:

- requesting identity;
- acting agent or service;
- target resource and action;
- policy decision and relevant grant reference;
- whether user approval was required and obtained;
- execution timestamp and result;
- correlation ID for related events.

Audit records should minimise personal data, protect against unauthorised modification, and follow a documented retention policy. Do not log secrets or unnecessary content.

## 11. Threats and required mitigations

| Threat | Required design response |
|---|---|
| Prompt injection attempts to expand authority | Treat external content as untrusted; enforce permissions outside the model |
| Stolen or replayed credentials | Short-lived scoped credentials, secure storage, replay protections |
| Confused-deputy behaviour | Bind each action to the initiating user, grant, resource, and purpose |
| Stale cached permission | Re-check authoritative policy before protected execution |
| Overbroad integration access | Narrow scopes, resource-level restrictions, periodic review |
| Compromised device | Device registration, revocation, and risk-based restrictions |
| Misleading approval prompt | Show concrete target, action, and expected effect before confirmation |

## 12. Open design questions

The following require further research and a recorded decision before production implementation:

- Account recovery and identity-provider strategy.
- Multi-device registration and device-loss recovery.
- Shared households and delegated family or team administration.
- Offline permission behaviour and safe synchronisation.
- Emergency access and recovery procedures.
- Audit retention and user export format.
- Organisation-managed deployments and policy precedence.

## 13. Acceptance criteria

An implementation conforming to this specification must demonstrate that:

- identity classes are distinct and verifiable;
- grants are scoped, expiring, inspectable, and revocable;
- delegated authority cannot exceed the grantor's authority;
- model output cannot grant permissions;
- protected actions are checked immediately before execution;
- approval is specific to the proposed action;
- failure and partial execution are reported accurately;
- sensitive actions have privacy-conscious audit records;
- tests cover expiry, revocation, replay, ambiguity, and denied access.

## 14. Related standards to evaluate

These are candidate references, not automatic requirements:

- OAuth 2.0 and OAuth 2.0 Token Exchange (RFC 8693).
- OpenID Connect for authentication and identity claims.
- WebAuthn / FIDO2 for strong user authentication.
- NIST Zero Trust Architecture (SP 800-207).
- NIST Digital Identity Guidelines (SP 800-63 series).
- W3C Verifiable Credentials, where portable attestations are useful.

Adoption should be based on a threat model and interoperability requirements, not standards branding alone.
