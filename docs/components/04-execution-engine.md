# 04 — Execution Engine and Run Management

**ID:** AF-04  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Execute only authorized workflow steps and manage run lifecycle.

## Responsibilities
Validate inputs, preconditions, exact artifact versions/integrity, dependencies, permissions. Execute approved scripts, commands, tests, diagnostics, and authorized GitHub Actions in isolation. Enforce allowlists, limits, timeout, cancellation, side-effect controls; capture outcomes/evidence.

## Inputs
Authorized execution plan, pinned artifacts, synthetic inputs, specifically approved scoped credentials.

## Outputs
Run record with start/end, exit code, duration, step outcomes, output and verification references.

## Security and operational boundaries
Tool availability is not authorization. No production access. No blind retry of non-idempotent actions. Scheduled execution disabled until approved.

## Open design decisions
Isolation, runner lifecycle, resource limits, cancellation, provenance, retry/idempotency.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
