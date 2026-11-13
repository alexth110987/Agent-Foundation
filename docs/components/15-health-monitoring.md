# 15 — Health Monitoring

**ID:** AF-15  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 3

## Purpose
Report observed operational condition of authorized components.

## Responsibilities
Observe availability, dependency state, queue/run condition, resource limits and recent failures; detect stale/missing/inconsistent signals; report freshness and evidence source; distinguish availability from correctness.

## Inputs
Authorized health signals, component inventory, monitoring policy.

## Outputs
Health snapshot, alerts, freshness metadata and trend evidence.

## Security and operational boundaries
Read-only by default; response actions require separately approved workflow. Online does not mean correct. No intrusive monitoring beyond approved scope.

## Open design decisions
Health indicators, polling/event model, freshness thresholds, alert severity, retention, privacy.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
