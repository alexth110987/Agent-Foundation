# 08 — Security and Permission Control

**ID:** AF-08  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Enforce least privilege and deny-by-default at each protected action boundary.

## Responsibilities
Evaluate actor, action, resource, purpose, environment, scope, constraints and validity. Separate read/propose/write/execute/delete/publish/admin. Bind approvals to exact action, resource, purpose, context and expiry; enforce immediately before side effect.

Every write, execute, delete, and publish action requires its own explicit approval. Approval for one action does not authorize another action, another resource, or a later action. No standing or blanket approval substitutes for this per-action requirement. Each approval must be bound to the exact requested operation and its relevant context, and checked immediately before execution.

Actors have unique, verifiable identities. Roles and permissions define the maximum authority available to an actor; role membership alone does not replace the separate explicit approval required for each write, execute, delete, or publish action.

## Inputs
Verifiable actor identity, requested action/resource, purpose and execution context, applicable policy, and action-specific approval evidence.

## Outputs
Permit/Deny/Indeterminate with scope, constraints, expiry, and audit reference.

## Security and operational boundaries
Reject stale, mismatched, expired, revoked, replayed, or ambiguous approvals. Each approval is single-use and bound to the actor, exact action, resource, purpose, relevant context, and expiry. Revocation invalidates an approval immediately. Enforce identity and approval checks immediately before the side effect; an earlier check is insufficient. Tool access or prior approval grants no unrelated permission. If authorization cannot be established at the action boundary, deny the operation. Read access remains separately governed by least-privilege policy and does not imply write authority. Record approval, authorization decision, and execution result as distinct audit events.

## Approved design decision
Actors require unique, verifiable identities. Roles define maximum permitted authority but do not replace action-specific explicit approval. Each write, execute, delete, or publish action requires a single-use approval bound to the actor, exact action, resource, purpose, relevant context, and expiry. Revocation invalidates approval immediately; replay, mismatch, expiry, ambiguity, or inability to verify authorization results in Deny. Checks occur immediately before the side effect. Approval, authorization decision, and execution result are distinct audit events. This approval supplements AF-08's per-action approval decision; it does not approve the Master Prompt draft or authorize implementation.

## Additional approved design decisions

The owner approved the following five refinements on 2026-11-14. These are design decisions only; they do not approve the Master Prompt 0.1.0-draft, authorize implementation, select an identity provider or technology, or approve release.

- **AF-72 — Approved design decision:** Changes to roles, permissions, and actor-to-role assignments are themselves protected administrative actions. They require explicit authorization, attributable evidence, and validation against the current policy immediately before taking effect. An actor must not use a permission-management action to expand their own authority or bypass an applicable restriction. Any unresolved authority, scope, or policy conflict results in Deny. This does not approve implementation.
- **AF-73 — Approved design decision:** Delegated authority, where separately permitted by policy, must be explicitly bounded to the delegating actor, delegate, permitted action, resource, purpose, context, and validity period. Delegation cannot exceed the delegator's current authority, cannot convert a restricted or non-delegable permission into a delegable one, and cannot be re-delegated unless an explicit approved rule permits it. Missing, ambiguous, expired, or unverifiable delegation evidence results in Deny. This does not establish that delegation is currently enabled or approve implementation.
- **AF-74 — Approved design decision:** Revocation of an identity, role, permission, delegation, or approval must prevent subsequent protected actions using that authority. Authorization must use current, verifiable revocation state at the action boundary. If timely revocation state cannot be established, the protected action is denied; stale cached authorization must not be used as a fallback. This does not select a propagation mechanism or measurable service-level target, or approve implementation.
- **AF-75 — Approved design decision:** Read access is independently scoped by actor, resource, data category, purpose, and permitted read operation. Access to one resource or data category does not imply access to related resources, metadata, search results, exports, or other representations. Read, search, inspect, and export remain distinguishable where their exposure differs. Ambiguous or excessive scope is denied. This does not approve implementation.
- **AF-76 — Approved design decision:** A single-use approval must be consumed atomically with its successful authorization at the protected action boundary, so concurrent or replayed requests cannot both use the same approval. If the system cannot establish whether an approval was already consumed, or cannot guarantee one-time use for the action, it must deny the protected operation rather than risk duplicate execution. This does not select a nonce/token mechanism or transaction technology, or approve implementation.

## Open design decisions
Identity proofing and recovery, role/permission administration and change governance, whether and under what conditions delegation is permitted, approval UX and authentication strength, exact context-binding fields, expiry defaults, revocation propagation guarantees and measurable freshness targets, one-time token/nonce and atomic-consumption mechanism, audit integrity and access controls, and read/search/inspect/export granularity.

## Acceptance criteria before implementation
- Role, permission, and actor-role changes are protected, attributable actions and cannot enable self-escalation or bypass policy.
- Any permitted delegation is explicitly bounded, non-escalating, and non-transitive unless separately authorized by policy.
- Revocation is checked against current verifiable state at the action boundary; unavailable or stale state denies access.
- Read, search, inspect, and export scopes are independently defined and do not imply access to other resources or representations.
- Single-use approvals cannot be accepted concurrently or replayed; uncertain consumption state denies the protected action.
- Interfaces, state transitions, failure modes and permissions are approved.
- Every write, execute, delete and publish action requires a distinct, single-use explicit approval bound to the exact actor, action and context.
- Positive and negative tests are defined, including unauthorized, stale, revoked, replayed and mismatched approvals.
- Revocation and action-boundary revalidation behavior is specified.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
