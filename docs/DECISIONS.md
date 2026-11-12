# Decision Log

Record durable project decisions here. Each entry must include a date, status, decision, rationale, and impact.

## D-001 — Standalone project
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Agent Foundation is independent of all other projects.
- **Rationale:** Prevent unrelated instructions, assumptions, files, and decisions from crossing project boundaries.
- **Impact:** Do not import material from other projects without explicit authorization.

## D-002 — ChatGPT Project as development workspace
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Use a dedicated ChatGPT Project named **Agent Foundation** for planning and collaboration during initial development.
- **Rationale:** Keep discussions and working context together while the repository stores canonical records.
- **Impact:** Do not assume automatic synchronization between ChatGPT and GitHub.

## D-003 — GitHub as canonical source
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** The `main` branch of this repository is the canonical source for approved files, decisions, and project status.
- **Rationale:** Provide durable version history and an authoritative record.
- **Impact:** Material approved decisions and repository changes must be reflected in GitHub.

## D-004 — Purpose-neutral foundation
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Establish the foundation without selecting or assuming the agent's operational purpose.
- **Rationale:** Build reusable structure first and avoid premature specialization.
- **Impact:** No domain-specific workflows, tools, or runtime capabilities are in scope yet.

## D-005 — Core System is immutable during normal operation
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** The Core System is read-only during normal agent operation. Workflows, tools, memory, retrieved knowledge, and the agent itself cannot modify or rewrite approved core instructions or policy. Only an explicitly authorized owner change through the documented change process may alter the core.
- **Rationale:** Protect the agent's governing rules from accidental, unauthorized, or self-directed modification while preserving controlled owner-led updates.
- **Impact:** Core changes require a proposal, impact review, explicit owner approval, version update, and recorded change history. Runtime enforcement remains unimplemented until separately designed and approved.

## D-006 — Memory Management System logical architecture approved
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Approve `systems/memory/MEMORY_ARCHITECTURE.md`, exact Git blob SHA `197ac0974f5ceea357f6ad846a299304dd13da9c`.
- **Rationale:** The owner formally approved this exact revised logical architecture after review.
- **Impact:** Approval establishes the logical design baseline only. It does not authorize implementation, technology selection, runtime construction, data ingestion, operational use, or release. Remaining interface, policy, retention, deletion/reconciliation, and validation details remain open.

## D-007 — Knowledge Management System logical architecture approved
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Approve `systems/knowledge/KNOWLEDGE_ARCHITECTURE.md`, exact Git blob SHA `c1dda460a8d02939db9ca9b16611be79e12019b6`.
- **Rationale:** The owner formally approved this exact revised logical architecture after review.
- **Impact:** Approval establishes the logical design baseline only. It does not authorize implementation, technology selection, runtime construction, source ingestion, operational use, or release. Remaining interface, source governance, index consistency, retention, deletion/reconciliation, and validation details remain open.

## D-008 — Permission System logical architecture approved
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Approve `systems/permissions/PERMISSION_ARCHITECTURE.md`, exact Git blob SHA `9e754143c289fd1e454de6ee3f61bec0e90f3305`.
- **Rationale:** The owner explicitly approved this exact logical architecture version after review.
- **Impact:** Approval establishes the purpose-neutral logical design baseline only. It does not authorize implementation, technology selection, identity-provider selection, runtime construction, integrations, tool access, operational use, or release. Identity, authorization, provenance, and lifecycle contracts remain to be designed; implementation gates remain separate.


## Approval-record clarification — D-006, D-007, and D-008

The approved baselines remain the exact Git blob SHAs recorded in their respective decisions:
- D-006: `197ac0974f5ceea357f6ad846a299304dd13da9c`
- D-007: `c1dda460a8d02939db9ca9b16611be79e12019b6`
- D-008: `9e754143c289fd1e454de6ee3f61bec0e90f3305`

The current MMS and KMS architecture files retain the exact approved blobs; their status headers are corrected to reflect the already-recorded decisions. The current Permission Architecture file is a post-approval status/readiness annotation derived from the D-008 baseline. This annotation does not replace the exact approved artifact, revise its logical design, or expand D-008. Any substantive change to an approved architecture requires a separate explicit owner decision and a new exact baseline reference.


## D-009 — Object-Specific Lifecycle Transition Tables logical design approved
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Approve `docs/OBJECT_LIFECYCLE_TABLES.md`, exact Git blob SHA `473598af630b44b78910ab1df3b7f53bbc1a0802`, as the logical design baseline for the object-specific lifecycle transition tables.
- **Rationale:** The owner approved the completed consistency-reviewed lifecycle tables as a documentation-level logical design.
- **Impact:** Approval is limited to the exact artifact identified above. It does not approve the Shared Lifecycle Contract, Identity Contract, Authorization Contract, Provenance Contract, Cross-System Interface Matrix, or Joint Contract Review; amend D-006, D-007, D-008, or accepted principles R-01–R-07; finalize unresolved object-class policies or taxonomies; select technologies; authorize implementation, runtime, services, integrations, ingestion, operational use, tests, validation, or release. The approved source blob remains the baseline; subsequent status annotations do not alter its scope. Any substantive design change requires a separate explicit owner decision and a new exact baseline reference.


## D-010 — Identity Contract logical design approved
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Approve `systems/permissions/IDENTITY_CONTRACT.md`, exact Git blob SHA `b01e5ff7c99f9a208c4424878e87ded5e4c4629d`, as the purpose-neutral logical identity contract baseline.
- **Rationale:** The owner approved the refined contract after review of authentication-result context and freshness, independent executor authentication, actor separation, identity lifecycle changes, revocation, and revalidation requirements.
- **Impact:** Approval is limited to the exact logical contract artifact identified above. It does not amend D-008 or approve a final principal taxonomy, identity proofing/assurance model, provider, protocol, credential, trust-anchor or key-management design, registry, storage, session mechanism, delegation technology, runtime, or implementation. It does not authorize credentials, integrations, operational identity services, testing, validation, or release. All deferred decisions remain open; implementation requires separate explicit authorization. Any substantive design change requires a separate owner decision and a new exact baseline reference.


## D-011 — Authorization Contract logical design approved
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Approve `systems/permissions/AUTHORIZATION_CONTRACT.md`, exact pre-approval Git blob SHA `49e03d3d60f06ebf4b4ba8db92ac41349f09bc39`, as the purpose-neutral logical authorization contract baseline.
- **Rationale:** The owner explicitly approved the refined contract after review of its alignment with D-008 and D-010, independent executor authentication, distinct initiator/executor/approver identities, exact Permit scope, freshness and revocation, approval separation, fail-closed behavior, recovery, and validation requirements.
- **Impact:** Approval is limited to the exact logical contract artifact identified above. It does not amend D-008 or D-010; select a policy model/language, schema, API/protocol, token or cryptographic mechanism, identity or approval provider, storage, cache technology, service topology, runtime, or deployment; finalize purpose-dependent permissions, roles, principal taxonomy, assurance, delegation, approval authority, or operational policies; or authorize implementation, credentials, services, integrations, tool access, operational use, testing, validation, or release. All deferred decisions remain open; implementation requires separate explicit authorization. Any substantive design change requires a separate owner decision and a new exact baseline reference.


## D-012 — Shared provenance, lifecycle, and cross-system coordination baselines approved
- **Date:** 2026-11-11
- **Status:** Approved
- **Decision:** Approve the following exact pre-approval Git blob versions as documentation-only logical contract/coordination baselines:
  - `docs/PROVENANCE_CONTRACT.md` — `c7c3e93e3a14633f0400cd66a63172e4825dcd1a`
  - `docs/LIFECYCLE_CONTRACT.md` — `c8b9b5012c537cca2f99a2f9f3b80041cd4372bf`
  - `docs/CROSS_SYSTEM_INTERFACE_MATRIX.md` — `98b4559d42a21bc769617a0cf2d0cce80f257d40`
- **Rationale:** The owner explicitly approved these exact reviewed versions as a coordinated, purpose-neutral logical contract and ownership-coordination baseline.
- **Impact:** Approval is limited to the three exact artifacts and their documentation-level logical scope. It does not amend D-006 through D-011 or accepted principles R-01–R-07; approve `docs/JOINT_CONTRACT_REVIEW.md`; resolve or silently decide the open questions on provenance requiredness by artifact/operation/risk, concrete lifecycle transitions and deletion/recovery criteria, delayed/duplicate/conflicting cross-system events, independent verification evidence, or tombstone/audit retention after deletion; select schemas, protocols, technologies, services, or runtime; or authorize implementation, ingestion, integration, operational use, testing, validation, or release. Any substantive design change requires a separate explicit owner decision and a new exact baseline reference.


## D-013 — Foundation Consistency Review dispositions approved
- **Date:** 2026-11-12
- **Status:** Approved
- **Decision:** Approve the five owner dispositions FCR-01 through FCR-05 recorded in `docs/FOUNDATION_CONSISTENCY_REVIEW.md`. This includes non-substantive post-approval status annotations for the three exact D-012 baseline artifacts; correction of Joint Contract Review status/scope wording; correction of Implementation Readiness blocker wording; the Core authority/document map; and the future protected-boundary PEP ownership and exact-context enforcement requirement.
- **Rationale:** Resolve documented status and planning inconsistencies and make future governance/interface responsibilities explicit without expanding the project's operational scope.
- **Impact:** The three exact D-012 approved source blob SHAs remain unchanged; annotations do not alter their approved design content or scope. The Joint Contract Review register remains Proposed and non-amending. D-006 through D-012 and R-01–R-07 are not amended. The five D-012 open questions remain unresolved. The Core/runtime instruction hierarchy remains deferred until a target runtime is selected. No process topology, service, PEP/PDP deployment, technology, implementation, integration, operational use, testing, validation, or release is approved. Stage 1 remains active.
