# Interoperability Specification

**Status:** Draft v0.1  
**Scope:** Integration across applications, operating systems, devices and services.

## 1. Purpose

Nally is intended to act as a personal intelligence and coordination layer across existing environments. It is not initially intended to replace Android, iOS, Windows, macOS, Linux or their security models.

Integrate through supported interfaces and preserve platform-native controls.

## 2. Separate three concepts

- **Connectivity:** Can Nally communicate with a system?
- **Capability:** What operations does that system expose?
- **Authority:** Which operations may this user and agent perform, and under what conditions?

A connection does not imply capability, and capability does not imply permission.

## 3. Integration layers

Use adapters for cloud services and web APIs, desktop operating systems and applications, mobile operating systems and applications, local networks and smart-home devices, and future vehicle or robotics research.

Each adapter translates a platform interface into the Nally capability contract. Avoid leaking provider-specific assumptions into core orchestration.

## 4. Protocol strategy

Evaluate standards based on their actual role:
- **MCP:** Tool and context integration between clients and servers.
- **Matter:** Smart-home device interoperability.
- **W3C Web of Things:** Common descriptions and interaction models for web-connected things.
- **Platform-native APIs:** Android intents and supported App Functions; Apple App Intents and Shortcuts; Windows app and system APIs; Linux desktop and service interfaces.
- **ROS 2:** Potential future robotics integration, subject to independent safety controls.

No protocol is a universal permission system. Nally's own authority checks remain mandatory.

## 5. Capability description

A device or application description should include stable identity and owner association, provider and software version, available capabilities and schemas, supported inputs and outputs, current state and availability, execution location, network constraints, required platform permissions, safety constraints, and update/revocation mechanisms.

Treat device-reported descriptions as untrusted until verified.

## 6. Local and cloud responsibilities

The current direction is cloud-first, with local execution where it materially improves latency, resilience, privacy or device access.

Candidate local responsibilities include wake-word and basic interaction, local discovery, local-only actions and permission enforcement, selected offline functions, and sensitive-content handling where feasible.

Candidate cloud responsibilities include cross-device coordination, long-term memory services subject to user control, heavier model orchestration, account management and synchronisation.

Validate the split per feature. Local execution must not bypass policy; cloud execution must not bypass operating-system permissions.

## 7. Reliability

Adapters should expose health, capability availability, version compatibility, timeouts, retries and clear errors. Device state may be stale; actions with physical or consequential effects require current-state checks and appropriate confirmation.

## 8. Safety boundaries

For smart-home, vehicle and robotics integrations:
- Keep manufacturer and operating-system safety controls authoritative.
- Do not disable interlocks or safety systems.
- Separate simulation from real-world execution.
- Require additional validation for physical effects.
- Fail safely when state, identity or connectivity is uncertain.
- Keep emergency and manual controls independent of Nally.

## 9. Conformance and testing

Maintain provider-specific conformance tests and a shared adapter suite. Test identity, schema validation, permission mapping, revocation, stale state, malformed output, network loss, retries and version changes.

## 10. Open decisions

- Which integration protocol is first for the prototype?
- Which mobile and desktop platforms are initially supported?
- What local agent runtime is acceptable?
- How should device ownership and household sharing work?
- Which capabilities are excluded from the first release?
