# 05 — Timestamp Engine

**ID:** AF-05  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Provide consistent time and event chronology.

## Responsibilities
Generate unique, non-semantic event IDs and UTC timestamps; use UTC as the canonical internal storage and interchange representation for all system event times. Serialize timestamps using RFC 3339 UTC form with a `Z` designator. Preserve the precision supplied by the source; do not invent fractional precision. Distinguish event, observed, created, modified, verified, and recorded times; record clock source, synchronization status when known, and uncertainty. Use a monotonic clock for elapsed durations where supported. Local time may be derived for display only and must not replace the canonical UTC value.

## Inputs
Clock source, event context, and available clock-quality metadata.

## Outputs
Timestamped event metadata with explicit semantics, unique event ID, source information, and uncertainty.

## Security and operational boundaries
Never fabricate timestamps or treat Git/filesystem timestamps as proof of original device creation. Preserve the original UTC timestamp and its semantics; timezone conversion is presentation-only. Where the source offset, synchronization status, or clock uncertainty is unknown, record that limitation rather than infer certainty. Event IDs must not encode secrets or unnecessary personal data. Monotonic readings are for duration measurement and are not interchangeable with wall-clock timestamps.

## Approved design decision
All internal system event timestamps use UTC as the canonical storage and interchange representation. Local time is display-only and must not replace or alter the canonical UTC value. RFC 3339 UTC serialization with `Z` is the standard representation; source precision is preserved without fabricated precision. Clock source and known synchronization/uncertainty metadata are recorded. Unique event IDs are non-semantic; monotonic time is used for durations where supported. This approval applies only to this design decision; it does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Exact fractional-second precision policy for sources with differing precision, clock synchronization requirements, event ID generation mechanism, monotonic clock availability/fallback, and event metadata retention.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including invalid timestamps, unknown clock quality, and duplicate event IDs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
