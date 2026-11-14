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

## Additional approved design decisions — AF-53 through AF-57
- **AF-53 — Approved design decision:** Every time observation used by the system carries an explicit clock-quality state: TRUSTED, DEGRADED, UNKNOWN, or UNTRUSTED. The state must reflect available source, synchronization, and uncertainty evidence; it must never be overstated. Time-dependent protected actions require TRUSTED time and are denied otherwise. This does not approve the Master Prompt draft or authorize implementation.
- **AF-54 — Approved design decision:** Only explicitly approved and identifiable time sources may support security-sensitive time decisions. Source identity and relevant synchronization/uncertainty metadata are recorded. Conflicting or unverifiable source evidence results in UNKNOWN or UNTRUSTED status and cannot be silently resolved by choosing an arbitrary system clock. The approved source list and measurable trust criteria remain open. This does not approve the Master Prompt draft or authorize implementation.
- **AF-55 — Approved design decision:** Monotonic clocks are used only for elapsed-duration measurement and never substitute for canonical UTC event time. Loss of monotonic continuity, including across reboot or suspend where continuity cannot be established, invalidates or marks the affected duration measurement uncertain. No security-sensitive duration may silently fall back to wall-clock time. Supported platform behavior remains open. This does not approve the Master Prompt draft or authorize implementation.
- **AF-56 — Approved design decision:** Event metadata records, at minimum, event ID, event type, event time, recording time, time source, source precision, clock-quality state, originating component, and a reference to the associated action where applicable. Unknown or unavailable values are explicitly represented rather than inferred. Event records must not include unnecessary personal data. Exact schema and field encoding remain open. This does not approve the Master Prompt draft or authorize implementation.
- **AF-57 — Approved design decision:** Event metadata retention is governed by a separately reviewed, owner-approved audit/privacy policy with retention periods defined by data category and appropriate least-privilege access. No blanket indefinite retention is introduced. Any deletion, redaction, or retention exception must preserve applicable approved integrity, audit, legal, and privacy requirements and be separately authorized where required. Detailed periods and procedures remain open. This does not approve the Master Prompt draft or authorize implementation.

These approvals apply only to the stated design decisions; they do not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Define measurable clock synchronization/quality criteria and the approved trusted-time source list; specify supported monotonic clock sources/platforms and behavior across suspend/reboot; define the exact event metadata schema and field encoding; and establish the audit/privacy retention policy, including category-specific periods and access controls. The event ID generation mechanism remains an implementation detail for later design review.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including invalid timestamps, precision preservation, every clock-quality state, conflicting/untrusted sources, denial when trusted time is unavailable, duplicate event IDs, and unavailable/lost monotonic continuity.
- Privacy/security controls and evidence requirements are specified, including category-specific retention limits and access controls under the separately approved audit/privacy policy.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
