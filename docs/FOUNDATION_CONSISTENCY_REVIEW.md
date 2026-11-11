# Foundation Consistency Review

**Status:** Completed documentation review; findings recorded, owner disposition pending.
**Review type:** Read-only assessment of the live `main` documentation set.
**Reviewed branch head:** `b559b91773fa94306f471dc69e535d434086e385`
**Reviewed on:** 2026-11-12
**Scope:** Purpose-neutral architecture, Core, workflow framework, MMS/KMS/Permissions, identity and authorization, provenance/lifecycle, cross-system coordination, error handling, logging, tools, validation, readiness, decisions, status, changelog and README.
**Change boundary:** This report does not amend approved baseline artifacts, resolve owner-reserved questions, approve proposals, or authorize implementation.

## 1. Executive result

The foundation is broadly aligned on its principal safety boundaries: Core rules remain owner-controlled; identity, authentication and authorization are distinct; permission is checked at protected action boundaries; each system owns its canonical records; memory and knowledge do not create authority; uncertain side effects remain indeterminate; and validation, verification and release approval are separate.

The review found **three confirmed documentation-status inconsistencies** and **two design/governance ambiguities** requiring disposition. These are documentation and governance findings, not defects in an implemented system. No runtime exists and no tests were run.

## 2. Findings

### FCR-01 — D-012 approved baselines still label themselves Proposed
- **Severity:** High — authoritative status ambiguity.
- **Classification:** Confirmed inconsistency.
- **Evidence:**
  - `docs/DECISIONS.md`, D-012, approves exact source blobs for `docs/PROVENANCE_CONTRACT.md`, `docs/LIFECYCLE_CONTRACT.md`, and `docs/CROSS_SYSTEM_INTERFACE_MATRIX.md` as documentation-only logical baselines.
  - `docs/PROJECT_STATUS.md` and `docs/FOUNDATION_COMPLETION_PLAN.md` correctly list those exact blobs as approved.
  - However, the current exact files retain headings/status text saying “Proposed” and “pending owner review”.
- **Risk:** Readers cannot determine from an artifact itself whether its design remains proposed or is approved within the narrow D-012 scope. The status vocabulary is internally contradictory even though the decision log and project status identify the approval.
- **Proposed disposition:** Preserve the exact D-012 blob references. Do not rewrite those approved artifacts as part of this review. Decide through an explicit owner-led status-annotation process whether to add non-substantive post-approval annotations that state “Approved logical baseline under D-012; exact baseline blob …; documentation only”, without changing the approved design content. Until then, treat D-012 and the exact baseline references in the decision log as the approval record, and flag the in-document status wording as stale.

### FCR-02 — Joint Contract Review contradicts D-012
- **Severity:** High — review scope/status conflict.
- **Classification:** Confirmed inconsistency.
- **Evidence:** `docs/JOINT_CONTRACT_REVIEW.md` opening status and §1 say the Provenance Contract, Shared Lifecycle Contract and Cross-System Interface Matrix remain Proposed pending separate owner approval. D-012 in `docs/DECISIONS.md` subsequently approved the exact baseline blobs for all three. The current `docs/PROJECT_STATUS.md` and changelog 0.33.0/0.32.0 also state that only the Joint Contract Review register remains Proposed.
- **Risk:** The refinement register misstates the current approval state and may cause reviewers to treat already-approved logical baselines as unapproved, or to misunderstand the register's own scope.
- **Proposed disposition:** Update only the register's status/scope wording after review: identify the three exact D-012 baselines as approved, while keeping the Joint Contract Review register itself Proposed and explicitly non-amending. Do not change any D-006–D-012 baseline or R-01–R-07.

### FCR-03 — Implementation-readiness framework contains stale contract blockers
- **Severity:** Medium — planning accuracy.
- **Classification:** Confirmed stale wording, with implementation details still unresolved.
- **Evidence:** `docs/IMPLEMENTATION_READINESS.md` §10 says the “Permission Service contract and identity model are unspecified”; §11 says to “Define shared identity, permission, provenance, and lifecycle contracts”. D-010 and D-011 approve logical Identity and Authorization Contract baselines, while D-012 approves logical Provenance and Shared Lifecycle Contract baselines. The Permission Service logical architecture is approved under D-008.
- **Risk:** The readiness plan obscures what has already been approved and can lead to redundant work or incorrect status reporting.
- **Proposed disposition:** Distinguish approved logical contract baselines from unresolved implementation-level choices. Keep provider, principal taxonomy, policy model, schemas/protocols, PEP/PDP deployment, operational policy, detailed provenance requiredness, lifecycle transition/deletion semantics, and independent verification evidence explicitly open. Update the readiness framework only as a separately reviewed documentation change.

### FCR-04 — Core instruction governance has an unresolved canonical-document boundary
- **Severity:** Medium — authority/document ownership ambiguity.
- **Classification:** Confirmed ambiguity; not a policy contradiction.
- **Evidence:** `AGENT.md` is explicitly the highest-authority project instruction document. `docs/CORE_SYSTEM.md` §4 says instruction governance is defined by “this document and future core instruction specification”; §6 defers the exact runtime instruction hierarchy. The repository tree contains `AGENT.md`, `core/IDENTITY.md`, `core/OPERATING_RULES.md`, and `core/CONFIGURATION.md`, but no separate core instruction specification.
- **Risk:** Future contributors may create a second source of core authority or mistake conceptual Core design text for an independently authoritative instruction file.
- **Proposed disposition:** Before adding any new core-instruction file, explicitly map the authority relationship: project-development instructions in `AGENT.md`; approved agent-level policy/identity/operating rules in designated Core records; and platform/runtime instruction precedence deferred until a target runtime is selected. Any new document should be subordinate and cross-referenced, not silently elevate itself.

### FCR-05 — Adapter and enforcement-point responsibility needs one explicit interface boundary
- **Severity:** Medium — future implementation interface ambiguity.
- **Classification:** Design ambiguity; no demonstrated contradiction in the approved logical contracts.
- **Evidence:**
  - `docs/ARCHITECTURE.md` §5 says tool adapters receive an authorization result and scoped parameters before protected actions.
  - `docs/COMPONENT_MODEL.md` diagram and §3 say the adapter requests/checks permission and authorization is enforced at the point of action.
  - `docs/CROSS_SYSTEM_INTERFACE_MATRIX.md` assigns PEP enforcement to each protected boundary, while the approved D-011 Authorization Contract defines bounded Permit/PEP semantics.
- **Risk:** An implementation could incorrectly assume that a prior orchestrator-level check is sufficient, or could leave unclear whether the adapter, downstream service, or both are enforcement points.
- **Proposed disposition:** In future detailed interface design, identify each protected side-effect boundary and its PEP owner. Require exact-context comparison immediately before the side effect; any downstream independent boundary must enforce its own applicable controls. Do not select a process topology or duplicate enforcement implementation in this foundation review.

## 3. Cross-cutting consistency observations

The following areas were checked and no direct contradiction was identified in the reviewed documents:

- **Identity and authorization:** D-010 and D-011 preserve identification/authentication/authorization separation, distinct initiator/executor/approver roles, bounded authorization context, freshness/revocation and fail-closed enforcement.
- **System ownership:** MMS, KMS, Permission, provenance and lifecycle materials consistently state that each owner retains authority over its own canonical records; derivatives do not inherit source ownership.
- **KMS-to-MMS:** Retrieval does not confer memory-write permission; MMS must independently evaluate eligibility, authorization and retention.
- **Lifecycle and recovery:** Owner-local state changes, partial/indeterminate outcomes, idempotency/replay concerns and reconciliation are consistently treated as unresolved design obligations rather than completed runtime behavior.
- **Error handling and logging:** Both distinguish process status, transaction outcome, verification and recovery; neither treats acknowledgement as proof of durable effect. Logging failure policy remains event-class/context dependent.
- **Tools:** Tool Governance and the Tool Registry agree that no tools are currently approved, configured, validated, active or connected.
- **Validation:** The six validation layers are distinct; no automated validation or runtime tests are claimed. Smoke-test evidence does not imply feature verification or release approval.
- **Workflow framework:** The shared template contains the required workflow elements, and the index distinguishes definition lifecycle from per-run execution lifecycle. No operational workflow is registered.

These observations are documentation-level consistency checks only, not verification of executable behavior.

## 4. Open questions retained

The review does not close the five D-012 questions:
1. Provenance requiredness by artifact, operation and risk.
2. Concrete lifecycle transitions and deletion/recovery criteria.
3. Delayed, duplicate, reordered and conflicting cross-system events.
4. Independent verification evidence and thresholds.
5. Tombstone and audit retention after deletion.

Other purpose/runtime-dependent decisions also remain deferred, including principal taxonomy/provider, policy technology, interface schemas/protocols, event delivery guarantees, logging retention, tool risk classification, operational objectives and deployment topology.

## 5. Recommended disposition order

1. Resolve FCR-01 and FCR-02 status wording without changing exact approved baseline content.
2. Refresh FCR-03 readiness language to distinguish approved logical contracts from unresolved implementation details.
3. Confirm the Core authority/document map before creating any additional instruction document.
4. Carry FCR-05 into future bounded interface design; do not implement it now.
5. After owner disposition, update the Foundation Completion Plan and status record. Keep the project in Stage 1 and retain all implementation, runtime, integration, testing, validation and release gates.

## 6. Review limitations

This was a static documentation review of the live `main` branch. It did not execute code, validate links automatically, run tests, assess a deployed system, select technologies, or independently verify operational controls. Findings describe the reviewed documentation only. No approved baseline was modified by this review.
