# Shared Lifecycle Contract — Proposed

**Post-approval status annotation (2026-11-12):** **Approved logical baseline under D-012.** Exact approved baseline blob SHA: c8b9b5012c537cca2f99a2f9f3b80041cd4372bf. This annotation records the later owner approval without changing the exact approved baseline content or scope. Any legacy “Proposed” or “pending owner review” wording retained below belongs to the approved baseline snapshot and is superseded for current status interpretation by this annotation. Implementation remains unauthorized.

**Status:** Proposed shared logical contract — pending owner review.  
**Implementation:** Documentation only. No lifecycle engine, event service, retention service, reconciliation worker, or runtime exists.  
**Authority:** Subordinate to `AGENT.md`, approved Core rules, and the exact approved logical architecture baselines D-006, D-007, and D-008. This proposal does not amend those decisions or authorize implementation.

## 1. Purpose

Define a purpose-neutral shared contract for lifecycle state, transitions, ownership, authorization, provenance, evidence, and recovery across components that create, maintain, use, suspend, supersede, expire, withdraw, or delete managed objects.

The contract establishes interoperable principles without requiring one shared lifecycle engine or imposing identical state machines on every component.

## 2. Scope and exclusions

This proposal applies conceptually to managed objects and their governed derivatives, including knowledge sources and versions, memory records, identity assertions, authentication contexts, authorization approvals and decisions, provenance references, indexes, copies, and derived artifacts.

It does not select an operational purpose, user model, data domain, deployment context, universal state machine, event schema, identifier format, API, protocol, serialization, lifecycle engine, event bus, database, scheduler, retention service, technology, retention period, deletion deadline, legal-hold rule, backup purge window, identity provider, authentication mechanism, policy language, approval role, or runtime topology.

System-specific architecture and approved policy remain authoritative for each object type. A component-specific rule may be stricter. Not every object type must implement every state or transition described here.

## 3. Governing principles

1. **Explicit ownership:** Every managed object has one authoritative owner responsible for lifecycle state and authoritative mutations.
2. **State is not authority:** A lifecycle state does not itself grant permission to read, modify, disclose, execute, or delete an object.
3. **Authorized transitions:** Every protected transition requires a valid, scoped authorization decision at the point of action; required human approval is a prerequisite, not a permit.
4. **No implicit activation:** Registration, ingestion, receipt, validation, or successful storage does not automatically mean an object is approved, trusted, active, or usable.
5. **Purpose and scope continuity:** Project, tenant, subject, purpose, classification, and access restrictions remain attached to objects and derivatives as applicable.
6. **Version and history awareness:** Corrections and changes preserve distinctions between versions and historical events where retention policy requires it.
7. **Provenance continuity:** Lifecycle events preserve or explicitly qualify provenance and actor attribution; missing evidence is not fabricated.
8. **Least privilege and minimization:** Lifecycle operations expose and retain only information needed for their approved purpose.
9. **Fail closed at protected boundaries:** Unknown, invalid, stale, revoked, or unverifiable authority blocks the protected transition. Uncertainty remains visible.
10. **No cross-owner mutation:** One system may request a lifecycle action from another owner but cannot directly mutate that system's authoritative records.
11. **Honest completion:** Requested, accepted, authorized, started, applied, reconciled, verified, and completed are distinct outcomes.
12. **Controlled recovery:** Partial failure remains explicit and recovers through bounded, idempotent steps or manual review; it is never reported as success.

## 4. Logical terms

| Term | Meaning |
| --- | --- |
| Managed object | A record, assertion, source, version, decision, approval, artifact, or other item governed by a lifecycle |
| Authoritative owner | Component/system responsible for the object's canonical state and authorized mutations |
| Lifecycle state | Current governed condition of an object |
| Transition | Validated state change caused by an authorized event or operation |
| Lifecycle event | Traceable statement that a transition or relevant action was requested, attempted, applied, or verified |
| Derivative | Copy, excerpt, index entry, cache, summary, memory record, or other artifact derived from an object |
| Reconciliation | Comparing related objects and required actions across owners to establish whether obligations are complete |
| Tombstone / withdrawal marker | Retained status/reference indicating an object is withdrawn, deleted, unavailable, or otherwise ineligible, subject to policy |
| Completion evidence | Evidence that the required transition and its defined side effects were applied and verified |

These are logical terms, not a final schema.

## 5. Ownership and system boundaries

| Domain / object | Authoritative owner | Lifecycle responsibility |
| --- | --- | --- |
| Core instructions and approved policy | Core governance / owner-controlled repository | Controlled proposal, review, explicit owner approval, versioning, and recorded change history |
| Workflow definitions and execution state | Workflow Management System | Definition review/activation/versioning separately from each run's execution lifecycle |
| Knowledge sources, source versions, extracted passages, review/trust state, and KMS indexes | Knowledge Management System | Registration, ingestion, review, versioning, freshness, quarantine, withdrawal, expiry, deletion, and index consistency |
| Durable memory records and MMS-managed derivatives | Memory Management System | Eligibility, authorized write, retrieval eligibility, correction, supersession, expiry, export, deletion, and reconciliation |
| Principal records and identity assertions | Future Identity Authority (not implemented) | Registration, authentication status, attribute/version changes, suspension, revocation, reactivation, and retirement |
| Approval records and authorization decisions | Future approval / Permission Service responsibilities (not implemented) | Approval validity and revocation; decision validity, policy attribution, and withdrawal/re-evaluation |
| Provenance references and lineage for a component's own artifacts | Component owning the artifact, using the shared Provenance Contract | Maintain attributable origin/handling references and lifecycle continuity for its own artifacts |
| Operational and audit events | Future Logging / audit responsibility (not implemented) | Event integrity, access, retention, and authorized disposal under separately approved policy |

The table assigns logical responsibility only. It does not assert that any future service or authority currently exists. A receiving system may hold a reference or controlled derivative, but that does not transfer ownership of the source object. Cross-system actions require an explicit request to the authoritative owner and a separately authorized operation at that boundary.

## 6. Common lifecycle model

The following are reusable logical states, not a mandatory universal state machine:

| State | Meaning | Typical restriction |
| --- | --- | --- |
| Candidate | Identified but not yet accepted into governed use | Not eligible for ordinary use |
| Pending review / authorization | Awaiting required review, validation, or authorization | No protected activation or use |
| Active | Meets applicable conditions for its declared use | Only within current scope, policy, and validity |
| Suspended | Temporarily disabled pending a condition or review | Protected use blocked unless a separately approved narrow exception applies |
| Quarantined | Isolated because provenance, integrity, scope, trust, conflict, or correctness is uncertain | Excluded from ordinary use and disclosure |
| Superseded | Replaced by a newer or corrected version | Not presented as current; historical access remains policy-controlled |
| Expired | Validity or retention condition elapsed | Excluded from ordinary use; retention/disposal rules apply |
| Withdrawn / revoked | Explicitly made no longer valid or available by responsible authority | New protected use blocked; dependent artifacts assessed |
| Pending deletion | Authorized disposal accepted but required deletion/reconciliation incomplete | Excluded from ordinary use; completion not claimed |
| Deleted | Required deletion conditions for defined scope verified | Not available for ordinary use; residual metadata only as policy permits |

A system may define additional states or use a subset. It must document the mapping and must not collapse materially different states such as Suspended, Quarantined, Expired, Withdrawn, and Deleted into one ambiguous flag.

## 7. Transition contract

Every transition specification defines source and target states, initiating event, authorized requester, authoritative owner, exact object/version/scope, preconditions, validation and evidence, authorization and approval prerequisites, effects on references/derivatives/indexes/caches/replicas/backups, effective and recorded time, version semantics, idempotency, expected output, completion evidence, failure states, and recovery/escalation path.

Common transition families:

1. **Create / register:** establish a unique object reference and owner; registration is not approval or activation.
2. **Validate / review:** evaluate declared criteria and record outcome/evidence; validation is not a permission grant.
3. **Activate:** make eligible for a defined use only after mandatory policy, provenance, integrity, review, retention, and authorization conditions are met.
4. **Change / correct:** create a distinguishable revision or correction event; preserve prior history where required and re-evaluate affected eligibility.
5. **Version / supersede:** identify the new version and its relation to the prior version; do not silently present stale versions as current.
6. **Suspend / quarantine:** block ordinary use while retaining enough state to investigate and recover under policy.
7. **Reactivate / release quarantine:** require fresh validation of the conditions that caused restriction; prior approval/status does not automatically revive.
8. **Withdraw / revoke:** make the object ineligible for future protected use and assess dependent artifacts.
9. **Expire:** apply the declared validity/retention condition; expiry is not silently extended.
10. **Delete:** execute approved disposal policy across the owner's authoritative stores and defined derivatives; maintain Pending Deletion until completion criteria pass.
11. **Reconcile:** compare expected versus actual state across owners and derivatives; unresolved work remains pending/degraded.

No transition is implied merely because it appears here. Each object type requires its own allowed-transition table before implementation.

## 8. Authorization and approval boundary

Every protected lifecycle transition is a distinct operation for authorization. Permission to read or register does not imply permission to activate, correct, export, withdraw, or delete. The requesting principal, initiating component, actual executing component, authoritative owner, and approver remain distinguishable where applicable.

Identity assertions are not authorization. Identity suspension, revocation, attribute changes, or delegation changes require fresh PDP evaluation where material. A Permit must match the exact principal/executor, action, object/version, purpose, project/tenant/subject, environment, constraints, PEP/audience, policy context, and validity. Approval-required outcomes pause the transition; approval must be attributable, bounded, valid, and followed by fresh PDP evaluation. A PEP may impose stricter local conditions but cannot broaden a central decision.

A decision authorizes only the bounded operation; it does not prove the transition occurred or succeeded. If authorization, approval, identity, scope, policy version, or revocation status cannot be established, block the protected transition.

This section aligns this proposal with the approved logical Identity Contract (D-010) and Authorization Contract (D-011), while remaining subordinate to D-008 and those exact approved baselines. It does not amend D-008, D-010, or D-011, nor implement the PDP/PEP model.

## 9. Provenance, versioning, and event evidence

Record as applicable and permitted: unique event/correlation references; object identifier, owner, version, prior/new state; event type and request/effective/recorded times; initiating principal, submitter, processing component, and executor as distinct roles; authorization/approval references; policy, validation, provenance, and integrity evidence references; affected derivative set or explicit pending impact assessment; result (requested, accepted, rejected, attempted, applied, failed, partial, reconciled, verified); reason and recovery reference.

Do not store raw credentials, unnecessary source payloads, or sensitive personal data in general lifecycle/audit records. Exact fields, integrity mechanisms, event immutability, access, and retention remain future decisions.

A lifecycle event is evidence of a reported event, not automatic proof that every side effect occurred. Verification checks actual authoritative state against defined acceptance criteria.

## 10. Retention, withdrawal, and deletion

Each object class requires an approved retention/disposal rule before implementation. No universal duration or legal-hold policy is selected. Expiry, withdrawal, and deletion are distinct: expired or withdrawn objects may require retention under policy; Deleted means the defined deletion criteria passed.

Deletion scope explicitly considers authoritative records, versions, extracted passages, indexes, caches, replicas, backups, copied excerpts, memory derivatives, and audit/provenance metadata as applicable. Each authoritative owner remains responsible for its own data and derivatives. A request to another owner is not proof of deletion.

Some metadata may need restricted retention to evidence an operation; this must be justified and governed separately. Source deletion does not allow another system to claim all derivatives were deleted. Each owner reports its own verified outcome. Where policy permits a tombstone, retain only the minimum needed to prevent accidental reuse and support reconciliation.

Deletion is not complete until all mandatory checks pass. If a required store or derivative cannot be checked, report incomplete/pending or a defined degraded state.

## 11. Cross-system coordination and reconciliation

1. Requester identifies target, authoritative owner, transition, purpose, scope, and expected effects.
2. Requester obtains authorization for its own coordination action where required.
3. Authoritative owner independently validates request, object state/version, policy, provenance, and authorization.
4. Owner accepts, rejects, or returns a typed pending/indeterminate result.
5. Owner performs only its authorized local transition and records the actual outcome.
6. Owner identifies affected references/derivatives requiring notification or reconciliation, subject to access policy.
7. Each downstream owner evaluates and performs its own separately authorized local action.
8. Reconciliation compares expected obligations with owner-reported and independently verified outcomes.
9. Coordinator reports complete only when all mandatory obligations have evidence; otherwise reports pending, partial, failed, or degraded with unresolved items.

Coordination does not create one broad permission. One system cannot rewrite another's records or claim completion on its behalf.

## 12. Failure handling and recovery

| Failure | Required behavior |
| --- | --- |
| Owner unavailable | Do not mutate its authoritative state; report pending/unavailable |
| Authorization or identity unavailable/invalid | Block protected transition; preserve failure category |
| Validation, provenance, or integrity uncertain | Keep pending or quarantine according to object-specific policy |
| Duplicate or replayed event | Detect using future idempotency/correlation semantics; avoid unintended duplicate effects |
| Version/state conflict | Reject or return conflict; do not silently overwrite newer state |
| Partial write or multi-step failure | Record partial outcome; reconcile actual state before retry |
| Derivative/index/cache update fails | Keep affected object unavailable where disclosure could violate policy; report degraded/pending |
| Revocation/withdrawal propagation uncertain | Block affected future protected use under applicable policy; do not claim propagation complete |
| Deletion incomplete | Keep Pending Deletion; continue bounded reconciliation; do not report Deleted |
| Audit/evidence recorder unavailable | Apply separately approved event-class policy; never bypass authorization; block consequential transitions when required evidence cannot be preserved |
| Recovery is not idempotent or safe | Stop automatic retry and escalate for controlled review |

Retries are bounded and use explicit idempotency/duplicate-suppression semantics defined for the object and operation. Do not blindly repeat external or destructive side effects. Recovery preserves the original failure and distinguishes repair from successful first execution.

## 13. Validation requirements

Before implementation authorization, define isolated acceptance tests for allowed/prohibited transitions; invalid, missing, stale, revoked, or mismatched authorization/approval; identity changes requiring PDP re-evaluation; state/version conflicts and concurrent updates; duplicate/replayed/out-of-order/partially delivered events; provenance continuity and actor separation; project/tenant/subject/classification isolation; activation only after mandatory review; suspension/quarantine/release/expiry/withdrawal/revocation; derivative impact and cross-owner reconciliation; deletion across authoritative stores and derivatives; event/audit integrity, minimization, access and secret leakage; failure recovery, idempotency, and truthful completion.

Use isolated environments and synthetic or explicitly authorized data. Documentation is not test evidence. Feature tests, security/privacy validation, operational smoke tests, compatibility tests, and release approval remain separate.

## 14. Deferred decisions and readiness gates

Unresolved: purpose/users/context; object-specific state machines and transition matrices; identity authority, principal taxonomy, authentication and delegation; policy model, authorization schema, approval roles and revocation propagation; event schema, identifiers, ordering, integrity and correlation; retention, legal holds, backup, deletion and audit policy; consistency guarantees, reconciliation cadence, availability and recovery objectives; component topology, runtime, technologies and operational ownership.

Readiness sequence:
1. Owner review of this shared logical contract.
2. Reconcile object-specific lifecycle tables with approved architectures and the approved logical Identity (D-010) and Authorization (D-011) Contracts, alongside the proposed Provenance Contract.
3. Define data ownership, event/evidence, retention, deletion, and recovery rules for each object class.
4. Define threat model, acceptance criteria, and isolated validation plan.
5. Prepare a bounded implementation proposal.
6. Obtain separate explicit implementation authorization.

This proposal does not approve implementation, technology selection, integration, data ingestion, operational use, or release.

## 15. Related records

- [Project Status](PROJECT_STATUS.md)
- [Decision Log](DECISIONS.md)
- [Provenance Contract](PROVENANCE_CONTRACT.md)
- [Memory Management System Architecture](../systems/memory/MEMORY_ARCHITECTURE.md)
- [Knowledge Management System Architecture](../systems/knowledge/KNOWLEDGE_ARCHITECTURE.md)
- [Permission System Architecture](../systems/permissions/PERMISSION_ARCHITECTURE.md)
- [Identity Contract](../systems/permissions/IDENTITY_CONTRACT.md)
- [Authorization Contract](../systems/permissions/AUTHORIZATION_CONTRACT.md)
- [Workflow Management System](../workflows/WORKFLOW_MANAGEMENT.md)

## Joint refinement — transition execution, events, and recovery

The common transition contract is refined by the following requirements. They apply as logical semantics only; object-specific transition tables and approved policies remain authoritative.

### Separate state, process, transaction, and verification

A lifecycle implementation must not overload one status field to represent different dimensions. Where applicable, distinguish:

- **Object lifecycle state:** the governed condition of the object (for example Candidate, Active, Suspended, Quarantined, Withdrawn, or Deleted).
- **Process status:** the progress of the operation (for example not started, running, waiting, or terminated).
- **Transaction outcome:** whether the requested change was rejected, not applied, applied, partially applied, or has an unknown outcome.
- **Verification result:** whether actual state and required side effects were independently checked and found to pass, fail, or remain inconclusive.

A successful process termination is not proof of a successful transaction; an applied transaction is not proof that all side effects were verified.

### Event identity, time, ordering, and concurrency

- Use distinct logical references for the object, object version, transition request, lifecycle event, correlation, and causation. Their eventual representation remains undecided.
- Preserve event, effective, observed, and recorded times separately where available.
- Define per-object and per-operation duplicate/replay handling, idempotency scope, ordering expectations, and concurrency/version-conflict behavior before implementation.
- A delayed or out-of-order notification must not overwrite a newer authoritative state. The authoritative owner resolves version conflicts and reports the conflict rather than silently accepting stale state.
- Unknown event delivery or acknowledgement is indeterminate until the owner checks actual state; retry only after determining whether the original side effect occurred.

### Partial outcomes and owner-local evidence

- Each owner records its own request validation, authorization, attempt, applied state, side effects, and verification evidence.
- A coordinator records its coordination outcome separately from each owner's transaction and verification result.
- A multi-owner operation is complete only when every mandatory owner obligation has an attributable result and required verification evidence. Missing, inaccessible, or contradictory owner evidence leaves the aggregate pending, partial, failed, or indeterminate.
- Notifications and acknowledgements are not proof of mutation, deletion, derivative reconciliation, or disclosure eligibility.
- A derivative owner independently authorizes and executes its local correction, restriction, quarantine, withdrawal, or deletion. No cross-owner command grants blanket authority.

### Recovery and escalation

- Before retrying after timeout or interruption, inspect the authoritative object/version and known side effects to determine whether the action was not applied, applied, partially applied, or remains unknown.
- Retry automatically only where the operation has a defined idempotency strategy and its side effects are safe to repeat. Otherwise stop and escalate for controlled reconciliation.
- Preserve the original event and failure evidence; represent repair or compensation as a separate event linked by causation.
- Compensation is not assumed to be possible or safe. Destructive, external, irreversible, or privacy-sensitive side effects require the separately authorized recovery path.
- If required audit, authorization, identity, provenance, or owner evidence is unavailable, apply the operation-specific fail-closed rule; never invent completion evidence.

These refinements do not define universal service levels, retry counts, consistency guarantees, retention periods, legal holds, or recovery objectives. Those remain purpose- and object-specific decisions.
