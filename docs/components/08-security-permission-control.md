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

## Open design decisions
Identity proofing and recovery, role/permission administration, approval UX and authentication strength, exact context-binding fields, expiry defaults, revocation propagation guarantees, one-time token/nonce mechanism, audit integrity and access controls, and read-access granularity.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Every write, execute, delete and publish action requires a distinct, single-use explicit approval bound to the exact actor, action and context.
- Positive and negative tests are defined, including unauthorized, stale, revoked, replayed and mismatched approvals.
- Revocation and action-boundary revalidation behavior is specified.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
