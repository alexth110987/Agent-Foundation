# Changelog

All notable project changes are recorded here.
 
## 0.39.0 — 2026-11-12

### Added
- Recorded a read-only follow-up Foundation Consistency Review (FCR-06 through FCR-11) covering the Core authority map, Foundation Completion Plan currency, WMS outcome/retry/registry requirements, and Error Handling validation terminology.

### Updated
- Updated Project Status and Foundation Completion Plan to identify the follow-up findings as pending owner disposition.

### Clarifications
- Findings are proposals for owner review only; no affected design document or approved baseline was changed. No D-015 was created. D-012 open questions, D-014 policy scope, Stage 1, and all implementation/testing/release safeguards remain unchanged. No tests or runtime checks were executed.


## 0.38.0 — 2026-11-12

### Approved
- Recorded D-014: owner approval of the exact Validation Strategy proposal blob `69398a8416a9f2d138d399084aa9e6a31a913b66` and Documentation and Version Governance proposal blob `beb94648d9308fed79695ccb1d9184c0f0725c89` as purpose-neutral foundation policies.
- Updated both current policy documents with explicit D-014 approval annotations while preserving the exact approved source baselines.

### Updated
- Aligned Project Status, Foundation Completion Plan, and README with D-014 and the next foundation milestone.

### Clarifications
- Approval is documentation-only. No validation framework, test runner, automated validation, document registry, versioning automation, runtime, integration, or executable capability was implemented. No tests were run.
- Corrected the policy review wording and clarified the historical pre-approval status entry in Project Status; no policy scope or approved baseline changed.
- Deferred policy choices and all five D-012 open questions remain open. Stage 1 remains active; purpose and runtime remain undefined. No implementation, production testing, or release is authorized.


## 0.37.0 — 2026-11-12

### Added
- Added a proposed purpose-neutral Validation Strategy defining risk-based validation layers, evidence records, result states, independence, revalidation, and separate feature, compatibility, security, smoke, operational-readiness, licensing, and release gates.
- Added a proposed Documentation and Version Governance policy covering stable document identity, metadata, document versioning, change classes, owner approval, exact baseline integrity, supersession, and repository records.

### Updated
- Linked both proposals from the README and Foundation Completion Plan.
- Updated Project Status to record both proposals as pending owner review and corrected the stale next-milestone text to reflect the approved FCR-01–FCR-05 dispositions.

### Clarifications
- Both policies remain Proposed; no D-014 decision or adoption approval is created.
- No test runner, automated validation, document registry, CI workflow, runtime, integration, or executable capability was added. No tests were run. Stage 1 remains active.


## 0.36.0 — 2026-11-12

### Corrected
- Recorded owner-approved dispositions for FCR-01 through FCR-05 in the Foundation Consistency Review.
- Added non-substantive post-approval status annotations to the exact D-012 Provenance, Shared Lifecycle, and Cross-System Interface baselines, preserving their approved source blob SHAs and design content.
- Corrected Joint Contract Review wording so the three D-012 baselines are approved while the register itself remains Proposed and non-amending.
- Updated Implementation Readiness to distinguish approved logical Permission, Identity, Authorization, Provenance, Shared Lifecycle, and Cross-System Interface baselines from unresolved implementation choices.
- Recorded the Core authority/document boundary and the future PEP owner/exact-context requirement in the proposed Core, Architecture, and Component Model documentation.
- Aligned Project Status, Foundation Completion Plan, README, and Decision Log with the approved dispositions.

### Clarifications
- D-006 through D-012 and R-01–R-07 are not amended; the exact D-012 baseline blobs are unchanged. The five D-012 open questions remain unresolved.
- The Joint Contract Review register and other previously proposed designs remain Proposed. No purpose, runtime, technology, process topology, service, or enforcement implementation was selected.
- Stage 1 remains active. No implementation, runtime, integration, operational use, test execution, validation, or release was authorized or performed.

## 0.35.0 — 2026-11-12

### Added
- Recorded a static Foundation Consistency Review covering cross-system ownership, identity/authorization, lifecycle/provenance, error handling/logging/recovery, validation, tools, workflow governance, and documentation status.

### Updated
- Linked the review from README, Project Status, and Foundation Completion Plan.
- Recorded three confirmed status/planning inconsistencies and two design/governance ambiguities for explicit owner disposition.

### Clarifications
- The review does not amend exact approved baselines, resolve D-012 open questions, approve proposals, or authorize implementation, runtime, integrations, testing, validation, or release.
- No runtime or tests were executed; findings are limited to static documentation review.

## 0.34.0 — 2026-11-11

### Added
- Added a proposed purpose-neutral Error Handling and Recovery Architecture covering failure taxonomy, typed outcomes, fail-closed behavior, retry/replay safety, uncertain side effects, reconciliation, recovery, escalation, and future validation.
- Added a proposed Logging Architecture covering event taxonomy, correlation/causation, integrity, access separation, privacy, retention, logging failure, and future validation.
- Added proposed Tool Governance lifecycle and review controls; clarified that the Tool Registry is empty and technical availability does not authorize invocation.

### Updated
- Replaced the Logging placeholder with a pointer to the proposed logical architecture.
- Updated the Tool Registry to state its empty status and link proposed governance controls.
- Aligned Project Status, Foundation Completion Plan, and README with the new proposals and current D-012 registration.

### Clarifications
- All three architectures remain Proposed and require owner review. No tool was selected, registered, approved, configured, connected, validated, or activated.
- These are documentation-only changes. They do not resolve D-012 open questions, select technology, authorize runtime, implementation, integration, ingestion, operational use, tests, validation, or release.
- No runtime tests or production actions were performed.

## 0.33.0 — 2026-11-11

### Corrected
- Aligned historical and current project-status wording with D-012: the exact Provenance Contract, Shared Lifecycle Contract, and Cross-System Interface Matrix blobs are approved documentation baselines; only the Joint Contract Review register remains Proposed.
- Replaced the stale next milestone with the actual unresolved D-012 questions and remaining foundation work.

### Clarifications
- This is a status/documentation consistency correction only. It does not change any approved baseline, resolve open design questions, or authorize implementation, runtime, integration, testing, validation, or release.
- No executable components or tests were added or run.

## 0.32.0 — 2026-11-11

### Approved
- Registered D-012 for the exact owner-approved Provenance Contract, Shared Lifecycle Contract, and Cross-System Interface & Ownership Matrix source blobs.
- Updated project status to distinguish the three approved documentation baselines from the still-proposed Joint Contract Review register.

### Clarifications
- D-012 is limited to the three exact logical documentation baselines. It does not amend D-006 through D-011 or R-01–R-07, resolve the listed open questions, approve the Joint Contract Review register, select technology, or authorize implementation, integration, ingestion, runtime, testing, validation, operational use, or release.
- No executable components or tests were added or run.

## 0.31.0 — 2026-11-11

### Added
- Added a proposed Foundation Completion Plan and durable handover snapshot covering four review batches and 20 logical review areas.
- Recorded the outstanding foundation gaps, recommended work order, minimum implementation-readiness gate, and exact D-012 approval baselines awaiting repository registration.
- Linked the plan from the README and recorded its scope in project status.

### Clarifications
- The plan is documentation only. It does not select the operational purpose or technology, approve proposed architecture, register D-012, or authorize implementation, runtime, integrations, tests, operational use, or release.
- No executable components or tests were added or run.


## 0.30.0 — 2026-11-11

### Updated
- Corrected cross-document status references: D-010 Identity and D-011 Authorization are approved logical contract baselines; the Provenance Contract, Shared Lifecycle Contract, and Cross-System Interface Matrix remain Proposed.
- Aligned the Lifecycle Contract and Cross-System Interface Matrix references with the exact approved D-010/D-011 baselines without amending those decisions.
- Updated the Joint Contract Review scope, owner checklist, and status wording to distinguish approved baselines from proposals awaiting owner approval.
- Recorded the consistency review in project status.

### Clarifications
- No approved baseline was amended, no proposed contract was approved, and no technology or implementation was selected or authorized.
- No runtime tests or operational validation were performed.

## 0.29.0 — 2026-11-11

### Updated
- Jointly refined the proposed Provenance Contract, Shared Lifecycle Contract, and Cross-System Interface & Ownership Matrix.
- Aligned provenance and lifecycle semantics for stable object/version/event/correlation/causation references; event/effective/observed/recorded times; typed operation outcomes; exact lineage; actor separation; idempotency, replay, ordering, and concurrency; partial/unknown outcomes; owner-local reconciliation and recovery; and independent validation.
- Clarified that KMS-to-MMS provenance is evidence, not MMS write authority, and that source lifecycle notices do not prove derivative changes or global completion.

### Clarifications
- All three documents remain Proposed and pending owner review. The refinement does not amend approved D-006 through D-011 baselines, select technology, or authorize implementation, integration, ingestion, testing, runtime, operational use, or release.
- No executable components or tests were added or run.


## 0.28.0 — 2026-11-11

### Approved
- Recorded D-011: the exact pre-approval blob `49e03d3d60f06ebf4b4ba8db92ac41349f09bc39` of `systems/permissions/AUTHORIZATION_CONTRACT.md` is approved as a documentation-only logical contract baseline.

### Clarifications
- D-011 is limited to that exact logical contract version. It does not amend D-008 or D-010, select authorization/identity/approval technology, or authorize credentials, services, integrations, tool access, runtime, implementation, testing, validation, or release. Deferred decisions remain open.
- The current Authorization Contract header records D-011 and points to the exact approved source blob; the annotation does not change the approved baseline or its scope.

## 0.27.0 — 2026-11-11

### Updated
- Refined the proposed Authorization Contract to consume D-010-approved logical authentication-result references, require independent executor authentication, and keep initiator, executor, and approver identities distinct.
- Tightened Permit binding to exact operation/resource/owner, purpose, project/tenant/subject, environment, constraints, PEP/audience, policy/context versions, and validity.
- Expanded freshness, revocation, cache, paused/resumed operation, policy/attribute-source failure, approval-evidence, recovery, and validation requirements.

### Clarifications
- The Authorization Contract remains Proposed pending owner review. The refinement does not amend D-008 or D-010, approve the contract, select technology, or authorize implementation, services, credentials, integrations, runtime, testing, validation, or release.
- No policy engine, approval service, PDP, PEP, runtime, integration, or test was implemented or executed.

## 0.26.0 — 2026-11-11

### Approved
- Recorded D-010: the exact pre-approval blob `b01e5ff7c99f9a208c4424878e87ded5e4c4629d` of `systems/permissions/IDENTITY_CONTRACT.md` is approved as a documentation-only logical contract baseline.

### Clarifications
- D-010 is limited to that exact logical contract version. It does not amend D-008, select identity technology, or authorize credentials, services, integrations, runtime, implementation, testing, validation, or release. Deferred identity decisions remain open.
- The current Identity Contract header records approval and points to the exact approved source blob; the annotation does not change the approved baseline or its scope.

## 0.25.0 — 2026-11-11

### Updated
- Refined the proposed Identity Contract authentication result requirements: explicit issuer/verifier, principal/assertion version, audience/scope, authenticated-at and validity boundaries, freshness, current status/revocation evidence, and independent relying-component validation.
- Clarified independent executor authentication, initiator/executor/approver separation, material identity-context changes, revalidation, and fresh PDP evaluation after relevant identity changes.
- Expanded failure and validation cases for stale/unavailable status, revocation, executor identity, and long-running or resumed operations.
- Updated the project status to reflect that the Identity Contract is refined but remains pending formal owner review.

### Clarifications
- The Identity Contract remains Proposed. This documentation refinement does not amend D-008, approve the contract, select identity technology, or authorize identity services, credentials, integrations, runtime, implementation, testing, validation, or release.
- No runtime, identity provider, authenticator, policy engine, or test was implemented or executed.

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

