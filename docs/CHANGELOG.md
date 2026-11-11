# Changelog

All notable project changes are recorded here.

## 0.24.0 — 2026-11-11

### Approved
- Recorded D-009: the exact pre-approval blob `473598af630b44b78910ab1df3b7f53bbc1a0802` of `docs/OBJECT_LIFECYCLE_TABLES.md` is approved as a documentation-only logical design baseline.

### Clarifications
- D-009 does not approve the underlying proposed contracts or Cross-System Interface Matrix, finalize unresolved object-class policies, select technology, or authorize implementation, runtime, integrations, operational use, testing, validation, or release.
- The approved source blob is retained as the exact baseline; the current document header/status annotation records the decision without changing the approved scope.

# Changelog

All notable project changes are recorded here.

## 0.23.0 — 2026-11-11

### Updated
- Completed the final consistency review of the proposed lifecycle tables and made the four dimensions explicit in the common model: object lifecycle state, process status, transaction outcome and verification result.
- Clarified remaining mixed-dimension rows for WMS run creation/admission, KMS source review and provenance-lineage registration.
- Cross-checked lifecycle semantics against the proposed Shared Lifecycle, Provenance, Identity, Authorization, Cross-System Interface and Joint Review documents; no conflict with D-006/D-007/D-008 or accepted R-01–R-07 principles was identified.
- Updated the project milestone to formal owner review.

### Clarifications
- The lifecycle tables and related contracts remain Proposed pending explicit owner decisions. This review does not approve them, amend D-006/D-007/D-008, or authorize implementation.
- No runtime, state machine, service, integration, or test was implemented or executed.

## 0.22.0 — 2026-11-11

### Updated
- Corrected remaining mixed-dimension lifecycle rows: WMS cancellation, KMS source ingestion and index build/rebuild, Identity principal proofing and authentication attempts, and Logging event recording now distinguish object lifecycle state, process status, transaction outcome and verification result.
- Aligned the provisional vocabulary mapping with those explicit distinctions and clarified that proofing, authentication, build and recording outcomes do not themselves become object lifecycle states.
- Updated the project milestone to require a final consistency review before formal owner review.

### Clarifications
- The lifecycle tables remain Proposed and pending owner review. These documentation changes do not approve the tables or underlying contracts, amend D-006/D-007/D-008, or authorize implementation.
- No runtime, state machine, service, integration, or test was implemented or executed.

## 0.21.0 — 2026-11-11

### Updated
- Completed the second consistency-review refinement of the proposed object-specific lifecycle tables: expanded authentication evidence, separated process/object state from transaction outcomes and verification results in the identified mixed-dimension rows, and made risk-based independent verification explicit.
- Updated the project milestone to record the completed consistency review and identify formal owner review as the next step.

### Clarifications
- The lifecycle tables remain Proposed and pending owner review. These changes do not approve the tables or underlying contracts, amend D-006/D-007/D-008, or authorize implementation.
- No runtime, state machine, service, integration, or test was implemented or executed.


## 0.20.0 — 2026-11-11

### Updated
- Refined the proposed object-specific lifecycle tables to separate object lifecycle state, transaction outcome and verification result; map object-specific vocabulary; clarify logical ownership; define minimum protected-transition evidence; and strengthen owner-local deletion/reconciliation evidence.
- Updated the project milestone to require a second consistency review and explicit owner review of the refined tables.

### Clarifications
- The lifecycle tables remain Proposed. These documentation refinements do not approve the tables or underlying contracts, amend D-006/D-007/D-008, or authorize implementation.
- No runtime, state machine, service, integration, or test was implemented or executed.

## 0.19.0 — 2026-11-11

### Added
- Added proposed object-specific lifecycle transition tables for Core governance, workflow definitions/runs, KMS sources and indexes, MMS records, identity/authentication, approval/authorization, provenance, derivatives, and audit records.
- Documented cross-owner reconciliation obligations, common transition controls, unresolved object-class decisions, and design-level validation cases.

### Updated
- Linked the proposed lifecycle tables from the README and project status.

### Clarifications
- The tables are proposals pending owner review. They do not approve the Shared Lifecycle Contract or any underlying contract, amend D-006/D-007/D-008, or authorize implementation.
- No state machine, service, schema, runtime, operational workflow, or test was implemented or executed.

## 0.18.0 — 2026-11-11

### Added
- Recorded a proposed joint contract refinement register for identity/authentication freshness, executor identity, bounded PEP enforcement, KMS-to-MMS provenance, derivative lifecycle, owner-specific transition tables, event/recovery semantics, and independent validation.
- Corrected malformed literal newline escapes in the project status record and aligned the review milestone and documentation map.

### Clarifications
- The refinement register is pending owner review and does not approve or amend the five proposed contracts or D-006, D-007, or D-008.
- No implementation, runtime, integration, operational workflow, or test was added.

