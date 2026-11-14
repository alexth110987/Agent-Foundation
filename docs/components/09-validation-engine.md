# 09 — Validation Engine

**ID:** AF-09  
**Specification:** Draft; approved design decisions AF-77–AF-81 recorded; remaining design details open  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Validate definitions, inputs, state transitions, outputs, and evidence.

## Responsibilities
Validate schemas, configuration, inputs, preconditions, outputs, acceptance criteria; coordinate unit, component, integration, security, privacy, compatibility and smoke tests where authorized; preserve exact versions/environments.

## Inputs
Specifications, artifacts, test definitions, environment, expected results.

## Outputs
Gate report PASS/FAIL/BLOCKED/INDETERMINATE with evidence.

## Security and operational boundaries
Design review, feature, integration, security/privacy, compatibility, smoke and release gates remain separate. Smoke PASS is version/environment-specific, not release approval.

## Approved design decisions

The owner approved the following five design decisions on 2026-11-14. They are design decisions only; they do not approve implementation, production access, external integrations, technology selection, or release.

- **AF-77 — Approved design decision:** Every validation run must identify and use an explicit, versioned validation contract bound to its validation target and scope. The contract defines the applicable schema/rule versions, required inputs, preconditions, expected outputs, acceptance criteria, and any explicitly permitted tolerances. Results are valid only for the exact contract and target versions assessed; a changed contract, target, or relevant rule set requires a new validation. Missing, ambiguous, incompatible, or unverifiable contract information produces BLOCKED or INDETERMINATE, never PASS.
- **AF-78 — Approved design decision:** Test execution is permitted only in an explicitly authorized, isolated non-production environment using synthetic test data and no production credentials. Environment identity, isolation boundary, permitted resources, and any explicitly authorized network access must be established before execution. Production systems, live services, production configuration, and installed production components are out of scope. Loss of isolation, unexpected external side effects, or inability to verify the environment blocks execution; no automatic retry in another environment is permitted.
- **AF-79 — Approved design decision:** Test fixtures and expected-result oracles must be explicitly identified, versioned, and traceable to the validation contract and test case. Fixtures must use synthetic or separately authorized data, and their integrity and provenance must be verifiable before use. Fixture or oracle changes create a new version and invalidate results that depend on the prior version where applicability changes. Missing, modified, untrusted, or inconsistent fixtures/oracles produce BLOCKED or INDETERMINATE, not PASS.
- **AF-80 — Approved design decision:** Each validation run must produce a traceable evidence record bound to the exact target, contract, test case, fixture/oracle versions, environment, relevant tool/rule versions, and result. PASS, FAIL, BLOCKED, and INDETERMINATE remain distinct outcomes; unavailable or incomplete evidence cannot support PASS. Evidence must exclude secrets and unnecessary personal data and retain only minimum necessary diagnostic detail. Evidence retention, access, redaction, and deletion follow a separately reviewed and owner-approved audit/privacy policy; this decision creates no blanket retention period.
- **AF-81 — Approved design decision:** Validation gates must declare their prerequisites and dependencies explicitly, and each gate category must retain an independent result. A failed, blocked, indeterminate, or missing required prerequisite prevents dependent gates from passing. Coverage must identify the applicable test categories and the portions actually exercised; untested or unobservable areas remain explicitly uncovered or unknown. Feature, integration, security/privacy, compatibility, smoke, and release gates are not interchangeable. Smoke PASS applies only to the exact tested version and environment and is never release approval.

## Remaining open design details
Exact contract and evidence schemas; supported validation-rule formats; measurable isolation and resource boundaries; fixture provenance and integrity mechanisms; diagnostic minimization; evidence access and retention policy; gate dependency representation; and coverage measurement/reporting remain open.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
