# Changelog

All notable project changes are recorded here.

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

