# Quality & Testing Standard

**Status:** Draft v0.1  
**Scope:** Software, models, integrations, data systems and releases across The Nally Project.

## 1. Quality principles

Nally should optimise for correctness, security, privacy, reliability, accessibility, maintainability and user control—not feature count alone.

A feature is not complete because a demo works. It needs documented requirements, tests, failure behaviour and evidence appropriate to its risk.

## 2. Test layers

- Unit tests for pure functions, schemas, policy rules and transformations.
- Integration tests for databases, queues, model gateways and provider adapters.
- Contract tests for API schemas, capability manifests and version compatibility.
- End-to-end tests for user-visible workflows.
- Security tests for authentication, authorisation, tenant isolation, secrets and input handling.
- Privacy tests for minimisation, retention, export, deletion and logging.
- Model evaluations for quality, calibration, abstention, bias and robustness.
- Resilience tests for timeouts, retries, outages, cancellation and recovery.
- Accessibility tests for keyboard, screen-reader, contrast, captions and understandable status.

## 3. Sensitive-path requirements

Authentication, memory access, capability invocation, permission changes, revocation, export and deletion require negative tests as well as success-path tests.

Test that unauthorised actions fail closed, revocation takes effect at the enforcement point, and one tenant cannot retrieve another tenant's records through any index or cache.

## 4. Model evaluation

Maintain versioned evaluation sets with documented sources, licences and limitations. Report per-task metrics and failure examples. Keep held-out tests separate from training and tuning. Evaluate prompt injection, uncertainty, unsupported requests, tool selection and structured-output validity.

A benchmark score alone is not release approval.

## 5. CI requirements

Repositories should progressively adopt formatting and linting, type and schema checks, unit and integration tests, dependency and secret scanning, reproducible build checks, licence and provenance review where relevant, and artifact retention with release metadata.

Required checks must be visible in pull requests. Do not make a check required until stable and maintainable.

## 6. Review requirements

Substantial changes should include the problem and expected user benefit, scope and non-goals, architecture and data-flow impact, security and privacy impact, compatibility and migration plan, tests and acceptance criteria, and rollback or recovery plan for risky changes.

Changes to identity, authority, memory, retention or model routing require review by maintainers responsible for those areas.

## 7. Release gates

Before public release:
1. Requirements and known limitations are documented.
2. Required CI checks pass.
3. Critical security findings are resolved or block release.
4. Privacy and data-flow changes are reviewed.
5. Migration, backup and rollback plans are tested where relevant.
6. User-facing claims match verified behaviour.
7. Release notes and version information are published.
8. Support and vulnerability-reporting paths are available.

## 8. Incident and regression learning

Record incidents and important regressions with impact, root cause, remediation and follow-up actions. Focus on system improvement rather than blame. Add regression tests for confirmed failure modes.

## 9. Quality metrics

Track task success, defect escape rate, regression rate, crash/error rate, latency, recovery time, permission violations, privacy incidents, deletion completion, accessibility findings and support burden. Interpret metrics in context; do not incentivise unsafe automation.

## 10. Open decisions

- Repository-specific CI templates.
- Minimum coverage expectations by component.
- Model evaluation reporting format.
- Release signing and provenance approach.
- Severity definitions and release-blocking policy.
