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
