# 05 — Timestamp Engine

**ID:** AF-05  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Provide consistent time and event chronology.

## Responsibilities
Generate unique event IDs and UTC timestamps; distinguish event, observed, created, modified, verified, recorded times; record clock source/uncertainty; use monotonic time for durations where supported.

## Inputs
Clock source and event context.

## Outputs
Timestamped event metadata with explicit semantics and uncertainty.

## Security and operational boundaries
Never fabricate timestamps or treat Git/filesystem timestamps as proof of original device creation.

## Open design decisions
UTC representation, clock assumptions, event ID format, precision and retention.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
