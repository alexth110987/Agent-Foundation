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

## Approved design decisions — AF-43 through AF-47
- **AF-43 — Approved design decision:** Policy changes remain proposals until explicitly approved by the owner. An approved change receives a new policy version; publication is permitted only after an integrity check, and the previous version remains available as historical evidence. Automatic policy changes and self-approval are prohibited. This does not approve the Master Prompt draft or authorize implementation.
- **AF-44 — Approved design decision:** The owner is represented by a unique internal identity that is independently verified. A login name or email address alone is insufficient proof of identity. Changes to owner identity require verification; uncertainty blocks protected actions. Detailed identity lifecycle, recovery, and verification mechanisms remain open. This does not approve the Master Prompt draft or authorize implementation.
- **AF-45 — Approved design decision:** The initial versioned authorization reason-code catalogue includes: PERMIT-001 (all required checks passed); DENY-001 (authorization absent); DENY-002 (policy conflict); DENY-003 (integrity check failed); DENY-004 (approval expired); DENY-005 (actor identity untrusted); INDET-001 (insufficient information); and INDET-002 (verification unavailable). Codes are not silently reused or redefined; catalogue changes require explicit versioning. This does not approve the Master Prompt draft or authorize implementation.
- **AF-46 — Approved design decision:** Policy conflicts are recorded explicitly and block the affected protected action until the owner resolves the conflict. The system must not choose a winning rule or policy source autonomously. Resolution must be owner-authorized and supported by a traceable decision/evidence record. This supplements AF-09 and does not weaken its single-authoritative-policy-source or deny-on-conflict requirements. This does not approve the Master Prompt draft or authorize implementation.
- **AF-47 — Approved design decision:** Every policy proposal records a unique proposal ID, trigger/problem statement, proposed change, rationale/evidence, expected effects, security/privacy impact, approval state, and final decision/evidence reference. A proposal is non-operative until owner approval and authorized publication into the approved policy source. This does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Specify detailed policy version syntax and transition mechanics; signing/publication mechanics in coordination with Component 2; owner identity lifecycle, recovery and verification mechanism; reason-code compatibility/migration rules; and detailed proposal review/audit record format.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
