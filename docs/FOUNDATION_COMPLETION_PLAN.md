# Foundation Completion Plan

**Status:** Proposed working plan — not an approval to implement.  
**Created:** 2026-11-11  
**Project stage:** Stage 1 — Core foundation  
**Operational purpose:** Intentionally undefined  
**Canonical source:** This repository, branch `main`.

## 1. Purpose

Provide a durable handover and an ordered route from the current foundation work to a separately authorized first implementation. This document preserves review progress and identifies what must be closed before implementation planning can be considered complete.

This plan does not select the agent's purpose, model, runtime, language, hosting, technology, provider, or deployment model. It does not authorize executable code, services, integrations, credentials, ingestion, operational use, tests against production, or release.

## 2. Current position

Twenty logical review areas have been reviewed in four batches. This is a documentation review count, not a completion percentage or verification result.

| Batch | Areas | Result |
| --- | --- | --- |
| 1 | Conceptual architecture; component model; Core System; Workflow Management System; implementation readiness | Drafts and proposed architecture reviewed; Core includes approved D-005 but broader design remains open |
| 2 | Core identity; configuration; workflow template/index; validation; standalone error/recovery architecture | Documentation reviewed; workflow template/index exist; no standalone error/recovery architecture was located |
| 3 | Logging; tool registry/governance; MMS; KMS; permissions | Logging is a placeholder; Tool Registry exists at `tools/TOOL_REGISTRY.md`; D-006, D-007 and D-008 are approved logical baselines only |
| 4 | Decision log; changelog/version history; README/documentation map; error handling; validation | D-001–D-011 recorded; changelog through 0.30.0; validation layers documented; error/recovery architecture remains a gap |

### Confirmed approved baselines

- D-005: Core read-only during normal operation; owner-led changes only.
- D-006: MMS logical architecture, exact approved blob `197ac0974f5ceea357f6ad846a299304dd13da9c`.
- D-007: KMS logical architecture, exact approved blob `c1dda460a8d02939db9ca9b16611be79e12019b6`.
- D-008: Permission logical architecture, exact approved baseline blob `9e754143c289fd1e454de6ee3f61bec0e90f3305`.
- D-009: Object-specific lifecycle tables, exact approved blob `473598af630b44b78910ab1df3b7f53bbc1a0802`.
- D-010: Identity Contract, exact approved blob `b01e5ff7c99f9a208c4424878e87ded5e4c4629d`.
- D-011: Authorization Contract, exact approved blob `49e03d3d60f06ebf4b4ba8db92ac41349f09bc39`.

These approvals are limited to their exact documentation baselines. They do not authorize implementation or runtime use.

### Owner approval awaiting repository registration

The owner approved the exact reviewed logical contract versions for D-012:
- `docs/PROVENANCE_CONTRACT.md` — pre-approval blob `c7c3e93e3a14633f0400cd66a63172e4825dcd1a`.
- `docs/LIFECYCLE_CONTRACT.md` — pre-approval blob `c8b9b5012c537cca2f99a2f9f3b80041cd4372bf`.
- `docs/CROSS_SYSTEM_INTERFACE_MATRIX.md` — pre-approval blob `98b4559d42a21bc769617a0cf2d0cce80f257d40`.

D-012 registration in `docs/DECISIONS.md`, status, and changelog is pending. This plan does not itself register D-012 or change the three documents' recorded status. Its exact scope and unresolved questions must be retained during registration. The GitHub commit-tree route was previously blocked by missing tree metadata in the connector wrapper; a direct Git reference/tree API route is now available for a safe atomic update and can be used in a separate registration task.

## 3. Outstanding foundation work

| Priority | Work item | Current finding | Completion evidence |
| --- | --- | --- | --- |
| P0 | Register D-012 | Owner approval exists in conversation; repository record is pending | Exact baseline SHAs recorded in decision log, status and changelog; post-commit verification |
| P0 | Error handling and recovery architecture | Requirements are scattered across WMS, MMS, KMS and Permissions; no standalone cross-system design found | Purpose-neutral architecture covering typed failures, safe stops, retry/idempotency, uncertain side effects, reconciliation, recovery and escalation |
| P0 | Logging architecture | `systems/logging/README.md` is a placeholder | Logical architecture for event taxonomy, correlation, integrity, access, minimization, retention, failure behavior and evidence |
| P1 | Tool governance review | `tools/TOOL_REGISTRY.md` exists; no tools registered or approved | Registry and governance reviewed against identity, authorization, lifecycle, validation and logging contracts |
| P1 | Architecture consistency review | Several proposed documents and approved contracts must be checked together | Traceable cross-document review with conflicts, owners and unresolved questions |
| P1 | Implementation-readiness framework | `docs/IMPLEMENTATION_READINESS.md` is proposed and owner review pending | Criteria for purpose/context, design, permissions, threat analysis, isolated tests, evidence and separate authorization |
| P2 | Documentation/version policy | README and changelog exist; dedicated versioning policy was not located | Clear rules for document IDs, versioning, immutable baselines, supersession, changelog and decision references |
| P2 | Validation strategy expansion | Six validation layers are documented; no automated validation exists | Risk-based evidence plan and independent validation requirements, kept separate from smoke tests and release approval |

## 4. Recommended execution order

1. **Secure the decision record:** register D-012 exactly as approved; do not expand its scope or close its listed open questions.
2. **Complete safety-critical cross-cutting designs:** write proposed Error Handling & Recovery and Logging architectures. Review them against WMS, MMS, KMS, Permission, Identity, Authorization, Provenance and Lifecycle contracts.
3. **Review tool governance:** clarify registration, review, activation, revocation, version pinning, permission scope, secret handling, side effects and tool-specific validation. No tool is enabled by this review.
4. **Run a foundation consistency review:** check ownership, authority, lifecycle, event/evidence semantics, failure recovery and validation across documents.
5. **Complete the readiness and documentation framework:** define artifact states, version/baseline rules, evidence requirements, and the owner review path.
6. **Prepare a bounded implementation proposal:** only after the purpose and operating context are explicitly selected. Identify one small first capability, dependencies, threat model, acceptance criteria, isolated test environment, rollback/recovery and operational owner.
7. **Obtain separate implementation authorization:** approval of this plan or any logical architecture is not implementation approval.

## 5. Minimum gate before first implementation

The first implementation proposal should not proceed until all applicable items below are explicit:

- Operational purpose, intended users, operating context and prohibited use are owner-approved.
- The first capability has a stable scope and named owner.
- Its architecture and interfaces are reviewed against approved Core, Identity, Authorization, Permission and relevant data/lifecycle contracts.
- Required inputs, outputs, data classification, provenance and retention are defined.
- Initiator, executor and approver roles are separated; least-privilege authorization is specified at the action boundary.
- External side effects, reversibility, idempotency, uncertain outcomes and recovery are documented.
- Threat analysis and privacy/security considerations are recorded.
- Acceptance criteria and isolated test strategy exist before implementation.
- Logging, evidence, access, retention and incident/escalation behavior are defined.
- Dependencies, deferred items, compatibility constraints and rollback are documented.
- The owner explicitly authorizes the specific implementation scope.

A successful smoke test is not feature verification, security validation or release approval. These remain separate gates.

## 6. Durable progress and handover

At the start of a future session:
1. Read `AGENT.md`, `AGENTS.md`, then `docs/PROJECT_STATUS.md` from live `main`, in that order.
2. Read this plan and the latest changelog entry.
3. Confirm the live branch head and check for concurrent changes before repository edits.
4. Continue with the first outstanding P0 item; do not repeat the completed 20-area review unless new changes require it.
5. Keep the project in Stage 1 until the owner approves the transition.

### Current handover snapshot

- Four review batches completed; 20 logical areas examined.
- No implementation or runtime test has been performed as part of these reviews.
- No operational purpose, runtime or technology has been selected.
- D-012 owner approval is awaiting repository registration.
- Next work: register D-012; then prepare Error Handling & Recovery and Logging architecture proposals.
- Preserve all existing approved baselines, exclusions, unresolved questions and separate authorization gates.

## 7. Status vocabulary

Use the repository's established terms precisely:
- **Proposed:** under consideration; not approved.
- **Approved:** explicitly accepted by the owner for a defined scope/version.
- **Implemented:** present in the repository or intended execution environment.
- **Verified:** checked against stated criteria with evidence.
- **Deferred / On hold:** intentionally inactive.
- **Rejected:** not selected.

A document may be implemented as documentation while its design remains proposed. Neither state proves executable functionality.
