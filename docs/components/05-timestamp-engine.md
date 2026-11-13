# 05 — Timestamp Engine

**ID:** AF-05  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Provide consistent time and event chronology.

## Responsibilities
Generate unique, random, non-semantic event IDs and UTC timestamps; use UTC as the canonical internal storage and interchange representation for all system event times. Serialize timestamps using RFC 3339 UTC form with a `Z` designator. Preserve the exact precision supplied by the source; do not silently round or fabricate fractional digits. Distinguish event, observed, created, modified, verified, and recorded times; record clock source and known synchronization status/uncertainty. Use a monotonic clock for elapsed-duration measurement where supported. Local time may be derived for display only and must not replace the canonical UTC value.

## Inputs
Clock source, event context, and available clock-quality metadata.

## Outputs
Timestamped event metadata with explicit semantics, unique event ID, source information, and uncertainty.

## Security and operational boundaries
Never fabricate timestamps or treat Git/filesystem timestamps as proof of original device creation. Preserve the original timestamp and its semantics; timezone conversion is presentation-only. Where source offset, synchronization status, or clock uncertainty is unknown or unreliable, record that limitation and never represent clock quality as reliable. Untrusted wall-clock time must not be used to decide expiry or other time-bound authorization. If trustworthy time cannot be established for such a decision, deny the protected action. Event IDs must be unique, random and non-semantic; they must not encode time, secrets or unnecessary personal data. Monotonic readings are for elapsed-duration measurement and are not interchangeable with wall-clock timestamps. If a monotonic clock is unavailable, mark duration measurement unreliable; do not silently substitute wall-clock time for a security-sensitive duration measurement. Event metadata retention is governed by a separately reviewed audit/privacy policy; this component does not establish blanket indefinite retention.

## Approved design decisions
- **AF-05:** All internal system event timestamps use UTC as the canonical storage and interchange representation. Local time is display-only and must not replace or alter the canonical UTC value. RFC 3339 UTC serialization with `Z` is the standard representation; source precision is preserved without fabricated precision. Clock source and known synchronization/uncertainty metadata are recorded. Unique event IDs are non-semantic; monotonic time is used for durations where supported.
- **AF-11:** Preserve exact source precision without silent rounding or fabricated fractional digits. Record clock source and known synchronization status; unknown or unreliable clock quality must not be represented as reliable. Untrusted wall-clock time must not determine expiry or time-bound authorization; if trustworthy time cannot be established, deny the protected action. Event IDs must be unique, random, non-semantic, and contain no personal data or embedded time. Monotonic clocks are for elapsed durations only; if unavailable, mark duration measurement unreliable and do not silently substitute ordinary wall-clock time for security-sensitive duration measurements. Metadata retention is deferred to a separately reviewed audit/privacy policy and is not blanket-indefinite by default.

These approvals apply only to the stated design decisions; they do not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Define measurable clock synchronization/quality criteria and trusted-time sources for security-sensitive decisions; specify supported monotonic clock sources/platforms and behavior across suspend/reboot; define the event metadata schema and the audit/privacy retention policy. The event ID generation mechanism remains an implementation detail for later design review.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including invalid timestamps, precision preservation, unknown/unreliable clock quality, untrusted time for expiry, denial when trusted time is unavailable, duplicate event IDs, and unavailable monotonic clocks.
- Privacy/security controls and evidence requirements are specified, including retention limits and access controls under the separately approved audit/privacy policy.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
