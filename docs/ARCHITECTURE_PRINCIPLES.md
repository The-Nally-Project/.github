# Architecture Principles

These principles guide design across The Nally Project. They are constraints for engineering decisions, not claims that the implementation is already complete.

## 1. Separate intelligence from authority

Models may interpret, classify, retrieve, and propose. A separate permission and execution layer decides whether an action is authorised. Model output alone never grants permission.

## 2. User-controlled personal memory

Users must be able to inspect, correct, export, and request deletion of personal memory. Retention, processing, and backup behaviour must be disclosed.

## 3. Preserve provenance

Important remembered facts and decisions should retain source, timestamps, confidence, and update history. Derived summaries, embeddings, and graphs must not silently replace source evidence.

## 4. Handle time and change explicitly

Where relevant, distinguish when something was true from when the system learned or recorded it. Preserve superseded information when needed for audit and explain conflicts rather than silently choosing.

## 5. Least privilege

Every capability should declare the resources and actions it needs. Permissions should be scoped by resource, action, context, and duration, and should be revocable.

## 6. Hybrid execution

Use cloud resources for coordination and workloads that need them; use local execution where it improves privacy, latency, resilience, or resource efficiency. Security boundaries must remain enforceable on the device.

## 7. Modular intelligence

Nally-developed and external models should connect through documented, replaceable interfaces. Model choice must not be tightly coupled to memory ownership or permission enforcement.

## 8. Minimal models have bounded roles

A small model may handle narrow tasks such as intent classification, entity extraction, or routing. It must be evaluated for uncertainty and escalation, and must not authorise sensitive actions or independently modify canonical memory.

## 9. Interoperability

Prefer documented standards and stable interfaces. Treat connectivity, compatibility, and authority as separate concerns: a connected device is not automatically trusted or authorised.

## 10. Secure and observable execution

Tool calls should be constrained, validated, logged appropriately, and explainable to the user. Logs must avoid unnecessary exposure of personal or secret data.

## 11. Design for failure

Network loss, model unavailability, stale context, partial execution, and revoked permissions are normal conditions to handle explicitly. The system should report what happened and avoid claiming success without confirmation.

## 12. Accessibility and efficiency

Design for varied devices, bandwidth, languages, and compute budgets. Measure latency, memory use, energy use, and reliability rather than assuming cloud or local execution is always superior.
