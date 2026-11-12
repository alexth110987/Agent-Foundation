# Foundation Completion Plan

**Status:** Proposed working plan — not an approval to implement.  
**Last updated:** 2026-11-12  
**Project stage:** Stage 1 — Core foundation  
**Operational purpose:** Intentionally undefined  
**Canonical source:** This repository, branch main.

## 1. Purpose and boundaries

Provide an ordered route for completing the purpose-neutral foundation before any future bounded implementation proposal. This plan does not select an operational purpose, users, runtime, model, language, hosting, provider, or deployment model. It does not authorize executable code, infrastructure, integrations, credentials, ingestion, operational use, production testing, or release.

## 2. Current position

Four review batches examined 20 logical review areas; a subsequent static Foundation Consistency Review is recorded separately. These are documentation-review activities, not a completion percentage, implementation metric, or verification result.

| Batch | Areas | Result |
| --- | --- | --- |
| 1 | Conceptual architecture; component model; Core System; Workflow Management System; implementation readiness | Proposed designs reviewed; D-005 is approved but broader Core design remains open |
| 2 | Core identity; configuration; workflow template/index; validation; error/recovery | Workflow template/index exist; validation layers are documented; standalone error/recovery proposal has now been added |
| 3 | Logging; tool registry/governance; MMS; KMS; permissions | Logging and tool governance proposals added; D-006, D-007 and D-008 are approved logical baselines only |
| 4 | Decision log; changelog/version history; README/documentation map; error handling; validation | D-001–D-012 are registered; changelog through 0.36.0; validation remains documentation-only |

### Confirmed approved baselines

- D-005: Core read-only during normal operation; owner-led changes only.
- D-006: MMS logical architecture, exact approved blob 197ac0974f5ceea357f6ad846a299304dd13da9c.
- D-007: KMS logical architecture, exact approved blob c1dda460a8d02939db9ca9b16611be79e12019b6.
- D-008: Permission logical architecture, exact approved baseline blob 9e754143c289fd1e454de6ee3f61bec0e90f3305.
- D-009: Object-specific lifecycle tables, exact approved blob 473598af630b44b78910ab1df3b7f53bbc1a0802.
- D-010: Identity Contract, exact approved blob b01e5ff7c99f9a208c4424878e87ded5e4c4629d.
- D-011: Authorization Contract, exact approved blob 49e03d3d60f06ebf4b4ba8db92ac41349f09bc39.
- D-012: Provenance Contract (c7c3e93e3a14633f0400cd66a63172e4825dcd1a), Shared Lifecycle Contract (c8b9b5012c537cca2f99a2f9f3b80041cd4372bf), and Cross-System Interface & Ownership Matrix (98b4559d42a21bc769617a0cf2d0cce80f257d40).

These approvals apply only to exact documentation baselines. They do not authorize implementation, technology selection, runtime use, data ingestion, integration, testing, validation, or release.

### Current proposed designs

- docs/ERROR_HANDLING_ARCHITECTURE.md — cross-system failure, outcome, retry, recovery, reconciliation, and escalation design.
- systems/logging/LOGGING_ARCHITECTURE.md — event, integrity, access, privacy, retention, and failure design.
- docs/TOOL_GOVERNANCE.md — candidate registration, review, activation, suspension, and revocation lifecycle.
- docs/IMPLEMENTATION_READINESS.md — readiness framework, pending owner review.
- docs/JOINT_CONTRACT_REVIEW.md — proposed refinement register, pending owner review.
- docs/VALIDATION_STRATEGY.md — approved purpose-neutral validation and evidence policy (D-014); no validation system implemented.
- docs/DOCUMENTATION_VERSION_GOVERNANCE.md — approved document identity, versioning, approval and baseline policy (D-014); no registry or automation implemented.

The Tool Registry remains empty. No tools are approved, configured, validated, active, or connected.

## 3. Outstanding foundation work

| Priority | Work item | Current finding | Completion evidence |
| --- | --- | --- | --- |
| P0 | Cross-document consistency review | Static review and owner dispositions FCR-01–FCR-05 recorded in `docs/FOUNDATION_CONSISTENCY_REVIEW.md`; documentation corrections prepared | Verify annotations/status language and preserve exact approved baseline references |
| P0 | Owner review of cross-cutting proposals | Error Handling, Logging, Tool Governance are Proposed | Explicit owner decisions with exact artifact/version scope; unresolved issues retained |
| P1 | Validation strategy | Approved under D-014 against exact source blob `69398a8416a9f2d138d399084aa9e6a31a913b66`; no automated validation exists | Keep policy aligned with future purpose/context; define deferred risk thresholds, independence, environments, evidence retention and release authority before implementation |
| P1 | Documentation/version governance | Approved under D-014 against exact source blob `beb94648d9308fed79695ccb1d9184c0f0725c89`; no registry or automation implemented | Apply policy to future governed documents; decide deferred ID taxonomy, metadata, review roles/cadence, retention, tagging and automation as needed |
| P1 | Core and architecture review | Conceptual architecture, component model and broader Core design remain proposed | Authority/dependency/ownership review and explicit owner disposition |
| P1 | Workflow framework review | Shared template/index exist; WMS remains proposed | Validate lifecycle, execution authority, cancellation, partial outcomes and recovery consistency |
| P2 | D-012 unresolved questions | Five questions remain open | Explicit decisions or documented deferral; no assumptions |
| Deferred | Purpose and operating context | Intentionally undefined | Owner selects purpose, intended users, prohibited uses and context before purpose-specific design |
| Deferred | Implementation | No bounded implementation scope approved | Separate explicit authorization after readiness gates |

## 4. Recommended execution order

1. The static foundation consistency review and owner disposition of FCR-01–FCR-05 are recorded; exact D-012 baseline blobs remain unchanged.
2. D-014 approval of the Validation Strategy and Documentation and Version Governance policies is recorded against the exact source blobs; retain their deferred choices and do not infer implementation.
3. Obtain separate owner decisions on the proposed Error Handling, Logging, Tool Governance, WMS, broader Core, and architecture designs, and on unresolved cross-system questions.
4. Carry the PEP-at-each-protected-boundary requirement into future detailed interface design without selecting topology or implementation.
5. Only after purpose and operating context are selected, prepare a bounded implementation proposal.
6. Obtain separate explicit authorization for that exact implementation scope.

## 5. Minimum gate before first implementation

A first implementation proposal must not proceed until applicable items are explicit:

- Operational purpose, intended users, operating context, prohibited use, and accountable owner are approved.
- The first capability has a stable bounded scope and measurable acceptance criteria.
- Architecture and interfaces are reviewed against approved Core, Identity, Authorization, Permission, and relevant data/lifecycle contracts.
- Inputs, outputs, data classification, provenance, privacy, retention, and deletion requirements are defined.
- Initiator, executor, and approver roles are separated; least-privilege authorization is specified at the action boundary.
- External side effects, reversibility, idempotency, uncertain outcomes, and recovery are documented.
- Threat analysis, security, privacy, licensing, and supply-chain considerations are recorded.
- Isolated test strategy and acceptance criteria exist before implementation.
- Logging, evidence, access, retention, incident response, and escalation behavior are defined.
- Dependencies, compatibility, deferred items, rollback, suspension, and retirement are documented.
- The owner explicitly authorizes the specific implementation scope, version, and environment.

A successful smoke test is not feature verification, security validation, operational readiness, or release approval.

## 6. Consistency review findings

The static review and owner-approved dispositions are recorded in `docs/FOUNDATION_CONSISTENCY_REVIEW.md`. FCR-01–FCR-03 status/readiness corrections are documented; FCR-04 records the Core authority/document map; and FCR-05 records a future PEP boundary requirement. The three exact D-012 approved source blobs remain unchanged; current files carry only non-substantive post-approval status annotations where applicable. The Joint Contract Review register itself remains Proposed. No D-012 open question is closed.

## 7. D-012 open questions — retained

The following remain unresolved and are not closed by the new proposals:

1. Provenance requiredness by artifact, operation, and risk.
2. Concrete lifecycle transitions and deletion/recovery criteria.
3. Handling delayed, duplicate, reordered, and conflicting cross-system events.
4. Independent verification evidence and thresholds.
5. Tombstone and audit retention after deletion.

## 8. Durable handover

At the start of a future repository task:

1. Read AGENT.md, AGENTS.md, then docs/PROJECT_STATUS.md from live main, in that order.
2. Read this plan and the latest changelog entry.
3. Confirm the live branch head and base tree before preparing changes.
4. Inspect affected files and preserve exact approved baseline blobs.
5. Make one atomic commit for the task; re-fetch and verify every changed file, commit, branch head, workflow runs, and status checks.
6. Keep the project in Stage 1 until the owner explicitly approves a change in scope.

## 9. Status vocabulary

- **Proposed:** under consideration; not approved.
- **Approved:** explicitly accepted by the owner for a defined scope/version.
- **Implemented:** present in the repository or intended execution environment.
- **Verified:** checked against stated criteria with evidence.
- **Deferred / On hold:** intentionally inactive.
- **Rejected:** not selected.

A document may be implemented as documentation while its design remains Proposed. Neither state proves executable functionality.


## 10. Owner disposition — FCR-01 through FCR-05 — 2026-11-12

The owner approved all five dispositions in the Foundation Consistency Review. The three D-012 artifacts carry post-approval status annotations that preserve their exact approved source blob references. The Joint Contract Review remains Proposed and non-amending. The readiness framework distinguishes approved logical baselines from open implementation choices. The Core authority/document map is recorded, and the PEP owner/exact-context requirement is carried into future interface design. These are documentation-only changes; no implementation, runtime, integration, test execution, validation, or release is authorized.


## 11. Owner approval — D-014 — 2026-11-12

The owner approved the exact source blobs of the Validation Strategy (`69398a8416a9f2d138d399084aa9e6a31a913b66`) and Documentation and Version Governance (`beb94648d9308fed79695ccb1d9184c0f0725c89`) as purpose-neutral foundation policies. Approval is documentation-only; neither validation execution capability nor a document registry/versioning automation has been implemented. Deferred choices and all five D-012 open questions remain open. No implementation, runtime, testing, production validation, or release is authorized.
