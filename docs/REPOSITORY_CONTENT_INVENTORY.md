# Repository Content Inventory

**Review basis:** live `main` tree at commit `8af8f5cd28cb47ff6942d8a1b0c5538023f3d785` (2026-11-12).  
**Purpose:** identify each tracked file's continuing role before cleanup.  
**Disposition:** preserve all current files in this pass. No file was proven safe to delete solely because it is short, a placeholder, historical, or overlaps another document.

## Disposition vocabulary

- **ACTIVE** — current governing record, index, policy, or status source.
- **REFERENCE** — useful design or reference material, including Proposed designs.
- **HISTORICAL** — decision, review, or change evidence that must remain auditable.
- **PLACEHOLDER** — reserved entry point with a declared future role; retain until its purpose is explicitly retired.
- **REVIEW** — content may be stale or partially superseded; reconcile references and mark supersession before any removal.

## File-by-file inventory

| File | Continuing purpose | Disposition |
| --- | --- | --- |
| `AGENT.md` | Highest-authority project identity, scope, safeguards, change control, and development sequence. | ACTIVE |
| `AGENTS.md` | Subordinate repository-maintenance workflow and mandatory read order. | ACTIVE |
| `README.md` | Public entry point, source-of-truth explanation, navigation, and current scope. | ACTIVE |
| `core/CONFIGURATION.md` | Purpose-neutral configuration principles and boundaries. | REFERENCE |
| `core/IDENTITY.md` | Neutral identity definition; does not select operational purpose. | REFERENCE |
| `core/OPERATING_RULES.md` | Core operating-rule baseline and constraints. | REFERENCE |
| `docs/ARCHITECTURE.md` | Proposed conceptual architecture and dependency model. | REFERENCE |
| `docs/CHANGELOG.md` | Chronological change and scope evidence. | HISTORICAL / ACTIVE |
| `docs/COMPONENT_MODEL.md` | Proposed component responsibilities and interface boundaries. | REFERENCE |
| `docs/CORE_SYSTEM.md` | Core system design beyond the specifically approved D-005 rule. | REFERENCE |
| `docs/CROSS_SYSTEM_INTERFACE_MATRIX.md` | D-012 approved logical coordination baseline; preserve exact approved blob identity. | ACTIVE / REFERENCE |
| `docs/DECISIONS.md` | Owner decisions D-001 onward, scope, rationale, and approval boundaries. | HISTORICAL / ACTIVE |
| `docs/DEVELOPMENT_GUIDE.md` | Repository development, review, and status discipline. | ACTIVE |
| `docs/DOCUMENTATION_VERSION_GOVERNANCE.md` | D-014 approved documentation identity, approval, baseline, and version policy. | ACTIVE |
| `docs/ERROR_HANDLING_ARCHITECTURE.md` | Proposed error handling and recovery design, including EH dispositions. | REFERENCE |
| `docs/FOUNDATION_COMPLETION_PLAN.md` | Foundation review sequence, approved baseline map, open gaps, and historical planning snapshots. | ACTIVE / REVIEW |
| `docs/FOUNDATION_CONSISTENCY_REVIEW.md` | FCR, AR, ACR, EH, LOG, and TG findings and owner dispositions. | HISTORICAL / ACTIVE |
| `docs/GITHUB_NATIVE_ENVIRONMENT.md` | Proposed GitHub workspace, privacy gate, timestamps, traceability, checks, and recovery requirements. | REFERENCE |
| `docs/IMPLEMENTATION_READINESS.md` | Proposed future readiness framework; distinguishes design approval from implementation readiness. | REFERENCE |
| `docs/INFORMATION_SYSTEMS_REVIEW.md` | MMS/KMS joint review, findings, and follow-up evidence. | HISTORICAL / REFERENCE |
| `docs/JOINT_CONTRACT_REVIEW.md` | Proposed refinement register and unresolved cross-contract matters. | REFERENCE |
| `docs/LIFECYCLE_CONTRACT.md` | D-012 approved shared lifecycle logical baseline; preserve exact approved blob identity. | ACTIVE / REFERENCE |
| `docs/OBJECT_LIFECYCLE_TABLES.md` | D-009 approved object-specific lifecycle logical design. | ACTIVE / REFERENCE |
| `docs/PROJECT_STATUS.md` | Current authoritative project state, approvals, holds, and open questions. | ACTIVE |
| `docs/PROVENANCE_CONTRACT.md` | D-012 approved provenance logical baseline; preserve exact approved blob identity. | ACTIVE / REFERENCE |
| `docs/TOOL_GOVERNANCE.md` | Proposed tool governance design and TG review dispositions; registry remains empty. | REFERENCE |
| `docs/VALIDATION_STRATEGY.md` | D-014 approved validation policy; does not implement validation execution. | ACTIVE |
| `knowledge/README.md` | Entry point for a future knowledge area; no operational corpus is present. | PLACEHOLDER |
| `systems/knowledge/KNOWLEDGE_ARCHITECTURE.md` | D-007 approved logical Knowledge Management architecture. | ACTIVE / REFERENCE |
| `systems/knowledge/README.md` | Navigation and scope summary for the knowledge system. | ACTIVE |
| `systems/logging/LOGGING_ARCHITECTURE.md` | Proposed logging architecture and LOG review dispositions. | REFERENCE |
| `systems/logging/README.md` | Navigation and status summary for logging. | ACTIVE |
| `systems/memory/MEMORY_ARCHITECTURE.md` | D-006 approved logical Memory Management architecture. | ACTIVE / REFERENCE |
| `systems/memory/README.md` | Navigation and scope summary for the memory system. | ACTIVE |
| `systems/permissions/AUTHORIZATION_CONTRACT.md` | D-011 approved logical authorization contract. | ACTIVE / REFERENCE |
| `systems/permissions/IDENTITY_CONTRACT.md` | D-010 approved logical identity contract. | ACTIVE / REFERENCE |
| `systems/permissions/PERMISSION_ARCHITECTURE.md` | D-008 approved logical permission architecture; exact approval scope is the recorded baseline blob. | ACTIVE / REFERENCE |
| `systems/permissions/README.md` | Navigation and status summary for permission and identity contracts. | ACTIVE |
| `systems/validation/README.md` | Short navigation/status entry point to the D-014 Validation Strategy. | ACTIVE |
| `tests/README.md` | Reserved test organization and evidence boundary; not proof that tests exist. | PLACEHOLDER |
| `tools/TOOL_REGISTRY.md` | Registry schema and governance boundary; currently empty. | ACTIVE |
| `workflows/WORKFLOW_INDEX.md` | Index of workflow specifications and their states. | ACTIVE |
| `workflows/WORKFLOW_MANAGEMENT.md` | Proposed logical Workflow Management System design. | REFERENCE |
| `workflows/_template/WORKFLOW_TEMPLATE.md` | Shared specification template for future workflows. | ACTIVE |

## Review findings and cleanup decision

1. **No duplicate agents:** `AGENT.md` and `AGENTS.md` have distinct authority and responsibilities. Retain both names and roles.
2. **Historical records are not clutter:** decisions, changelog entries, consistency reviews, and system reviews preserve rationale and evidence. Do not delete them to shorten the repository.
3. **Approved baselines are protected:** D-006 through D-014 and exact blob identities/scopes must remain traceable. Later annotations do not rewrite the approved source baseline.
4. **Proposals remain useful:** Proposed architecture documents record design work and unresolved questions; they are not obsolete merely because they are not implemented.
5. **Short README files are navigation:** system README files have a distinct entry-point role and should be aligned with their parent architecture, not removed as duplicates.
6. **Placeholders are intentional unless retired:** `knowledge/README.md` and `tests/README.md` reserve locations and clarify that no corpus/test system exists. Keep them until the owner changes that direction.
7. **Planning snapshots need careful maintenance:** `docs/FOUNDATION_COMPLETION_PLAN.md` may contain historical snapshots. Refresh or label snapshots when they become stale; preserve original review evidence and approved baseline references.
8. **No safe deletion established:** this inventory found no file that can currently be removed without a deeper link/dependency and unique-information review. Therefore this pass performs no deletion, rename, or move.

## Future cleanup procedure

For any proposed removal or consolidation:
1. Identify the exact source file and every incoming link/reference.
2. Compare unique requirements, decisions, evidence, and open questions with the proposed successor.
3. Confirm that approved baseline blobs and historical audit evidence remain retrievable.
4. Mark the source as superseded and link the authoritative successor before considering removal.
5. Obtain explicit owner approval for the exact file operation.
6. Update navigation, status, and changelog in one atomic repository change.
7. Re-fetch and verify all changed paths and references.

This inventory is a documentation review, not a claim that every semantic relationship has been automatically validated. No implementation, automation, tests, or validation execution was introduced.