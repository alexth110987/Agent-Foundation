# Validation Strategy — Proposed

**Status:** Proposed foundation policy; pending owner review.  
**Version:** 0.1.0  
**Last updated:** 2026-11-12  
**Scope:** Purpose-neutral validation policy only. No validation framework, test runner, test suite, or runtime process is implemented or authorized by this document.

## 1. Purpose

Define a repeatable, risk-aware approach for checking foundation artifacts and any future, separately authorized capabilities. Validation must produce evidence tied to the exact artifact, version, scope, and environment examined. Validation is not a substitute for owner approval. A test passing does not authorize implementation, external operation, or release.

## 2. Governing principles

1. Define acceptance criteria before executing a check.
2. Validate the smallest useful unit first, then progress through applicable layers.
3. Match validation depth to impact, exposure, reversibility, data sensitivity, and failure consequence.
4. Keep test, smoke, feature, compatibility, security, privacy, licensing, operational-readiness, and release decisions distinct.
5. Record exact versions and environments. A result applies only to what was actually examined.
6. Fail closed where authorization, integrity, safety, or required evidence is absent.
7. Use isolated environments and synthetic or explicitly authorized data. Never use production systems or data as a default test fixture.
8. Preserve failures, limitations, blocked checks, and negative results; do not silently omit them.
9. Require independent review or verification when risk warrants it.
10. Do not claim verification without reproducible steps and retained evidence.

## 3. Validation layers

Apply only the layers relevant to the artifact and risk. A higher layer does not erase or imply a lower-layer result.

| Layer | Subject | Minimum evidence | Does not establish |
| --- | --- | --- | --- |
| L0 — Document and contract review | Requirements, decisions, architecture, workflow specifications | Review checklist, exact document version/commit, findings and dispositions | Executable behavior |
| L1 — Static and structural checks | Schemas, configuration examples, dependency declarations, repository structure | Tool/check version, inputs, output, exceptions | Runtime correctness |
| L2 — Unit/component tests | One isolated component or defined behavior | Test IDs, setup, exact component version, expected/actual results | End-to-end behavior |
| L3 — Workflow/feature tests | One workflow or capability against acceptance criteria | Scenario coverage, normal/negative/boundary cases, outputs and evidence | Compatibility or operational readiness |
| L4 — Integration/contract tests | Interactions and interface contracts between approved components | Exact participant versions, contract cases, event/order/error cases | Production behavior |
| L5 — Security, privacy and permission validation | Trust boundaries, authorization, data handling, secrets, abuse and failure paths | Threat/risk scope, test cases, findings, independent review where warranted | General feature completeness |
| L6 — Compatibility and environment validation | Supported versions, platforms, dependencies and configuration combinations | Compatibility matrix, exact environment, tested combinations and exclusions | Untested combinations |
| L7 — Operational smoke checks | Minimal startup/health and bounded operational checks in an isolated target environment | Exact build/runtime/environment, health criteria, logs and result | Feature completeness, security approval, production readiness or release approval |
| L8 — Operational readiness and release review | Monitoring, recovery, rollback, support, licensing, evidence completeness and release risks | Separate readiness checklist and explicit owner decision | Authorization beyond the approved release scope |

A smoke test is a narrow operational check. It is never a proxy for feature tests, compatibility validation, security review, licensing review, or release approval.

## 4. Risk-based validation depth

Risk classification is a planning aid, not an automatic permission grant. The owner must approve applicable risk criteria when a real operating context exists.

Consider at least impact and scope of an incorrect or unauthorized action; external exposure and trust-boundary crossings; data sensitivity and volume; irreversibility and recoverability; privilege level and escalation potential; dependency criticality and supply-chain exposure; and uncertainty, novelty, and evidence quality.

For a low-impact, reversible, isolated change, focused review and targeted checks may be proportionate. Changes affecting identity, authorization, secrets, data retention/deletion, external side effects, or core policy require deeper negative-path testing, independent review, and explicit owner disposition. Critical or irreversible effects require a separately approved validation plan before any implementation proposal is considered.

No risk tier, threshold, or test-depth matrix is finalized by this proposal. Those choices depend on the future purpose and operating context.

## 5. Validation workflow

1. **Identify the subject:** assign a stable artifact/component/workflow ID and exact version or commit.
2. **Define scope:** state what is included, excluded, and explicitly not being tested.
3. **Assess risk:** record impact, exposure, reversibility, data sensitivity, and required independence.
4. **Set acceptance criteria:** make expected outcomes observable and unambiguous before testing.
5. **Prepare isolation:** identify environment, dependencies, synthetic/authorized fixtures, access controls, and cleanup.
6. **Select layers and cases:** include normal, boundary, negative, failure, recovery, and permission-denial cases as applicable.
7. **Execute and capture:** retain procedures, tool versions, timestamps, outputs, logs, and deviations without exposing secrets.
8. **Review results:** distinguish observed facts, interpretation, limitations, and unresolved findings.
9. **Disposition findings:** fix, accept with explicit owner-approved rationale, defer, or block. A failed or blocked criterion cannot be silently waived.
10. **Revalidate changes:** rerun affected checks when code, configuration, dependency, interface, policy, or environment changes.
11. **Report separately:** state test outcome, verification status, readiness status, and release decision as distinct conclusions.

## 6. Result vocabulary

- **PASS:** all stated acceptance criteria for this check were met on the exact tested subject and environment.
- **FAIL:** one or more acceptance criteria were not met.
- **BLOCKED:** the check could not be validly performed because a prerequisite, permission, environment, dependency, or required evidence was unavailable.
- **NOT RUN:** no execution was attempted.
- **NOT APPLICABLE:** a planned check genuinely does not apply, with a recorded rationale. Never use this to conceal a blocked or failed check.

Overall verification is a separate conclusion requiring applicable planned checks to be complete, findings to be dispositioned, and evidence to be reviewed. State residual limitations explicitly.

## 7. Evidence record

Each executed check should retain:

| Field | Required content |
| --- | --- |
| Validation ID | Stable unique identifier |
| Subject | Artifact, component, workflow or capability ID |
| Version | Exact document version, Git commit/blob, build or dependency versions as applicable |
| Scope | Included and excluded behavior |
| Risk | Rationale and applicable risk considerations |
| Environment | Isolated environment and relevant configuration; no secrets |
| Procedure | Reproducible steps, test case IDs and tool versions |
| Criteria | Expected results defined before execution |
| Outcome | PASS, FAIL, BLOCKED, NOT RUN or justified NOT APPLICABLE |
| Observed result | Actual behavior, deviations and failure details |
| Evidence | Stable repository reference or controlled evidence location |
| Reviewer | Executor and reviewer; independence rationale where required |
| Timestamp | Date/time and relevant timezone |
| Limitations | Untested paths, known gaps, assumptions and retest triggers |
| Disposition | Finding owner, decision, rationale and follow-up reference |

Evidence must be integrity-protected and access-controlled in proportion to its sensitivity. Do not commit credentials, personal data, private logs, or sensitive payloads to this public repository.

## 8. Independence and change impact

The person who authored a change should not be the sole reviewer of high-impact validation evidence. The degree of independence must be stated and justified; a future organizational model is not selected here.

Revalidation is required when a change could invalidate prior evidence, including changes to the tested artifact or approved baseline; dependencies, toolchains, runtime, configuration, or environment; interfaces, permissions, identity, policy, data classification, or lifecycle behavior; or acceptance criteria, risk assumptions, and operational boundaries.

A prior PASS is not transferable to a different version or environment without an explicit impact assessment and evidence.

## 9. Separate gates

Track these decisions independently:

1. **Document/design review:** is the specification coherent and approved for its stated design scope?
2. **Test execution:** what checks were run, against which exact version, and with what results?
3. **Feature verification:** do defined capabilities meet their acceptance criteria?
4. **Compatibility validation:** which exact combinations were tested and supported?
5. **Security/privacy/permission validation:** were relevant risks and boundaries examined with adequate evidence?
6. **Licensing and supply-chain review:** are the exact dependencies and distribution/use conditions acceptable?
7. **Operational readiness:** are monitoring, recovery, access, support and incident procedures adequate?
8. **Release approval:** has the owner explicitly authorized the exact artifact/version, scope, environment and release conditions?

No gate is implied by another. Passing tests is not release approval.

## 10. Current limitations and open decisions

- No runtime, test runner, automated validation, executable tests, or test evidence exists.
- No production or live-system testing is authorized.
- Risk thresholds, independence requirements, supported environments, evidence retention periods, and release authority details remain to be selected after purpose and operating context are defined.
- The five D-012 open questions remain open; this strategy does not resolve them.
- This document remains Proposed pending explicit owner review.

## 11. Review and change control

Review this proposal when the foundation architecture changes materially and before any bounded implementation proposal. Substantive changes to an approved future validation policy require owner authorization, a new document version/baseline, impact review, and a changelog entry.
