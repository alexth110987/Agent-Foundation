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
| 4 | Decision log; changelog/version history; README/documentation map; error handling; validation | D-001–D-012 are registered; changelog through 0.35.0; validation remains documentation-only |

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

The Tool Registry remains empty. No tools are approved, configured, validated, active, or connected.

## 3. Outstanding foundation work

| Priority | Work item | Current finding | Completion evidence |
| --- | --- | --- | --- |
| P0 | Cross-document consistency review | Static review recorded in `docs/FOUNDATION_CONSISTENCY_REVIEW.md`; findings await owner disposition | Resolve documented status conflicts and ambiguities without changing approved baselines |
| P0 | Owner review of cross-cutting proposals | Error Handling, Logging, Tool Governance are Proposed | Explicit owner decisions with exact artifact/version scope; unresolved issues retained |
| P1 | Validation strategy | Six validation layers documented; no automated validation exists | Risk-based validation plan, independent evidence, exact-version traceability, separate release approval |
| P1 | Documentation/version governance | README, decisions and changelog exist; dedicated policy not yet established | Rules for document identity, versioning, immutable baselines, supersession, review and evidence |
| P1 | Core and architecture review | Conceptual architecture, component model and broader Core design remain proposed | Authority/dependency/ownership review and explicit owner disposition |
| P1 | Workflow framework review | Shared template/index exist; WMS remains proposed | Validate lifecycle, execution authority, cancellation, partial outcomes and recovery consistency |
| P2 | D-012 unresolved questions | Five questions remain open | Explicit decisions or documented deferral; no assumptions |
| Deferred | Purpose and operating context | Intentionally undefined | Owner selects purpose, intended users, prohibited uses and context before purpose-specific design |
| Deferred | Implementation | No bounded implementation scope approved | Separate explicit authorization after readiness gates |

## 4. Recommended execution order

1. Conduct a foundation-wide consistency review across approved and proposed documents without changing approved baselines.
2. Identify and record conflicts, missing definitions, ambiguous ownership, stale status statements, and unresolved decisions.
3. Refine proposed validation and document/version governance frameworks.
4. Obtain owner decisions on proposals and unresolved cross-system questions.
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

## 6. Consistency review findings\n\nThe static review is recorded in `docs/FOUNDATION_CONSISTENCY_REVIEW.md`. It identified status conflicts between D-012 and the in-document status wording of its three approved artifacts; stale status statements in the Joint Contract Review and Implementation Readiness documents; an unresolved Core instruction-document boundary; and a future PEP/tool-adapter boundary requiring clarification. Findings are not silently resolved here. Exact approved baselines remain unchanged pending explicit owner disposition.\n\n## 7. D-012 open questions — retained

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
