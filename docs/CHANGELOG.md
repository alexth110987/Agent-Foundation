# Changelog

All notable project changes are recorded here.

## 0.11.0 — 2026-11-11

### Added
- Drafted the purpose-neutral Permission System logical architecture, including central policy decision authority, local enforcement, bounded approvals, revocation, failure behavior, threat considerations, and validation requirements.

### Corrected
- Aligned the MMS and KMS overview documents with the formal D-006 and D-007 approvals, preserving the exact approved architecture document versions.
- Updated the project status and repository map to distinguish approved logical architectures from proposed architecture work.

### Clarifications
- Permission architecture is proposed and pending owner review; no permission service, identity provider, policy engine, or enforcement adapter exists.
- No runtime, policy language, identity model, provider, technology, integration, or implementation authority was selected.
- No runtime tests were executed.

## 0.10.0 — 2026-11-11

### Added
- Drafted a purpose-neutral implementation-readiness framework covering approval gates, requirements, interfaces, permissions, data governance, validation, operations, auditability, supply chain, and release.
- Added MMS, KMS, and cross-system readiness checklists that preserve approved logical boundaries and identify unresolved prerequisites.

### Clarifications
- The framework is proposed for owner review and does not authorize implementation, technology selection, data ingestion, integrations, runtime construction, or release.
- Outstanding items are planning blockers; no runtime exists and no runtime tests were executed.

## 0.9.0 — 2026-11-11

### Approved
- Owner formally approved the exact MMS logical architecture (D-006; blob `197ac0974f5ceea357f6ad846a299304dd13da9c`).
- Owner formally approved the exact KMS logical architecture (D-007; blob `c1dda460a8d02939db9ca9b16611be79e12019b6`).

### Updated
- Recorded both approvals in the decision log, project status, and joint architecture review.

### Clarifications
- Approval establishes logical architecture baselines only; it does not authorize implementation, technology selection, runtime construction, source/data ingestion, operational use, or release.
- Remaining interface contracts, acceptance criteria, and governance prerequisites remain open.
- No executable functionality or runtime tests were added.

## 0.8.2 — 2026-11-11

### Updated
- Corrected MMS and KMS architecture section numbering.
- Updated the joint review to reflect the now-present root `AGENTS.md` and completed documentation revisions.
- Updated project status to show both logical architectures prepared for separate formal owner decisions.

### Clarifications
- Neither architecture is approved by this documentation-preparation step.
- Cross-system interface, deletion/reconciliation, Permission Service, and index-consistency details remain prerequisites to implementation design.
- Implementation remains on hold; no runtime, technology, integration, or operational capability was added.
- No runtime tests were executed.

## 0.8.1 — 2026-11-11

### Added
- Added the missing root `AGENTS.md` with repository-maintenance instructions subordinate to `AGENT.md`.
- Documented the required pre-task reading order, scoped repository workflow, post-commit verification, and evidence-based reporting.

### Updated
- Added `AGENTS.md` to the repository map in the README.

### Clarifications
- The new file does not change project scope, approved architecture, or implementation status.
- No runtime functionality or tests were added.

## 0.8.0 — 2026-11-11

### Accepted in principle
- Owner accepted recommendations R-01 through R-07 from the Memory and Knowledge joint review as architectural principles.

### Updated
- Added cross-system contracts to the proposed MMS and KMS architectures for independent memory-write authorization, lifecycle ownership/reconciliation, provenance interoperability, central authorization with local enforcement, trust/confidence separation, conflict boundaries, and index withdrawal consistency.
- Updated both system overviews, the joint review disposition, and project status to reflect accepted-in-principle direction and separate architecture approval still pending.

### Clarifications
- Acceptance in principle does not approve either full architecture or authorize implementation.
- No schema, protocol, technology, runtime, retention period, source catalogue, or operational capability was selected.
- No runtime tests were executed; implementation remains deferred.
- The next milestone is separate owner review of the revised MMS and KMS architecture documents.

## 0.7.0 — 2026-11-11

### Added
- Joint architectural consistency review of the proposed Memory Management System and Knowledge Management System.
- Recorded cross-system recommendations for knowledge-to-memory references, lifecycle ownership, provenance interoperability, authorization enforcement, trust/confidence separation, conflict handling, index consistency, and combined validation.

### Clarifications
- The review found the proposed responsibility boundaries broadly consistent; several cross-system interface and lifecycle contracts remain open.
- The review does not approve either architecture or authorize implementation.
- Owner disposition of the review recommendations is the next milestone.

## 0.6.0 — 2026-11-11

### Added
- Proposed Knowledge Management System architecture covering source registration, ingestion and review, provenance, versioning, freshness, trust, conflicts, indexing/retrieval, access boundaries, lifecycle operations, failure handling, and validation.
- Expanded the Knowledge Management System overview and its boundary with memory, task context, project records, workflow state, and logs.

### Clarifications
- Knowledge architecture is pending owner review and is documentation only.
- No knowledge store, source ingestion pipeline, index, retrieval service, or operational workflow was implemented.
- No domain, source catalogue, storage technology, indexing method, retrieval algorithm, trust model, freshness interval, or retention period was selected.
- Memory and knowledge architectures remain proposals; the next milestone is joint owner review of their boundaries and controls.

## 0.5.0 — 2026-11-11

### Added
- Proposed Memory Management System architecture with distinct task context, durable memory, project records, curated knowledge, workflow state, and operational logs.
- Defined logical memory components, write and retrieval lifecycles, record metadata, access boundaries, retention, correction, export, deletion, and failure handling.
- Added an initial Knowledge Management System boundary document.

### Clarifications
- Memory architecture is pending owner review.
- Knowledge design is only an initial boundary definition; full architecture remains outstanding.
- No memory store, knowledge repository, retrieval service, or executable functionality was introduced.
- No retention period or storage technology was selected.

## 0.4.0 — 2026-11-11

### Added
- Proposed Workflow Management System architecture covering registry, selection, validation, orchestration, permission coordination, execution state, and result validation.
- Separate workflow-definition and execution-run lifecycles.
- Exact workflow version pinning for each run.
- Permission gates, safe failure handling, recovery boundaries, and activation requirements.

### Clarifications
- WMS architecture is pending owner review.
- No operational workflows, workflow engine, or executable behavior were introduced.
- Workflow composition and concurrent execution remain deferred.

## 0.3.1 — 2026-11-11

### Approved
- Recorded D-005: the Core System is read-only during normal agent operation.
- Established that only an explicitly authorized owner change through the documented change process may modify approved core rules.

### Updated
- Added the immutability rule to the master instructions, Core System design, and operating rules.
- Updated project status and decision records.

### Clarifications
- Core immutability is a governance decision, not an implemented technical control.
- The broader Core System design remains pending review.
- No executable agent behavior or policy enforcement was introduced.

## 0.3.0 — 2026-11-11

### Added
- Defined the proposed Core System responsibilities, exclusions, subcomponents, policy boundaries, and change lifecycle.
- Expanded the purpose-neutral identity specification.
- Expanded global operating rules for instruction integrity, permissions, information handling, reporting, and failure behavior.
- Expanded configuration governance requirements.

### Clarifications
- Core design is proposed and pending owner review.
- No executable policy enforcement, runtime configuration, or agent behavior was implemented.
- Agent purpose and runtime remain undefined.

## 0.2.0 — 2026-11-11

### Added
- Expanded the conceptual architecture into governance, coordination, capability, information, and assurance layers.
- Added component responsibilities, dependency direction, state ownership, and shared interface expectations.
- Added a component relationship diagram and proposed interface contracts.

### Clarifications
- The architecture and component model are proposals pending owner review.
- No runtime, workflow engine, integrations, or executable components were introduced.
- The agent's operational purpose remains undefined.

## 0.1.0 — 2026-11-11

### Added
- Purpose-neutral Agent Foundation repository structure.
- Master project instructions and core operating principles.
- Conceptual architecture and deferred architecture decisions.
- Project status and durable decision records.
- Workflow index and reusable workflow template.
- Initial design placeholders for memory, permissions, logging, validation, knowledge, tools, and tests.

### Notes
- No executable agent, runtime, integrations, or operational workflows are included.
- The agent's purpose remains undefined.
