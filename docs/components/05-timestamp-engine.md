# 05 — Timestamp Engine

**ID:** AF-05  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Provide consistent time and event chronology.

## Responsibilities
Generate unique event IDs and UTC timestamps; use UTC as the canonical internal storage and interchange representation for all system event times. Distinguish event, observed, created, modified, verified, and recorded times; record clock source and uncertainty; use monotonic time for durations where supported. Local time may be derived for display only and must not replace the canonical UTC value.

## Inputs
Clock source and event context.

## Outputs
Timestamped event metadata with explicit semantics and uncertainty.

## Security and operational boundaries
Never fabricate timestamps or treat Git/filesystem timestamps as proof of original device creation. Preserve the original UTC timestamp and its semantics; timezone conversion is presentation-only. Where the source offset or clock uncertainty is unknown, record that limitation rather than infer certainty.

## Approved design decision
All internal system event timestamps use UTC as the canonical storage and interchange representation. Local time is display-only and must not replace or alter the canonical UTC value. This approval applies only to this design decision; it does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
UTC serialization precision/format, clock assumptions and synchronization, event ID format, monotonic clock behavior, precision and retention.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
