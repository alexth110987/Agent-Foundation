# 08 — Security and Permission Control

**ID:** AF-08  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Enforce least privilege and deny-by-default at each protected action boundary.

## Responsibilities
Evaluate actor, action, resource, purpose, environment, scope, constraints and validity. Separate read/propose/write/execute/delete/publish/admin. Bind approvals to exact action/resource/context/expiry and enforce immediately before side effect.

## Inputs
Identity, requested action/resource, context, policy, approval evidence.

## Outputs
Permit/Deny/Indeterminate with scope, constraints, expiry and audit reference.

## Security and operational boundaries
Reject stale, mismatched, expired, replayed, ambiguous approvals. Tool access or prior approval grants no unrelated permission.

## Open design decisions
Identity/role model, granularity, approval UX, expiry/revocation, enforcement points.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
