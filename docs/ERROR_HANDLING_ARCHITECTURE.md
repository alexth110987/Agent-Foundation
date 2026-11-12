# Error Handling and Recovery Architecture

**Status:** Proposed logical architecture; not owner-approved.  
**Scope:** Purpose-neutral design documentation only.  
**Implementation authority:** None. No runtime, retry engine, recovery service, integration, test execution, or operational procedure is authorized.

## 1. Purpose and boundaries

Define a fail-safe approach to representing, containing, recovering from, and learning from failures across future components. This complements rather than replaces WMS, Identity and Authorization Contracts, Permission System, Provenance Contract, Lifecycle Contract, Logging Architecture, and the D-014-approved Validation Strategy policy. No validation framework or validation execution capability is implemented; validation, test execution, verification, and release remain separate gates.

No technologies, providers, storage, transport, retry counts, timeout values, severity targets, retention periods, or service levels are selected. These depend on a future approved purpose and operating context.

## 2. Governing principles

1. **Fail closed at authority boundaries.** Missing, expired, stale, revoked, malformed, or indeterminate identity or authorization evidence blocks protected action.
2. **Never manufacture success.** Sending, accepting, queueing, timing out, cancelling, or acknowledging a request does not prove the intended effect occurred.
3. **Preserve uncertainty.** If a side effect cannot be established, record an indeterminate outcome and reconcile with the authoritative owner; do not guess.
4. **Respect ownership.** Each component remains authoritative for its own canonical state. A coordinator cannot declare another component's state changed.
5. **Retries are new attempts, not proof.** Retry only under explicit safety conditions and link every attempt to the original operation.
6. **Recovery cannot bypass authority.** Recovery, compensation, replay, and reconciliation require current identity, authorization, and policy checks.
7. **Verify independently where risk requires it.** Execution evidence and verification evidence are distinct.
8. **Minimize evidence.** Preserve identifiers and references without copying secrets or unnecessary payloads into error records.

## 3. Failure taxonomy

Assign one primary category and any contributing categories:

- Input / contract: missing, malformed, unsupported, or incompatible input/interface.
- Precondition / lifecycle: invalid state, expired object, unmet dependency, prohibited transition.
- Identity: absent, invalid, stale, revoked, or unverifiable actor identity.
- Authorization / approval: denial, missing approval, expired permit, scope mismatch, unavailable authority.
- Policy / integrity: invariant violation, tampering suspicion, inconsistent version, policy conflict.
- Dependency / availability: unavailable, degraded, unreachable, inconsistent dependency.
- Resource / capacity: quota, rate, concurrency, memory, storage, or cost boundary.
- Execution: exception, termination, crash, timeout, cancellation, partial execution.
- Data / provenance: missing, conflicting, unverifiable, withdrawn, or stale source evidence.
- Validation: failed, inconclusive, unavailable, or not-run validation.
- Logging / audit: required event could not be durably recorded or integrity established.
- Recovery / reconciliation: replay conflict, unresolved side effect, ownership mismatch, failed compensation.
- Security / abuse: suspicious invocation, boundary violation, credential exposure, suspected compromise.

A category is not a severity rating. Severity, urgency, and escalation targets remain purpose-dependent.

## 4. Keep outcome dimensions separate

Never collapse these dimensions into a single success/failure flag.

| Dimension | Suggested values | Meaning |
| --- | --- | --- |
| Process status | not started; running; completed; failed; cancelled | Execution process state. A timeout is recorded as a timeout event/condition, not as an additional process state; the coordinator maps it to an existing execution state under its own lifecycle contract. |
| Transaction outcome | not attempted; rejected; accepted; applied; partial; failed; indeterminate | Known result of requested operation or side effect |
| Verification result | not run; pass; fail; inconclusive; not applicable | Evidence-based assessment against criteria |
| Recovery status | not required; pending; reconciling; recovered; unrecoverable; escalated | State of recovery work |

An accepted request may still have an unapplied or indeterminate transaction outcome. A completed process may fail validation. Recovery does not automatically mean the original transaction was reversed or the object is valid.

## 5. Error record envelope

A future versioned envelope should define applicable fields:

- Stable error/event ID, schema version, primary and contributing categories.
- Component and exact version; original error code/type and safe diagnostic summary.
- Request, workflow run, step, attempt, correlation, and causation identifiers.
- Initiator, executor, approver, and accountable owner as distinct roles where applicable.
- Target object/resource identifier, owner, version, and operation scope.
- Event and recorded time; effective/observed times where meaningful.
- The four typed outcome dimensions in Section 4.
- Retry eligibility decision and rationale; retry-budget reference where defined.
- Known, unknown, and potentially partial side effects, clearly distinguished.
- Authorization/approval references, never credentials or secret material.
- Provenance, lifecycle, validation, and logging evidence references.
- Classification, redaction, access, and retention metadata.

Do not include passwords, API keys, access tokens, private keys, recovery codes, or full sensitive payloads. A reference to protected evidence does not grant access to it.

## 6. Decision and containment flow

1. **Capture:** preserve original error and identifiers without exposing sensitive payloads.
2. **Classify:** assign category; distinguish observed facts from hypotheses.
3. **Contain:** stop unsafe continuation and prevent unauthorized duplicate/downstream actions.
4. **Establish outcome:** determine process status, transaction outcome, verification result, and recovery status independently.
5. **Select disposition:** stop, reject, retry, reconcile, compensate, escalate, or close with rationale.
6. **Re-authorize:** obtain fresh identity and authorization for protected retry, replay, repair, or compensation.
7. **Recover:** act only within authoritative owner's boundary and approved scope.
8. **Verify:** collect independent evidence appropriate to risk and criteria.
9. **Close or escalate:** close only when outcome and evidence are adequate.

## 7. Retry and replay safety
## 7.1 Retry-budget governance

Retry eligibility and a bounded retry budget are separate requirements. The existence of a retry budget does not make an operation retryable, and no retry may proceed unless every applicable safety condition in §7 is established.

The following values remain intentionally unresolved until the future purpose, dependency profile, and operating context are approved: maximum attempts, back-off schedule, per-operation and aggregate budgets, timeout values, circuit-breaker thresholds, and escalation thresholds or targets. No default numeric values are implied by this architecture.

Before implementation or operational use, the owner-approved policy for each operation class must set and justify those bounds using, at minimum, operation criticality, idempotency or deduplication guarantees, failure class, uncertainty about side effects, dependency/provider rate limits, cost exposure, and available reconciliation and verification evidence. The policy must also define budget exhaustion behavior and escalation criteria. A timeout or exhausted budget never establishes that an external side effect did not occur and never independently authorizes retry, replay, or compensation.


Retry is permitted only when all applicable conditions are established:

- Failure is explicitly classified as transient/retryable; unknown failures are not presumed transient.
- Operation is idempotent, or evidence establishes no side effect, or an approved deduplication mechanism makes repetition safe.
- Original transaction outcome is known sufficiently to prevent duplicate effects. A timeout alone is insufficient.
- Target object, version, lifecycle state, and authorization context remain current.
- Bounded retry budget and provider/rate/cost limits are defined for the approved context.
- Each attempt receives a distinct attempt ID and links to the original request through causation/correlation.
- Retry does not cross an owner boundary or bypass validation, approval, or logging requirements.

Do not blindly replay events, requests, or tool calls. Delayed, duplicate, reordered, or conflicting events require owner-local deduplication and reconciliation rules. Retry counts, backoff, timeout, and circuit-breaker policies remain unresolved until purpose and dependency profile are approved.

## 8. Timeouts, cancellation, partial effects, compensation

- A timeout means the caller stopped waiting; it does not prove the remote action did not happen. Record the timeout as an event/condition and map the process to an existing lifecycle state (for WMS runs, see `workflows/WORKFLOW_MANAGEMENT.md` §7); do not create a separate `Timed out` state.
- If an external effect may have occurred, preserve an `indeterminate` transaction outcome until the authoritative owner establishes it. A timeout alone never establishes retry eligibility; reconcile first and apply all retry/replay safety conditions.
- Cancellation is a request to stop, not rollback.
- A process can terminate after a side effect and before acknowledgement.
- Represent partial effects explicitly, including confirmed, absent, and unknown effects.
- Compensation is a new action with its own authorization, preconditions, idempotency, evidence, and verification.
- If outcome remains indeterminate, reconcile against the authoritative component before potentially duplicative action.

## 9. Reconciliation and recovery ownership
### 9.1 Cross-component recovery responsibility boundaries

The component that owns a resource or operation's canonical state retains authority to reconcile and mutate that canonical state. A workflow coordinator or other orchestrator may coordinate a recovery process, collect outcomes, preserve correlation, and request owner action; it cannot decide that another component's canonical state has changed or direct an owner to accept an unverified state.

Compensation is a new protected action. It must be freshly authorized and executed by the owner of the affected resource or operation, or by a boundary to which that owner has explicitly delegated the relevant authority. The actor, authority, preconditions, scope, and resulting evidence must be recorded and independently verified where risk requires it.

The exact role that approves cross-system recovery, the governance for disputes between owners, and any organizational or technical coordination authority remain unresolved. They must be defined for a future approved operating context. This architecture does not create a central recovery authority, transfer canonical ownership to WMS, or grant a coordinator compensation authority.


- Reconciliation is performed by or under the authority of the component owning canonical state.
- Consumers may request or observe reconciliation but cannot assert another component's canonical state changed.
- Check object identity, owner, version, lifecycle eligibility, event ordering, deduplication key, and provenance before applying an event.
- Derivative components independently assess their records, indexes, caches, and copies after source changes.
- If deletion, withdrawal, or access eligibility is unknown, block disclosure/use until the owner resolves the state.
- Preserve unresolved conflicts and evidence references; do not silently overwrite one side.

## 10. Dependency-specific fail-safe behavior
### 10.1 Ownership of event-class logging policy

Before implementation or operational use, an accountable policy owner must be designated for each event class whose recording is required or whose availability affects a protected action. That policy owner is responsible for defining the event-class recording obligation and the consequence of logging failure in the approved operating context. The producer remains responsible for creating the event; a future logging component remains responsible for handling and preserving it according to the approved policy. These responsibilities are distinct and do not imply a selected organizational structure or technical topology.

The event-class policy must explicitly determine when unavailable or unverified logging evidence requires blocking, pending/buffering, or controlled continuation, and identify the authority accountable for that decision. No blanket shutdown or blanket continuation rule is established here. Exact policy owners, event-class obligations, and technical handling remain unresolved until the operating context is approved.


| Dependency or condition | Minimum safe behavior |
| --- | --- |
| Identity or Permission service unavailable | Deny protected action; no stale/indeterminate authority fallback |
| KMS or MMS unavailable/inconsistent | Do not fabricate source, memory, or index state; preserve pending/unknown outcome |
| Required provenance missing/unverifiable | Mark unknown and block operations for which provenance is required |
| Lifecycle owner unavailable | Do not infer eligibility; keep transition/recovery pending |
| Validation unavailable | Do not report pass or promote artifact as verified |
| Required logging evidence unavailable | Apply event-class policy; block consequential actions when required evidence cannot be established |
| Tool/provider outage | Do not silently substitute an unreviewed tool/provider |
| Core instruction/policy conflict | Stop affected processing and escalate through owner-led governance |

The event classes for which logging failure blocks action must be defined in the future operating context. Neither blanket continuation nor blanket shutdown is assumed.

## 11. Escalation and closure

Escalate when authority is uncertain, side effects are indeterminate, integrity is suspect, canonical owners disagree, recovery exceeds approved scope, repeated failures exceed defined limits, required evidence is unavailable, or security/privacy impact is suspected.

An escalation package should include stable identifiers, component/version, category, timeline, typed outcomes, known/unknown effects, containment, evidence references, attempted recovery and its authorization, unresolved questions, and accountable owner. Exclude secrets and unnecessary sensitive content.

Do not close merely because a handler returned, a retry succeeded, or a ticket was created. Closure requires established outcome, required verification, owner disposition, and preserved evidence.

## 12. Future validation requirements

Before implementation, define isolated tests for malformed contracts, stale/revoked identity, denied authorization, dependency outage, timeout after remote side effect, cancellation, duplicate/reordered events, partial execution, indeterminate outcomes, retry exhaustion, failed compensation, unavailable logging, conflicting owner state, and recovery authorization.

Record exact artifact/version, environment, procedure, expected/observed result, evidence reference, and limitations. Keep smoke tests, feature tests, compatibility/security validation, independent verification, and release approval separate. Never test against production or real user data by default.

## 13. Unresolved decisions

Purpose-specific severity, response targets, escalation ownership, support hours, retry counts/backoff/timeouts, budgets, rate/cost thresholds, idempotency keys, deduplication windows, ordering/conflict rules, compensation catalogue, verification thresholds, jurisdiction, and retention remain open.

D-012 open questions remain open: provenance requiredness by artifact/operation/risk; concrete lifecycle transitions and deletion/recovery criteria; delayed/duplicate/conflicting cross-system events; independent verification evidence; and tombstone/audit retention after deletion.

## 14. Related documents

- [Workflow Management System](../workflows/WORKFLOW_MANAGEMENT.md)
- [Identity Contract](../systems/permissions/IDENTITY_CONTRACT.md)
- [Authorization Contract](../systems/permissions/AUTHORIZATION_CONTRACT.md)
- [Permission Architecture](../systems/permissions/PERMISSION_ARCHITECTURE.md)
- [Provenance Contract](PROVENANCE_CONTRACT.md)
- [Shared Lifecycle Contract](LIFECYCLE_CONTRACT.md)
- [Logging Architecture](../systems/logging/LOGGING_ARCHITECTURE.md)
- [Validation principles](../systems/validation/README.md)
