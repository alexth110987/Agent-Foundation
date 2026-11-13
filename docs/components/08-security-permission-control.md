# 08 — Security and Permission Control

**ID:** AF-08  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Enforce least privilege and deny-by-default at each protected action boundary.

## Responsibilities
Evaluate actor, action, resource, purpose, environment, scope, constraints and validity. Separate read/propose/write/execute/delete/publish/admin. Bind approvals to exact action, resource, context and expiry; enforce immediately before side effect.

Every write, execute, delete, and publish action requires its own explicit approval. Approval for one action does not authorize another action, another resource, or a later action. No standing or blanket approval substitutes for this per-action requirement. Each approval must be bound to the exact requested operation and its relevant context, and checked immediately before execution.

## Inputs
Identity, requested action/resource, context, policy, approval evidence.

## Outputs
Permit/Deny/Indeterminate with scope, constraints, expiry and audit reference.

## Security and operational boundaries
Reject stale, mismatched, expired, replayed, ambiguous approvals. Tool access or prior approval grants no unrelated permission. If authorization cannot be established at the action boundary, deny the operation. Read access remains separately governed by least-privilege policy and does not imply write authority.

## Open design decisions
Identity/role model, read-access granularity, approval UX, expiry/revocation, enforcement points, replay protection, and audit evidence.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Every write, execute, delete and publish action requires a distinct explicit approval bound to the exact action and context.
- Positive and negative tests are defined, including unauthorized, stale, replayed and mismatched approvals.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
