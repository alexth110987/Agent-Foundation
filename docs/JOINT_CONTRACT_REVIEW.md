# Joint Contract Review — Refinement Register

**Status:** Proposed refinement register — pending owner review.  
**Scope:** Documentation review only. D-010 and D-011 remain approved exact logical contract baselines. D-012 also approves the exact Provenance Contract, Shared Lifecycle Contract, and Cross-System Interface & Ownership Matrix baselines recorded in docs/DECISIONS.md. This register remains Proposed and non-amending; it does not revise any approved baseline, amend D-006 through D-012 or R-01–R-07, or authorize implementation.

## 1. Purpose

Record cross-document refinements so subsequent review and edits remain traceable and consistent. The Identity Contract (D-010), Authorization Contract (D-011), Provenance Contract, Shared Lifecycle Contract, and Cross-System Interface & Ownership Matrix are approved only as their exact documentation-level logical baselines under D-010, D-011, and D-012. This refinement register remains Proposed and does not amend those baselines.

## 2. Identity and authorization boundary

The contracts must preserve three distinct actors wherever applicable:

- **Initiating principal:** the actor that requests or causes the task.
- **Executing principal/component:** the authenticated workload or component that performs the protected operation.
- **Approver:** the authority that approves a bounded request when policy requires it.

The identity-to-authorization handoff must carry a validated authentication result with issuer/verifier, principal and assertion version, intended audience/PEP, authentication time, not-before/expiry or maximum age, current status/revocation evidence, and assurance context where policy requires it. The receiving PDP/PEP validates these properties through a separately approved mechanism. If freshness or revocation cannot be established where required, the protected operation fails closed.

Identity changes that affect authorization, issuer/key compromise, session invalidation, and principal suspension/revocation require revalidation and fresh PDP evaluation. Existing decisions must not silently survive a material identity-context change.

The executing component must be authenticated independently of the initiating principal. Delegation, if ever required, remains a separately specified, bounded and revocable authority; it is not implied by workflow context or identity association.

## 3. Authorization decision and PEP boundary

A Permit is bound to the exact initiating principal, authenticated executor, operation, resource and owner, purpose, project/tenant/subject, environment, constraints, intended PEP/audience, policy/context version, and validity. A PEP must compare the actual operation context with every applicable bound dimension immediately before the side effect. Any mismatch, material change, stale decision, unavailable required revocation state, or inability to enforce blocks execution and requires fresh evaluation.

Approval remains a prerequisite rather than a permit. After approval, the PDP performs a fresh evaluation. PEP enforcement outcome, operation outcome, and independent verification remain separate evidence.

## 4. KMS-to-MMS provenance and derivative lifecycle

For every KMS result eligible to be considered by MMS, the handoff must preserve, where available and permitted:

- source-system and canonical source-record identifiers;
- exact source version or an explicit indication that version identity is unavailable;
- passage/extraction locator and transformation/version reference where applicable;
- origin/acquisition actor and processing actor references;
- source status, review/trust state, freshness/observation context and applicable use restrictions;
- provenance completeness/uncertainty and integrity-check state.

Missing fields must be represented as unknown/unavailable, never fabricated. MMS independently evaluates memory-write eligibility, authorization, retention and lifecycle; KMS retrieval is not write authority.

A source correction, restriction, quarantine, withdrawal or deletion notice is a versioned lifecycle signal, not proof that derivatives have been updated. Each derivative owner (including MMS, indexes, caches, replicas, exports and copies) must identify affected objects, independently authorize and perform its own action, record outcome/evidence, and report pending, partial, failed or verified states. Disclosure of affected content remains blocked where its current eligibility cannot be established under the applicable policy.

## 5. Lifecycle ownership and transitions

Each object class requires an owner-maintained transition table before implementation design. At minimum, each row must define:

- object class and canonical owner;
- source and target state;
- initiating event and authorized requester;
- required authorization and approval scope;
- preconditions and invariants;
- side effects and dependent/derivative owners to notify;
- evidence required to mark the transition applied and verified;
- retry/idempotency and concurrency behavior;
- failure, compensation (if safe), reconciliation and escalation behavior.

Requested, accepted, authorized, attempted, applied, partially applied, failed, pending reconciliation, reconciled, verified and completed are distinct outcomes. A coordinator may report global reconciliation only when each responsible owner supplies evidence for its own obligations. No owner may mutate another owner's canonical state.

Deletion requests do not prove deletion. Where policy requires downstream handling, the object remains pending/degraded until required derivative owners verify their own obligations. Any retained tombstone or audit metadata requires separately defined purpose, access, retention and disposal rules.

## 6. Event, evidence and recovery semantics

Before implementation proposals, the contracts and matrix should define common logical semantics for:

- stable object, version, request, event, correlation and causation references;
- event time, effective time, observed time and recorded time;
- idempotency keys, duplicate/replay handling, ordering and concurrency/version conflicts;
- typed errors and explicit unknown/unavailable/partial outcomes;
- retry eligibility only for operations proven safe/idempotent;
- partial side-effect discovery and reconciliation before retry;
- minimum evidence and access controls for each operation class;
- retention, integrity, privacy and disposal of decision, provenance and audit references.

No serialization format, protocol, database, broker, service topology, runtime, retention interval or implementation is selected.

## 7. Independent validation

Validation criteria must be defined by risk and operation class. Where the producing component cannot independently establish a critical property, validation must be performed by a separate responsibility or evidence path. The validation record identifies the exact artifact/version, execution context, criteria/version, evidence, result and limitations. A pass applies only to the tested version and criteria; it is not authorization, release approval or proof beyond its scope.

## 8. Owner review checklist

- [ ] Cross-document use of the approved D-010 identity freshness, audience, and revocation semantics confirmed; this review does not reopen or amend D-010.
- [ ] Initiator/executor/approver distinctions and executor authentication accepted.
- [ ] Cross-document alignment with the approved D-011 Permit-binding and PEP-boundary requirements confirmed; this review does not reopen or amend D-011.
- [ ] KMS-to-MMS provenance minimum and missing-data semantics accepted.
- [ ] Derivative notification, owner-local action and reconciliation semantics accepted.
- [ ] Object-specific lifecycle transition tables required before implementation design.
- [ ] Event, idempotency, concurrency, partial-failure and recovery semantics accepted.
- [ ] Independent validation boundaries accepted.

## 9. Authority and status

This register remains a Proposed, non-amending review artifact. D-010, D-011, and the three exact D-012 baselines are approved only within their recorded documentation-level logical scope; this register itself remains Proposed pending owner review. D-006, D-007, and D-008 remain unchanged and limited to their exact approved logical architecture baselines. Stage 1 remains active. No runtime, service, integration, tool, data ingestion, operational workflow, or implementation is authorized.
