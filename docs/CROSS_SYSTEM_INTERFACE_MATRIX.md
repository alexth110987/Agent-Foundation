# Cross-System Interface & Ownership Matrix — Proposed

**Status:** Proposed coordination artifact — pending owner review.  
**Implementation:** Documentation only; no interfaces, services, adapters, event infrastructure, or runtime exist.  
**Authority:** Subordinate to `AGENT.md` and approved Core rules. This matrix coordinates D-006, D-007, D-008 and the proposed Identity, Authorization, Provenance, and Lifecycle Contracts. It does not amend approvals or authorize implementation.

## 1. Purpose

Make cross-system ownership, handoffs, evidence references, and unresolved interface questions explicit while remaining purpose-neutral. Terms such as *owns*, *provides*, *consumes*, *references*, *enforces*, and *requests* describe logical responsibilities only, not deployed services.

## 2. System ownership matrix

| System / responsibility | Canonical ownership | Provides | Consumes / references | Must not do |
| --- | --- | --- | --- | --- |
| Core governance | Approved Core rules, global constraints, authority precedence, owner-controlled change history | Applicable constraints and policy references | Owner decisions and reviewed change proposals | Be modified by ordinary workflows, tools, memory, knowledge, or model output |
| WMS | Workflow definitions/versions, activation status, run coordination and execution state | Version-pinned workflow/run references, operation requests, execution outcomes | Core constraints, authorization decisions, KMS/MMS results, validation/error outcomes | Grant permission, authenticate principals, or own KMS/MMS records |
| Identity Authority (future) | Principal records, identity assertions, authentication status and evidence references | Validated identity/authentication context | Issuer and verification evidence | Decide authorization or imply permissions |
| Permission Service / PDP (future) | Central authorization decisions under approved policy | Typed bounded decision and reason/reference | Validated identity, trusted attributes, policy/resource context, approval evidence | Execute protected operations or authenticate principals by itself |
| Approval responsibility (future) | Approval record and its validity/revocation state | Explicit bounded approval or denial | Exact proposed operation, scope, requester/executor context | Issue a Permit or override Core prohibitions |
| PEP at each boundary | Local enforcement result at its protected action boundary | Enforcement and action-outcome references | PDP decision, actual operation/context, local constraints | Broaden a Permit or infer approval |
| KMS | Source records/versions, extracted passages, review/trust state, freshness, retrieval eligibility and indexes | Authorized retrieval with exact source/version/provenance references | Identity, authorization, provenance and source inputs | Automatically promote content into MMS or treat relevance/index membership as truth or permission |
| MMS | Durable memory records, memory eligibility/confidence, MMS-managed derivatives and lifecycle | Authorized memory results and record/version/provenance references | Authorized KMS/task inputs, identity and authorization context | Treat KMS retrieval as write authorization or mutate KMS-owned records |
| Provenance (shared contract) | Each artifact owner owns provenance/lineage for its artifacts | Origin, acquisition, transformation and lineage references | Actor identity references, event/outcome and source/version references | Authenticate actors, grant permissions, assert truth/trust |
| Lifecycle (shared contract) | Each object owner owns canonical state and authorized transitions | Transition requests/results and reconciliation evidence | Object/version, authorization, provenance and validation evidence | Mutate another owner's authoritative records or claim its completion |
| Validation (future) | Criteria/results within an assigned validation scope | Pass/fail/inconclusive results and evidence | Candidate results, criteria and execution evidence | Create authorization or certify beyond tested criteria |
| Logging/audit (future) | Governed event records within assigned scope | Event references and recording outcome | Minimal event metadata from responsible components | Create authority or determine execution success |

## 3. Cross-system handoff matrix

| Handoff | Provider / owner | Consumer | Minimum logical content | Boundary |
| --- | --- | --- | --- | --- |
| Identity → Authorization | Identity Authority | PDP / PEP | Principal, issuer, authentication result/context, audience, validity/status, assurance reference | Identity is validated input, never a grant |
| Authorization → Enforcement | PDP | PEP | Typed outcome; exact principal/executor/action/resource/purpose/project/tenant/subject/environment/constraints/audience/policy/validity | Only a complete current Permit can permit; PEP may deny more narrowly |
| Approval → Authorization | Approval authority | PDP | Attributable decision, approver authority, exact request scope, time, expiry/status | Approval is prerequisite; fresh PDP evaluation required |
| KMS → MMS | KMS | MMS | Authorized result, exact source/version, minimal provenance, restrictions and relevant freshness/review metadata | Retrieval does not authorize memory write; MMS independently evaluates write and retention |
| Artifact owner → Provenance | Owning component | Authorized provenance consumers | Artifact/version, origin/actor attribution, processing event, input/output lineage, uncertainty/integrity state | No fabricated lineage; metadata remains access-controlled |
| Owner → Lifecycle requester | Authoritative object owner | Requester/coordinator | Accepted/rejected/pending result, object/version, transition outcome and evidence references | Requester cannot mutate owner state |
| Source owner → Derivative owners | KMS or other source owner | MMS/index/cache/copy owners | Withdrawal/correction/deletion notice, source/version, scope and affected references | Each derivative owner separately authorizes and performs its own action |
| Executor → Validation | Executing component | Validation responsibility | Actual result, run/version, criteria and outcome/provenance references | Authorization is not execution evidence |
| Component → Logging | Event-producing component | Logging/audit | Minimal event, actor/executor, correlation, state/outcome and decision references | Logging records evidence; it creates no authority |

## 4. Object lifecycle ownership

| Object class | Canonical owner | Dependent references / derivatives | Coordination requirement |
| --- | --- | --- | --- |
| Core rules / decisions | Owner-controlled Core governance and canonical repository | Workflow and component policy references | Explicit owner change process and version history |
| Workflow definition / run | WMS | Run outputs, logs, validation and downstream references | Definition lifecycle separate from run lifecycle; version-pinned execution |
| Principal / identity assertion | Future Identity Authority | Authentication contexts, authorization and provenance references | Relevant status/attribute changes trigger revalidation and PDP re-evaluation |
| Approval / authorization decision | Future approval / Permission responsibilities | PEP enforcement and audit references | Expiry, revocation and policy changes constrain future use |
| KMS source/version/passage | KMS | Indexes, retrieval results, copied excerpts, MMS references | Source changes/withdrawal/deletion propagate status; each derivative owner handles its records |
| MMS memory/derivative | MMS | Retrieval results and derived memory | MMS owns correction, expiry, export, deletion and reconciliation |
| Provenance record/reference | Owner of described artifact | Downstream lineage references | Correction and retention follow owner policy and shared contract |
| Index/cache/replica/copy | System operating the derivative | Search/retrieval outputs | Preserve source/version/scope links and honor owner notifications |
| Audit/event record | Future logging/audit owner | Validation, incident and reconciliation references | Separate access, integrity, retention and disposal policy |

## 5. Cross-system invariants

1. Each managed object has one authoritative owner for canonical state and authorized mutation.
2. A reference, copy, cache, index, excerpt, or derivative does not transfer ownership.
3. A consumer requests changes from the owner; it does not mutate the owner's source record.
4. Every protected operation is independently authorized at its action boundary.
5. Initiator, executor, approver, owner, and processing actor remain distinct where applicable.
6. Provenance and lifecycle records describe origin/events; neither creates permission nor proves truth.
7. KMS source trust and MMS memory confidence/verification remain separate.
8. KMS retrieval cannot cause an MMS durable write without separate MMS eligibility and authorization.
9. Withdrawal, restriction, correction, or deletion is not complete across systems until each affected owner verifies its own obligations.
10. Unknown, conflicting, stale, or unavailable evidence remains explicit; gaps are not filled by inference.

## 6. Interface semantics still to define

Before implementation proposals, contracts must jointly define stable identifiers and reference rules; correlation/causation; event/effective/observed/recorded times; requested/accepted/authorized/attempted/applied/failed/partial/reconciled/verified/completed outcomes; version and concurrency semantics; idempotency and replay handling; typed errors and uncertainty propagation; minimum evidence per operation class; metadata access control; notification, retry, reconciliation and recovery; and contract compatibility/versioning.

These are logical requirements only. No schema, API, protocol, broker, database, event store, service topology, or runtime is selected.

## 7. Open questions

1. Which identity result is sufficient for PDP evaluation, and how are audience, freshness, revocation and assurance conveyed to each PEP?
2. How is the actual executing component authenticated independently of the initiating principal?
3. Is approval-record lifecycle owned by a dedicated approval responsibility or the Permission Service boundary?
4. May a shared provenance facility provide common services while each artifact owner retains authority over its own lineage?
5. Which minimal tombstone/audit/provenance metadata may remain after deletion, under what obligations, and who verifies each derivative class?
6. What evidence lets a coordinator report reconciliation complete without assuming another owner's verification?
7. What consistency and disclosure-blocking guarantees apply during withdrawal, quarantine, version replacement and deletion?
8. Which validation checks must be independent of the component producing the candidate result?

These are unresolved design questions, not implementation defects. Resolve purpose-neutral questions now; defer purpose-dependent choices until the operating context is approved.

## 8. Status and readiness

This matrix is **Proposed**, pending owner review. It does not approve or revise D-006, D-007, or D-008; approve any of the four proposed contracts; select technology; authorize ingestion, services, integrations, runtime, operations, or release.

## 9. Joint refinement requirements

The proposed [Joint Contract Review refinement register](JOINT_CONTRACT_REVIEW.md) records additional cross-document requirements for authentication freshness and revocation, independent executor identity, exact PEP scope comparison, KMS-to-MMS provenance completeness, derivative-owner reconciliation, object-specific lifecycle transition tables, event/idempotency semantics, partial-failure recovery, and independent validation. These remain proposals pending owner review; they do not change the authority or status of this matrix or the underlying contracts.

## 10. Related records

- [Component Model](COMPONENT_MODEL.md)
- [Project Status](PROJECT_STATUS.md)
- [Decision Log](DECISIONS.md)
- [Identity Contract](../systems/permissions/IDENTITY_CONTRACT.md)
- [Authorization Contract](../systems/permissions/AUTHORIZATION_CONTRACT.md)
- [Permission Architecture](../systems/permissions/PERMISSION_ARCHITECTURE.md)
- [Memory Architecture](../systems/memory/MEMORY_ARCHITECTURE.md)
- [Knowledge Architecture](../systems/knowledge/KNOWLEDGE_ARCHITECTURE.md)
- [Provenance Contract](PROVENANCE_CONTRACT.md)
- [Shared Lifecycle Contract](LIFECYCLE_CONTRACT.md)

## Joint refinement — minimum cross-system interaction semantics

The following refinements align the handoff matrix with the proposed Provenance and Shared Lifecycle Contracts and the approved logical Identity and Authorization Contract baselines. They do not create deployed interfaces or amend D-006, D-007, D-008, D-010, or D-011.

### Handoff envelope and evidence boundaries

For a cross-system request or result, the logical exchange should identify, where applicable and permitted:

- owning system/namespace and exact object/version references;
- request, event, correlation, and causation references;
- initiating principal, authenticated executor/component, authoritative owner, and approver as distinct roles;
- requested operation, purpose, project/tenant/subject scope, environment, and relevant constraints;
- identity/authentication context reference and bounded authorization/approval decision reference;
- event, effective, observed, and recorded times;
- provenance, validation, integrity, and lifecycle evidence references;
- typed outcome, reason, unresolved uncertainty, and next responsible owner.

This is a minimum conceptual envelope, not a selected schema. Omit inapplicable fields rather than inventing values; represent required but unavailable information explicitly as unknown or unavailable.

### Typed outcomes and ownership

Every provider response distinguishes rejection, acceptance for processing, authorization, attempted execution, applied change, partial application, failure, indeterminate outcome, reconciliation, and verification. A transport-level success or acknowledgement is not an application-level success.

The provider remains authoritative for its own records. Consumers may request, reference, and verify within their permissions; they may not mutate another owner's canonical state or infer completion from a notification. Each owner supplies evidence for its own state changes and derivative obligations.

### Cross-system KMS-to-MMS and derivative handling

KMS supplies the exact source/version and permitted provenance, locator, source status/trust/review/freshness, restrictions, and uncertainty/integrity context needed for MMS to assess a candidate input. KMS retrieval remains distinct from MMS write eligibility, authorization, retention, and write execution.

Source correction, restriction, quarantine, withdrawal, and deletion are versioned owner signals. MMS and every other derivative owner independently assess impact, authorize and perform local actions, and report their own outcomes. If current eligibility cannot be established, the consumer applies the relevant disclosure-blocking policy; no global completion is inferred from event delivery.

### Identity and authorization handoff

Identity assertions and authentication results are validated inputs, not permissions. A Permit remains bound to the exact operation context and intended enforcement point. The PEP checks the actual operation and current context immediately before the protected side effect. Initiator, executor, and approver remain distinct; approval is a prerequisite and requires fresh authorization evaluation. Relevant identity, policy, scope, resource-version, or revocation changes invalidate reliance on stale context as defined by D-010/D-011.

### Event delivery, conflicts, and recovery

Before implementation, each interface defines version-conflict behavior, duplicate/replay detection, idempotency scope, ordering expectations, timeout/unknown outcomes, safe retry conditions, partial-side-effect discovery, and reconciliation ownership. Delayed or reordered events cannot silently roll back a newer owner state. Unknown outcomes remain indeterminate until checked against the authoritative owner.

### Independent validation

Where a critical property cannot be established by the producing component, the matrix identifies a separate validation responsibility or evidence path. Validation records identify exact artifact/version, context, criteria/version, evidence, result, and limitations. Validation does not grant permission, prove unrelated properties, or approve release.
