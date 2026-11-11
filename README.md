# Agent Foundation

A purpose-neutral foundation for designing and developing a modular AI agent and its workflows.

> **Current state:** Foundation only. The agent's operational purpose, target environment, runtime, and capabilities have not yet been selected.

## Where work happens

- **ChatGPT Project — Agent Foundation:** planning, discussion, drafting, and reviewing.
- **GitHub — this repository:** canonical source for approved files, decisions, and project status.
- **Default branch:** `main`.

ChatGPT conversations are a workspace, not an alternative source of truth. A decision becomes authoritative only when it is recorded in this repository. Do not assume that this repository is automatically synchronized with ChatGPT.

## Foundation principles

1. Keep the architecture modular and purpose-neutral until a purpose is explicitly chosen.
2. Separate core rules, workflows, memory, knowledge, tools, permissions, validation, and logs.
3. Apply least privilege: tools and permissions start disabled or unconfigured.
4. Require explicit authorization for consequential, external, destructive, or irreversible actions.
5. Define validation and success criteria before a workflow is considered ready.
6. Record decisions, changes, limitations, and evidence.
7. Distinguish proposed, approved, implemented, and verified work.

## Repository map

| Path | Responsibility |
| --- | --- |
| `AGENT.md` | Highest-authority project and agent-development instructions |
| `AGENTS.md` | Repository-maintenance conventions subordinate to `AGENT.md` |
| `core/` | Neutral identity, operating rules, and configuration principles |
| `workflows/` | Workflow index, reusable workflow template, and proposed Workflow Management System |
| `systems/memory/` | Approved logical Memory Management System architecture; not implemented |
| `systems/knowledge/` | Approved logical Knowledge Management System architecture; not implemented |
| `systems/permissions/` | Approved permission architecture; proposed identity and authorization contracts; documentation only, not implemented |
| `systems/validation/` | Validation principles and future evidence requirements |
| `systems/logging/` | Logging principles and future audit requirements |
| `tools/` | Tool registry and integration controls; no tools approved or connected |
| `tests/` | Testing policy and future test organization |
| `docs/` | Conceptual architecture, component model, cross-system interface/ownership matrix, provenance and lifecycle contracts, readiness framework, decisions, status, and development process |

## Current milestone

**Stage 1 — Core foundation.** Establish governance, architecture, and reusable workflow standards. No agent runtime, integrations, automated actions, or domain-specific workflows are implemented in this stage.

See [Project Status](docs/PROJECT_STATUS.md), [Architecture](docs/ARCHITECTURE.md), [Component Model](docs/COMPONENT_MODEL.md), [Provenance Contract](docs/PROVENANCE_CONTRACT.md), [Shared Lifecycle Contract](docs/LIFECYCLE_CONTRACT.md), [Cross-System Interface & Ownership Matrix](docs/CROSS_SYSTEM_INTERFACE_MATRIX.md), [Permission Architecture](systems/permissions/PERMISSION_ARCHITECTURE.md), [Identity Contract](systems/permissions/IDENTITY_CONTRACT.md), [Authorization Contract](systems/permissions/AUTHORIZATION_CONTRACT.md), and [Workflow Index](workflows/WORKFLOW_INDEX.md).
