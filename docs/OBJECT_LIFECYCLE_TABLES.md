# Object-Specific Lifecycle Transition Tables — Proposed

**Status:** Proposed design artifact — pending owner review.  
**Implementation:** Documentation only. No state machine, lifecycle engine, event service, retention service, reconciliation worker, or runtime is implemented or authorized.  
**Authority:** Subordinate to `AGENT.md`, approved Core rules, and the exact approved logical architecture baselines D-006, D-007, and D-008. This document does not amend those decisions or approve the proposed Identity, Authorization, Provenance, Lifecycle, or Cross-System Interface contracts.

## 1. Purpose and interpretation

This document translates the shared Lifecycle Contract into initial owner-specific transition tables. It identifies plausible transition boundaries and minimum controls without selecting a universal state machine, schema, event protocol, technology, retention period, or operational purpose.

These are **candidate transitions for review**, not a complete authorization policy. A transition listed here is not automatically permitted. Each object owner must define exact state semantics, required evidence, authorization policy, retention/disposal rules, and acceptance criteria before any implementation proposal.

Terms:
- **Owner:** the single logical authority for the object's canonical state and authorized mutations.
- **Requester:** principal or component asking the owner to perform a transition.
- **Executor:** authenticated component performing the protected operation; distinct from requester where applicable.
- **Approval:** a required prerequisite where policy demands it; never a Permit.
- **Derivative owner:** the component that owns a copy, index, cache, excerpt, export, or other derived object.

## 2. Common transition controls

Every transition row is subject to all applicable controls below:

1. Identify the exact object, owner, version, project/tenant/subject, purpose, and requested operation.
2. Validate current state/version and reject stale or conflicting requests; do not silently overwrite.
3. Authenticate requester and executor independently where they differ. Do not infer identity from workflow context.
4. Obtain a fresh, bounded authorization decision for the actual operation at the enforcement boundary. Compare every applicable Permit dimension with the actual operation immediately before the side effect.
5. Obtain attributable, bounded approval where policy requires it, then perform fresh authorization evaluation. Approval is not permission.
6. Preserve provenance, actor separation, source/version references, restrictions, and uncertainty. Missing evidence remains explicitly unknown.
7. Record request, event, correlation/causation, effective/observed/recorded times, outcome, and minimum evidence references as permitted by policy.
8. Treat requested, accepted, authorized, attempted, applied, partially applied, reconciled, verified, and completed as distinct outcomes.
9. Use idempotent bounded retries only where proven safe. Discover and reconcile partial side effects before retrying.
10. A downstream notification is not proof of downstream action. Each derivative owner independently authorizes, acts, records, and verifies.
11. If authority, current eligibility, owner state, or required evidence is unknown, block protected use/disclosure and report pending, indeterminate, or degraded.
12. Deletion, retention, legal holds, backup treatment, and tombstone retention require separately approved object-class policy; no duration is implied here.

## 2A. State, transaction outcome and verification are separate

The candidate tables identify transition activity without defining one shared lifecycle state machine. Three independent dimensions are required and must remain separate in every owner table: object lifecycle/process state (where applicable), transaction outcome for the individual request, and verification result against explicit criteria. A row describing a process stage must not encode a transaction outcome or verification result as the object's next state. Before implementation, each owner must define explicit fields and allowed values for these dimensions; the tables below are review prompts, not a final taxonomy.

| Dimension | Meaning | Illustrative values |
| --- | --- | --- |
| **Object lifecycle state** | Current authoritative status of the object itself | Active, Suspended, Quarantined, Superseded, Expired, Withdrawn, Deleted |
| **Transaction outcome** | Result of a particular request or attempted transition | Accepted, Rejected, Applied, Failed, Partial |
| **Verification result** | Result of checking a stated property against criteria and evidence | Verified, Failed verification, Inconclusive, Not verified |

A transaction can be Applied while the resulting object remains Suspended; an object can be Active while a particular transaction Failed; and an Applied transaction is not necessarily Verified. “Succeeded”, “completed”, “failed”, “verified”, “queued”, and “running” are not universal object lifecycle states. Where a table below still uses a compact from/to label, treat it only as a shorthand for the activity named in that row, not as a combined state transition. Each owner must represent process/object state, transaction outcome, and verification result independently; a row's outcome or verification must never be written as an object lifecycle state.

### Object-specific vocabulary mapping

| Owner / object class | Vocabulary | Classification |
| --- | --- | --- |
| KMS source/version | Candidate, Ingested, Pending validation, Eligible, Retrievable, Suspended, Quarantined, Superseded, Withdrawn, Expired, Deleted | Candidate/Ingested/Pending validation are ingestion or review process states; Eligible/Retrievable are use-eligibility states; remaining terms are lifecycle/restriction states. |
| KMS passage/index | Candidate, Current, Stale, Invalid, Eligible, Removed | Candidate/Current/Stale/Invalid are build/index process states; Eligible is retrieval eligibility; Removed is a local transaction result unless represented by a retained tombstone. |
| MMS memory | Candidate, Eligible, Active, Restricted, Quarantined, Superseded, Expired, Deleted | Candidate/Eligible are intake/write eligibility; remaining terms are object lifecycle states. |
| WMS definition | Draft, Pending review, Approved, Active, Suspended, Withdrawn, Superseded, Retired | Draft/Pending review/Approved are governance/activation states; Active and restriction/retirement terms describe definition lifecycle. Approval does not authorize a run. |
| WMS run | Queued, Admitted, Running, Succeeded, Failed, Partial, Cancelled, Cancellation pending | Queued/Admitted/Running are execution-process states; Succeeded/Failed/Partial/Cancelled are run outcomes; Cancellation pending is unresolved process status. |
| Identity principal/assertion | Pending verification, Verified, Active, Suspended, Revoked, Retired | Pending verification/Verified are proofing or assertion-verification results; other terms describe principal/assertion lifecycle. |
| Authentication context | Authenticated, Rejected, Expired, Revoked, Indeterminate | Authenticated/Rejected/Indeterminate are authentication results; Expired/Revoked describe context validity, not principal lifecycle. |
| Approval record | Pending, Approved, Denied, Revoked, Expired | Approval workflow/decision status; Approved is not a Permit. |
| Authorization decision | Permit, Deny, Approval required, Indeterminate, Expired, Revoked, Superseded | PDP outcome and decision validity, not protected-resource lifecycle. |
| PEP enforcement record | Enforced, Denied, Indeterminate | Enforcement transaction result, not authorization outcome or proof of operation success. |
| Provenance record | Recorded, Incomplete, Corrected, Retained, Disposal pending, Disposed | Record completeness/version and retention status; Recorded does not imply verified or truthful. |
| Derivative | Candidate, Eligible, Stale, Restricted, Quarantined, Withdrawn, Deleted | Candidate/Eligible are local readiness; remaining terms describe local derivative status. |
| Logging/audit event | Recorded, Recording failed, Integrity checked, Inconclusive | Recording transaction and verification outcomes, not originating-object lifecycle. |

These mappings are provisional guidance, not a final state taxonomy or authorization policy. Each owner must define exact semantics, allowed transitions and classification before implementation.

## 2B. Ownership clarifications

Canonical ownership is logical and does not imply an implemented component:

- **Core governance:** owner-controlled governance and the canonical repository retain authority over Core rules, approvals and change history; D-005 remains controlling.
- **WMS:** proposed logical owner of workflow definitions/versions, activation status and run coordination/state; architecture remains Proposed and unimplemented.
- **KMS / MMS:** KMS owns source records, versions, passages, indexes and source trust/review/freshness. MMS owns durable memory, memory eligibility/confidence and MMS-managed derivatives. D-007 and D-006 remain limited to their exact approved baselines.
- **Identity:** a future Identity Authority would own principal records, assertions and identity/authentication status; none is implemented or selected.
- **Approval versus authorization:** a separately accountable approval responsibility owns approval records and their lifecycle; the future Permission Service owns PDP decisions. The exact organizational/service boundary remains unresolved. Neither record substitutes for the other.
- **PEP:** each protected boundary owns its local enforcement evidence, not the PDP decision or another system's object.
- **Provenance:** each artifact owner retains authority over provenance and lineage for its artifacts; a shared contract/facility does not transfer ownership.
- **Derivatives:** the system operating each copy, index, cache, replica, excerpt or export owns its canonical derivative state and local mutation evidence.
- **Logging/audit:** a future logging/audit responsibility would own event records within its assigned scope; it is not defined or implemented.
- **Validation:** the assigned validation responsibility owns its validation result/evidence, not the candidate artifact or authorization decision.

## 2C. Minimum evidence for protected transitions

Where applicable and permitted, evidence must identify:

1. Initiating principal and authenticated executor separately; approver and authority basis where required. For each authentication result relied upon, include the issuer and verifier, principal/assertion identifier and version, intended audience/PEP, authentication time, validity start/end or maximum age, current revocation/status evidence, and relevant assurance context. Identify the evidence reference and its freshness; missing or unverifiable elements remain unknown and block where required.
2. Exact object identifier/class, canonical owner, version and requested operation.
3. Scope: project, tenant, subject, purpose, environment, resource and destination/recipient where relevant.
4. PDP decision identifier and typed outcome; policy and relevant context version; decision validity, expiry and current revocation/status evidence.
5. Intended PEP/audience and the PEP comparison of actual operation/context against every applicable Permit constraint immediately before the side effect.
6. Actual enforcement result, separately from actual transaction outcome (including partial or uncertain side effects).
7. Independent verification result, criteria/version, evidence references and limitations where required by risk. Independence is risk-based: where the producing or executing component cannot independently establish a critical property of its own action, verification must use a distinct responsible party or an independent evidence path capable of establishing that property. If neither is available, record the limitation and leave the result inconclusive/not verified; do not self-certify the critical property.
8. Correlation/causation and idempotency references, event/effective/observed/recorded times, and reconciliation references for partial or cross-owner work.

Keep evidence classes distinct: **approval is not a Permit; a Permit is not execution evidence; execution evidence is not independent verification.** Missing mandatory evidence remains unknown and blocks the transition where required. Minimize sensitive data; retain references rather than credentials or unnecessary payloads.

## 3. Core governance objects

**Canonical owner:** owner-controlled Core governance and canonical repository. Core rules remain read-only during normal operation under D-005.

| Object / transition | Candidate from → to | Initiator and owner action | Preconditions and authorization | Side effects and evidence | Failure / recovery |
| --- | --- | --- | --- | --- | --- |
| Core change proposal | Draft → submitted for review | Authorized owner or designated proposer submits a versioned proposal; owner governance records it | Rationale, impact analysis, affected rules, compatibility and rollback review; submission grants no change authority | Record proposal version, author, references, review status | Incomplete proposal remains draft/rejected; no core mutation |
| Core change approval | Under review → approved / rejected | Owner explicitly decides through the owner-led process | Explicit owner approval for exact diff/version; approval scope and identity must be verifiable | Record decision, approver, exact version/hash, rationale and time | Ambiguous or stale approval is invalid; return for fresh review |
| Core change application | Approved version → current version | Authorized owner applies exact approved change | Exact approved artifact matches; authorization revalidated; prior version recoverable | Preserve old/new version references and change evidence; update dependent policy references for review | Mismatch blocks; partial update requires repository reconciliation; never claim applied from approval alone |
| Core retirement | Current → superseded / retired | Owner initiates explicit replacement or retirement | Impact on dependent workflows and policy references reviewed; owner approval | Mark successor relation and affected references; preserve history per policy | Unresolved dependants remain flagged; no silent removal |

No workflow, tool, memory, knowledge item, model output, or ordinary task may initiate or apply a Core change autonomously.

## 4. Workflow Management System objects

**Canonical owner:** Workflow Management System (logical responsibility only; not implemented).

### 4.1 Workflow definition and version

| Transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Create definition | Absent → draft | Authorized maintainer; unique definition reference; shared workflow template and required fields present | Definition ID, author, initial version, scope and provenance | Invalid/incomplete definition remains draft |
| Submit for review | Draft → pending review | Required trigger, inputs, preconditions, permissions, steps, validation, outputs and failure handling documented | Exact version and review request reference | Missing fields reject submission |
| Approve / reject | Pending review → approved / rejected | Reviewer authority and criteria defined separately; approval bound to exact version | Reviewer, criteria/version, decision, rationale and time | Stale version or unverifiable reviewer blocks decision |
| Activate | Approved → active | Explicit activation authority; current Core constraints and permission boundaries satisfied; no unresolved critical validation | Active version, scope, activation decision and evidence | Any mismatch leaves version inactive |
| Revise | Active/draft → new draft version; prior version retained | Change request and impact review; never mutate a version pinned by an existing run | Parent version, diff, author, rationale and affected run assessment | Conflict creates a new review requirement; no in-place rewrite |
| Suspend / withdraw | Active → suspended / withdrawn | Authorized owner action; reason and affected runs identified | State change and notification references | Uncertain propagation means status remains pending; block new selections where current eligibility is unknown |
| Retire | Superseded / withdrawn → retired | Dependants and retention obligations assessed | Successor/retirement reference and retained history decision | Unresolved active dependencies prevent completion |

### 4.2 Workflow execution run

| Transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Create run | Absent → queued | Active exact workflow version; trigger and required inputs identified | Run ID, pinned definition/version, trigger, initiator and scope | Ineligible version or missing inputs rejects run |
| Admit / authorize | Queued → admitted / blocked | Fresh bounded authorization; executor identity established; preconditions and resource limits checked | PDP decision reference, executor identity context, admission result | Deny/indeterminate/stale context blocks admission |
| Start | Process status: admitted → running; transaction outcome: recorded separately | PEP confirms actual run/action matches Permit immediately before execution | Start event, executor, exact version and context | Context drift requires fresh evaluation; no execution |
| Complete | Process status: running → terminal; transaction outcome: applied / failed / partial (record separately) | Defined steps reached terminal outcomes; no hidden partial side effects | Per-step outcomes, output references, errors, evidence | Partial side effects trigger reconciliation; never label partial as succeeded |
| Cancel | Queued/admitted/running → cancelled / cancellation pending | Authorized cancellation request; executor/owner confirms safe stopping boundary | Requester, cancellation point, completed side effects | If stop cannot be confirmed, report cancellation pending and reconcile |
| Validate | Object/process status: terminal run remains terminal; verification result: verified / inconclusive / failed (separate record) | Independent criteria and evidence path appropriate to risk | Exact run/version, criteria, evidence, result and limitations | Missing/ambiguous evidence is inconclusive, not pass |
| Retain / dispose | Retained → disposal pending → disposed (policy-defined) | Approved run-log retention/disposal policy and authorization | Scope, stores/derivatives checked, disposal evidence | Unchecked stores remain pending; no universal retention period |

A workflow definition's activation does not authorize any run. A run's authorization does not approve the definition or its outputs.

## 5. Knowledge Management System objects

**Canonical owner:** KMS for source records, source versions, extracted passages, review/trust state, freshness and KMS indexes.

| Object / transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Source registration | Absent → candidate | Authorized registration request; source identity and acquisition context recorded where available | Canonical source reference, submitter/acquisition actor, time, declared restrictions; registration is not trust | Unknown origin stays explicit; incomplete source remains candidate |
| Ingest source version | Candidate → ingested / pending validation | Source scope and handling authority checked; exact source version captured or marked unavailable | Source/version reference, acquisition method, integrity result, provenance and restrictions | Integrity/provenance uncertainty quarantines or blocks ordinary use |
| Validate / review | Pending validation → eligible / rejected / quarantined | Defined criteria and reviewer authority; independent check for critical properties | Criteria/version, reviewer, evidence, source trust/review result and limitations | Inconclusive remains pending or quarantined |
| Publish for retrieval | Eligible → retrievable | Current authorization, scope, freshness and restrictions checked | Retrieval eligibility and version reference | Unknown current status blocks retrieval |
| Correct / version | Current version → superseded; new version → candidate | Owner-approved source correction or new acquisition; exact lineage captured | Old/new version links, changed content locator, provenance and review reset as required | Conflicting concurrent versions remain unresolved; no silent overwrite |
| Suspend / quarantine | Eligible/retrievable → suspended/quarantined | Authorized owner action or defined integrity/trust trigger; trigger evidence recorded | Restriction signal and affected index/reference scope | If index eligibility cannot be confirmed, block affected retrieval |
| Withdraw / revoke | Current → withdrawn | Authorized source-owner decision; scope and effective time recorded | Versioned withdrawal signal to derivative owners; notice is not proof of propagation | Remain propagation-pending until each required owner reports outcome |
| Expire | Eligible/retrievable → expired | Approved validity/freshness/retention rule reached | Rule/version, observed time, status and derivative impact | Clock/rule uncertainty blocks ordinary use pending review |
| Delete | Pending deletion → deleted (only after criteria) | Approved object-class disposal policy, owner authorization and derivative obligations enumerated | Owner-store and KMS derivative evidence; minimal tombstone only if separately permitted | Unchecked index/cache/backup remains pending; downstream owner verifies its own copies |

### 5.1 KMS indexes and extracted passages

| Transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Create/update passage or index entry | Absent/stale → current candidate | Exact source/version and extraction/transformation lineage available or explicitly incomplete; KMS authorization | Passage locator, transformation version, index build reference and source status | Missing lineage prevents promotion to eligible retrieval |
| Make searchable | Candidate → eligible | Source and passage remain eligible; scope and access restrictions enforced | Eligibility evidence tied to exact source version | Uncertain source status suppresses result |
| Invalidate / rebuild | Current → stale/invalid → rebuilt candidate | Source change/withdrawal or index consistency trigger; owner-controlled rebuild | Old/new index version, affected source set, reconciliation evidence | Partial rebuild remains degraded; do not report consistency |
| Delete index entry | Index-entry state: removal pending → absent or tombstoned; transaction outcome and verification result recorded separately | Source owner policy and KMS authorization; exact source/version mapping | Deletion evidence and reconciliation of replicas/caches | Unchecked replicas/caches keep removal pending |

## 6. Memory Management System objects

**Canonical owner:** MMS for durable memory records and MMS-managed derivatives. KMS retrieval is never memory-write authority.

| Transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Propose memory record | Absent → candidate | Authorized task/context handoff; source references and provenance completeness captured | Proposed content reference, source/version, passage locator, transformation, initiator/executor and uncertainty | Missing source details remain unknown; no invented provenance |
| Evaluate write eligibility | Candidate → eligible / rejected / quarantined | MMS independently evaluates scope, purpose, source status/trust, confidence/verification, conflict, privacy, retention and write policy | Criteria/version, decision and evidence; source trust remains distinct from memory confidence | Uncertain eligibility blocks durable write |
| Commit memory | Eligible → active | Fresh MMS-specific Permit for exact record/action/scope; PEP verifies actual write | Record/version ID, authorized writer, source lineage, policy and write outcome | Partial write reconciled before retry; KMS permission cannot substitute |
| Retrieve | Active → active (read event) | Current MMS retrieval authorization and record eligibility | Record/version and access evidence; read does not alter lifecycle state | Stale/unknown eligibility blocks protected disclosure |
| Correct / revise | Active → superseded; new candidate → active after review | Authorized correction; conflict and provenance review; fresh write authorization | Prior/new version relation, correction reason, lineage and revalidation | Concurrent edit conflict remains unresolved; do not overwrite |
| Restrict / quarantine | Active → restricted/quarantined | Authorized owner action or defined confidence/provenance/privacy trigger | Reason, affected retrieval paths and downstream notice | Disclosure blocked until eligibility restored |
| Reactivate | Restricted/quarantined → active | Cause resolved; fresh review and authorization; current source eligibility rechecked | New validation and authorization evidence | Old Permit/review cannot revive record |
| Expire | Active → expired | Approved retention/validity rule | Rule/version and derivative impact | No inferred extension; pending review if rule unavailable |
| Export | Active → export pending → exported | Separate export authorization, destination/purpose scope and disclosure review | Exact records/versions, destination, actor and export evidence | Partial export contained/reconciled; do not claim complete |
| Delete | Active/restricted/expired → pending deletion → deleted | Approved disposal rule and owner authorization; identify MMS stores and derivatives | Store/derivative evidence; restricted residual metadata only if policy permits | Any unchecked derivative or store leaves deletion pending |

## 7. Identity and authentication objects

**Canonical owner:** future Identity Authority; not implemented. Authentication context is distinct from principal record and identity assertion.

| Object / transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Register principal | Absent → pending verification | Approved enrollment authority and identity proofing policy must exist first | Principal reference, issuer, enrollment actor and evidence references; minimize personal data | No approved proofing basis means registration is blocked |
| Verify principal/assertion | Pending → verified / rejected | Issuer and verifier authority, audience, assurance and freshness rules defined | Assertion/version, issuer, verifier, evidence and limitations | Unknown issuer or unverifiable evidence remains unverified |
| Authenticate session/context | Absent → authenticated / rejected | Approved authentication mechanism and current principal/assertion status; intended audience and maximum age checked | Authentication result, issuer/verifier, time, audience, validity and revocation context | Stale, revoked, wrong-audience or indeterminate result fails closed |
| Change attributes/assertion | Current → new version / pending revalidation | Authorized identity owner; material changes identified | Old/new assertion references and affected authorization contexts | Affected contexts invalidated or re-evaluated before protected use |
| Suspend / revoke | Active → suspended / revoked | Authorized identity authority or defined compromise trigger | Status/version, effective time, revocation evidence and downstream notification | Unavailable revocation state blocks operations requiring current status |
| Reactivate | Suspended → active | Cause resolved; fresh proof/verification and policy approval | New assertion/version and fresh downstream PDP evaluation | Prior sessions/Permits do not automatically revive |
| Retire principal | Active/revoked → retired | Ownership, retention, audit and dependency rules approved | Dependency impact and restricted residual references | Unresolved obligations keep retirement pending |

No principal taxonomy, authentication protocol, identity provider, credential store, proofing method, or delegation model is selected. Delegation requires a separate bounded and revocable contract.

## 8. Approval and authorization objects

**Canonical owner:** future approval responsibility for approval records; future Permission Service responsibility for authorization decisions. Exact split remains unresolved.

| Object / transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Create approval request | Absent → pending | Exact proposed operation, initiator, executor, resource/version, purpose, scope and requested validity supplied | Request reference and immutable scope snapshot | Incomplete or changing request is returned for correction |
| Decide approval | Pending → approved / denied | Approver identity/authority verified; conflict-of-interest and policy requirements defined | Approver, authority, exact scope, decision, rationale, time and expiry | Approval mismatch/uncertainty is invalid |
| Revoke / expire approval | Approved → revoked / expired | Authorized approval owner or expiry rule | Status/version and affected decision references | Unavailable current status blocks use where required |
| Evaluate authorization | Request → Permit / Deny / Approval required / Indeterminate | PDP has validated identity, executor, resource, policy/context and approval evidence | Decision ID, exact binding dimensions, policy version, reason and validity | Indeterminate never becomes Permit; stale context requires reevaluation |
| Enforce Permit | Permit → enforced / denied / indeterminate | PEP authenticates executor and compares actual operation with every applicable bound dimension immediately before side effect | PEP, actual context, decision reference, enforcement result | Any mismatch, expiry, revocation uncertainty or policy/context drift blocks |
| Re-evaluate | Existing decision → superseded / re-evaluation required | Material identity, resource, policy, environment, purpose or revocation change | New decision linked to prior decision | Prior Permit cannot be silently reused |
| Close decision | Current → expired / revoked / superseded | Validity end, revocation, or policy/context change | Closure reason/time and dependent PEP references | Propagation uncertainty remains visible; protected action fails closed |

Approval is not Permit. A Permit is not evidence that an action occurred. Enforcement is not independent verification.

## 9. Provenance and lineage objects

**Canonical owner:** owner of the artifact described; shared Provenance Contract defines interoperability only.

| Transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Establish lineage reference | Absent → recorded / incomplete | Artifact owner can identify source and processing event; missing elements explicitly unknown | Artifact/version, source/version, locator, transformation, actor roles, times and integrity status | Incomplete lineage remains labelled; never fabricate |
| Append transformation | Current lineage → extended lineage | Authorized artifact owner; input/output versions identifiable | Transformation/version, initiator, executor, tool/component reference and result | Untraceable transformation marks lineage incomplete |
| Correct lineage | Current → corrected version; prior retained as policy allows | Owner-authorized correction with evidence and reason | Before/after references, correction actor and rationale | Disputed lineage quarantined from claims requiring certainty |
| Mark source restriction/withdrawal | Current → restriction signal recorded | Source owner signal validated and versioned | Source/version, status, effective time, notification references | Signal does not prove derivative updates |
| Retain / dispose lineage | Retained → disposal pending → disposed | Approved retention, privacy and evidence policy | Scope and authorized disposal evidence | Unchecked dependent references remain pending |

Provenance does not establish truth, correctness, trust, identity, authentication, or permission.

## 10. Derivatives, copies, indexes, caches and exports

**Canonical owner:** the component operating each derivative. The source owner may notify but may not mutate derivative-owner records.

| Transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Create derivative | Absent → candidate | Source/version and use restrictions captured; derivative owner authorizes creation | Parent/source reference, transformation, actor, destination/scope and integrity reference | Missing parent/version remains explicit; block if required by policy |
| Activate/disclose derivative | Candidate → eligible | Current source status and derivative policy rechecked; fresh authorization for actual disclosure | Exact derivative/source version, recipient/destination, purpose and PEP result | Unknown source eligibility blocks disclosure |
| Update after source change | Current → stale/restricted → updated candidate | Versioned source-owner notice; derivative owner identifies affected items | Impact assessment, old/new links and update evidence | Unidentified affected set remains pending; disclosure blocked where eligibility unknown |
| Withdraw / quarantine derivative | Eligible → restricted/quarantined | Authorized derivative owner or validated source restriction trigger | Local state change and downstream notices | No global completion claim until downstream owners verify |
| Delete derivative | Derivative state: deletion pending → deleted or retained-under-approved-rule; transaction outcome and verification result recorded separately | Owner-specific disposal authorization and retention rules | Local store/cache/replica/export checks and evidence | Unchecked copy or backup remains pending |

## 11. Logging and audit objects

**Canonical owner:** future Logging/Audit responsibility; not implemented.

| Transition | Candidate from → to | Preconditions / authorization | Required evidence and effects | Failure / recovery |
| --- | --- | --- | --- | --- |
| Record event | Absent → recorded / recording failed | Event producer supplies minimal attributable metadata; logging access policy permits it | Event/correlation/causation refs, actor roles, object/version, event and outcome times, evidence refs | Recording failure is explicit; never invent success |
| Validate event integrity | Recorded → integrity checked / inconclusive | Defined integrity and access controls; independent check where risk requires | Check method/version, evidence and limitations | Inconclusive remains so; do not assert tamper-proof status |
| Restrict access | Accessible → restricted | Authorized audit owner and purpose-based access policy | Actor, reason, scope and time | Unverifiable authority blocks access change |
| Retain / dispose | Retained → disposal pending → disposed | Separately approved retention/disposal and legal obligations | Exact stores, scope, authorization and disposal evidence | Unchecked stores remain pending |

Logging does not grant authority, establish execution success, or justify retaining unnecessary sensitive payloads.

## 12. Cross-owner reconciliation obligations

| Trigger | Coordinator / source owner | Each affected derivative owner | Completion condition |
| --- | --- | --- | --- |
| Source correction or new version | Publish versioned notice with source/version and known affected scope | Identify references, re-evaluate eligibility, update or restrict local derivatives | Each mandatory owner reports evidence-backed local outcome |
| Source quarantine, restriction or withdrawal | Publish versioned restriction signal and current status | Block affected disclosure where eligibility is unknown; quarantine/update/delete according to local policy | No global completion until all required owners verify |
| Source deletion request | Identify source stores and derivative owners; report source-local result separately | Independently authorize and perform local disposal or justified retention | Every mandatory owner supplies scope-specific evidence; otherwise pending/degraded |
| Identity suspension/revocation/material change | Identity owner publishes validated status/version signal | PDP/PEPs revalidate context and invalidate/re-evaluate affected decisions | Protected actions blocked until fresh valid context and decision |
| Policy/Core change | Owner publishes exact approved version and impact references | Each component assesses affected workflows/decisions and updates only under its authority | Dependent components report reviewed/updated outcomes; Core change itself remains owner-controlled |
| Workflow version withdrawal | WMS marks version unavailable for new admission and identifies active runs | Run owners/executors stop or reconcile according to safe boundary | New runs blocked; existing runs have evidence-backed terminal or pending status |

A coordinator may report **globally reconciled** only when every required owner has supplied evidence for its own obligations and the coordinator has checked the obligation set. Acknowledgement or notification receipt is not completion.

## 12A. Owner-local deletion and reconciliation evidence

Deletion or withdrawal must be evaluated separately for each canonical owner and each derivative owner. A source owner's notification, acknowledgement, or successful source-local deletion does not establish global deletion.

For each owner-local obligation, evidence must identify:

- exact object class, canonical identifier, versions and scope;
- canonical stores and replicas checked, including relevant indexes, caches, exports, backups and known derivative classes;
- applicable authorization, approval (if required), retention rule, legal-hold result and any permitted residual metadata/tombstone basis;
- each affected derivative owner notified, notice/version reference, and that owner's independently authorized local action and outcome;
- local verification criteria, evidence and limitations, including how the owner established that in-scope copies are no longer available for prohibited use/disclosure;
- outstanding, inaccessible, unknown or unverified stores/copies and the responsible owner/reconciliation action.

The originating owner may report only its own canonical-store result. A coordinator may report global reconciliation only after the obligation inventory is explicit and every required owner supplies evidence-backed local outcomes. Unknown or unverified backups, replicas, caches or exports keep the relevant obligation pending/degraded; do not claim complete deletion. Any residual tombstone, audit, provenance or legal-hold metadata requires a separately approved purpose, access boundary, retention and disposal rule. No universal deletion guarantee or retention interval is implied.

## 13. Object-class decisions still required

Before any implementation proposal, owner review must resolve at least:

- exact object taxonomies and state vocabularies for each owning system;
- exact permitted transition graph, including prohibited transitions and exceptional paths;
- role/authority mapping for requester, executor, approver, owner and verifier;
- authorization policy and PEP boundaries for each protected operation;
- mandatory evidence and independent verification criteria by risk;
- source freshness, trust, memory confidence and conflict rules without conflation;
- event identity, ordering, idempotency, concurrency and replay semantics;
- derivative discovery completeness and reconciliation obligations;
- retention, legal holds, backups, tombstones, audit metadata and deletion scope;
- notification failure, partial side effects, retry limits, manual escalation and recovery objectives;
- privacy, minimization, access, integrity and evidence retention controls.

## 14. Validation plan (design-level; not executed)

Proposed review/test cases include: every allowed transition and prohibited transition; missing/stale/revoked/mismatched authorization; approval without fresh PDP evaluation; initiator/executor confusion; stale object versions and concurrent changes; unknown provenance and fabricated-field prevention; source withdrawal with one derivative owner unavailable; incomplete index rebuild; MMS write without independent authorization; identity revocation with stale sessions; partial delete and backup uncertainty; duplicate/replayed/out-of-order events; logging failure during consequential action; reconciliation falsely reported complete; and independent validation scope mismatch.

Any future test record must identify exact artifact/version, context, criteria/version, evidence, result and limitations. A pass applies only to that tested scope; it is not implementation authorization or release approval. No tests are executed by this document.

## 15. Status and related records

This document is **Proposed**, pending owner review. It does not approve the shared Lifecycle Contract or any underlying contract, change D-006/D-007/D-008, select technologies, define runtime policy, or authorize implementation, integration, ingestion, operational use, or release.

Related:
- [Shared Lifecycle Contract](LIFECYCLE_CONTRACT.md)
- [Cross-System Interface & Ownership Matrix](CROSS_SYSTEM_INTERFACE_MATRIX.md)
- [Joint Contract Review](JOINT_CONTRACT_REVIEW.md)
- [Provenance Contract](PROVENANCE_CONTRACT.md)
- [Identity Contract](../systems/permissions/IDENTITY_CONTRACT.md)
- [Authorization Contract](../systems/permissions/AUTHORIZATION_CONTRACT.md)
- [Project Status](PROJECT_STATUS.md)
- [Decision Log](DECISIONS.md)
