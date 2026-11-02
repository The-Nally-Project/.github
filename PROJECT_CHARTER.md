# The Nally Project — Project Charter

**Status:** Foundational draft  
**Steward:** The Nally Project organisation

## 1. Mission

Build an open, interoperable personal intelligence layer that helps people understand, organise, and act across their digital environments while keeping people in control.

## 2. Vision

A future in which a person can work with one consistent personal intelligence system across phones, computers, applications, homes, vehicles, and other compatible environments—without surrendering control of personal data or authority.

## 3. Foundational decisions

- **Intelligence ownership:** Nally develops and governs its intelligence systems and interfaces.
- **Personal autonomy:** Users control their personal memories, settings, permissions, and delegated authority.
- **Openness:** The architecture, specifications, and ecosystem are intended to be open source, subject to clearly documented licences and security considerations.
- **Governance:** The project uses a steward model. The founding organisation acts as initial steward and is responsible for transparent project administration.
- **Model strategy:** Hybrid. Nally may develop its own models and integrate external models through replaceable interfaces.
- **Memory hosting:** Cloud-first as the initial default, with clear user controls and a path to portability and local options.
- **Hosting:** Cloud-first for initial coordination, with local execution where it improves privacy, latency, resilience, or cost.
- **Minimal model:** Research and develop a small Nally model for bounded tasks such as intent classification and routing. It is not an authority-granting component.

These decisions establish direction, not permission to skip validation. Implementation choices must be tested and documented.

## 4. Product boundaries

Nally is an intelligence and coordination layer, not a replacement operating system. It should integrate with existing platforms through documented interfaces and user-approved capabilities.

The system must distinguish:
- understanding a request;
- planning a possible action;
- obtaining authority to act;
- executing the action;
- recording and explaining the result.

A model's prediction is never, by itself, user permission.

## 5. Initial priorities

1. Publish governance, contribution, security, and architecture foundations.
2. Define the core identity, memory, permission, and capability interfaces.
3. Research a minimal model and establish evaluation criteria before training.
4. Build a narrow software prototype with observable, testable behaviour.
5. Add integrations only after permission boundaries and failure handling are defined.

## 6. Non-goals for the initial phase

- Replacing Android, iOS, Windows, macOS, or Linux.
- Unrestricted autonomous access to user accounts or devices.
- Training a large general-purpose foundation model from scratch.
- Promising universal compatibility before integrations are tested.
- Treating generated summaries or embeddings as the sole source of truth.

## 7. Measures of progress

Progress should be measured through reliability, user control, security, interoperability, accessibility, latency, resource use, memory accuracy, and the ability to explain and reverse authorised actions.

## 8. Change process

Foundational decisions may change through documented proposals that explain the evidence, trade-offs, migration impact, and user consequences. Changes must not silently weaken user control or security.
