# Initial Roadmap

**Status:** Directional roadmap; milestones are not promises or release dates.  
**Last reviewed:** 2026-11-02

## Stage 0 — Project foundation

- [x] Create the GitHub organisation.
- [x] Publish the organisation profile.
- [x] Publish the project charter, governance, contribution, conduct and security policies.
- [x] Establish the engineering documentation index and decision-record process.
- [ ] Establish repository-specific issue, proposal and release templates.

## Stage 1 — Core specifications

- [x] Define user, agent and device identity boundaries.
- [x] Specify the authority and permission model.
- [x] Specify memory records, provenance, retention, export and deletion.
- [x] Define the capability and integration interface.
- [x] Document intelligence orchestration and model boundaries.
- [x] Document data governance, privacy and initial threat boundaries.
- [x] Define quality, testing and release principles.
- [x] Record the accepted model strategy in ADR-0001 and publish the model research plan.
- [ ] Review specifications with maintainers and convert unresolved questions into decision records.

## Stage 2 — Model research

- [x] Define the narrow Nally Mini role and exclusions.
- [x] Define candidate model approaches and experiment size bands.
- [x] Define dataset provenance, licensing and annotation requirements.
- [x] Define evaluation metrics and release gates.
- [x] Establish a staged research plan for provider comparison, evaluation, Nally Mini, model adaptation and pretraining feasibility.
- [ ] Finalise task taxonomy, risk tiers and representative test cases.
- [ ] Establish provider/model review and licence/data-handling register.
- [ ] Build deterministic and pretrained baselines.
- [ ] Build a reproducible evaluation harness.
- [ ] Compare external and open-weight candidates on identical tasks.
- [ ] Benchmark Nally Mini candidates for quality, calibration, latency, memory and energy.
- [ ] Publish a first model research report and stage-gate recommendation.

## Stage 3 — Software prototype

- [ ] Select the initial implementation stack through documented decisions.
- [ ] Build a narrow, testable orchestration prototype.
- [ ] Separate planning, approval, execution and audit.
- [ ] Implement inspectable user memory controls.
- [ ] Add failure handling and observability.
- [ ] Run security, privacy and usability reviews.

## Stage 4 — Interoperability

- [x] Define the cross-platform adapter strategy and protocol evaluation criteria.
- [ ] Select initial integrations based on user value and documented APIs.
- [ ] Implement scoped, revocable permissions.
- [ ] Test local and cloud execution paths.
- [ ] Publish integration guidance and compatibility tests.

## Stage 5 — Expansion

- [ ] Improve portability and local-first options.
- [ ] Evaluate additional languages and accessibility needs.
- [ ] Explore broader device and environment integrations.
- [ ] Establish sustainable maintainer and release processes.

## Release discipline

A stage is complete only when its acceptance criteria, tests, documentation and known limitations are published. A checked item means its documentation or design work exists; it does not imply an implementation is complete. Roadmap items may be reordered when evidence or security findings justify a change.
