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

## Approved design decision
The Master Control System may identify and propose additional policy rules when it detects a gap or recurring need. A proposed rule is non-operative: it must not influence authorization, workflow execution, or other protected decisions until the owner explicitly approves it and the approved policy source is updated through the authorized process. The system cannot approve, activate, or grant authority to its own proposals. This approval applies only to this design decision; it does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Policy source and update process, actor identity, reason-code taxonomy, conflict handling, proposal format and review evidence.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
