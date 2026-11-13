# 01 — Master Control System

**ID:** AF-01  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Central policy coordinator for incoming tasks.

## Responsibilities
Normalize request, actor, target resource, purpose, expected outcome, and side effects. Load applicable policy/status/workflow/permission context. Decide Permit, Deny, or Indeterminate with reason codes and evidence. Coordinate other components without bypassing enforcement.

## Inputs
Request, approved policy, project state, workflow, permission context.

## Outputs
Decision record: ID, outcome, reasons, resource, approvals, timestamp.

## Security and operational boundaries
Default deny. Missing, stale, conflicting, or unverifiable policy blocks protected work. Cannot rewrite policy or grant itself authority.

## Approved design decision — AF-01
The Master Control System may identify and propose additional policy rules when it detects a gap or recurring need. A proposed rule is non-operative: it must not influence authorization, workflow execution, or other protected decisions until the owner explicitly approves it and the approved policy source is updated through the authorized process. The system cannot approve, activate, or grant authority to its own proposals. This approval applies only to this design decision; it does not approve the Master Prompt draft or authorize implementation.

## Approved design decision — AF-09
There is exactly one authoritative approved policy source for protected decisions. The Master Control System must identify and verify that source; it must not merge, choose between, or silently substitute competing policy sources. Authorization outcomes use a fixed, versioned reason-code taxonomy so that equivalent decisions are reported consistently. Any conflict between policy sources, policy versions, or applicable rules results in Deny for the affected protected operation until resolved through the owner-controlled process. This approval does not define or authorize the policy update mechanism, nor does it approve the Master Prompt draft or authorize implementation.

## Open design decisions
Specify the owner-controlled policy publication/update and version-transition process; actor identity and identity lifecycle; the actual reason-code catalogue, versioning and compatibility rules; policy conflict precedence/resolution evidence; and the proposal format, review record and audit evidence.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
