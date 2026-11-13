# Agent Foundation

A purpose-neutral foundation for designing and developing a modular AI agent and its workflows.

> **Current state:** Stage 1 — foundation only. The operational purpose, target environment, runtime, and capabilities have not been selected.

## Official project environment

- **GitHub repository (`main`):** the sole official project-management environment and canonical source of truth for instructions, decisions, status, documentation, and durable evidence.
- **ChatGPT:** an assisting interface for discussion, research, drafting, review, and authorized repository work. It is not a second project database or an independent authority.
- **Working conversation:** prefer one central ChatGPT conversation for continuity where practical. Chat context is limited; if a handover becomes necessary, GitHub records remain authoritative and the next conversation must re-read the required live-main instructions.
- GitHub and ChatGPT do not automatically synchronize. Only approved information recorded in the repository becomes an official project record.

## Governing instructions

| File | Authority and responsibility |
| --- | --- |
| [AGENT.md](AGENT.md) | Highest-authority project instructions: identity, scope, safeguards, change control, and development sequence. |
| [AGENTS.md](AGENTS.md) | Subordinate repository-maintenance instructions: mandatory read order, Git workflow, verification, and reporting. It cannot override AGENT.md. |

These are two instruction documents, not two agents. Their separate names and responsibilities are intentional; retain both.

## Foundation principles

1. Keep the architecture modular and purpose-neutral until a purpose is explicitly selected.
2. Separate core rules, workflows, memory, knowledge, permissions, tools, validation, logging, and error handling.
3. Apply least privilege; tools and permissions start disabled or unconfigured.
4. Require explicit authorization for consequential, external, destructive, or irreversible actions.
5. Define validation criteria before claiming readiness.
6. Preserve decisions, changes, limitations, and evidence.
7. Distinguish proposed, approved, implemented, and verified work.

## Repository map

| Path | Responsibility / current state |
| --- | --- |
| `core/` | Neutral identity, operating rules, and configuration principles. |
| `workflows/` | Workflow index, shared template, and Proposed Workflow Management design; no operational workflows. |
| `systems/memory/` | Approved logical Memory Management architecture (D-006); documentation only. |
| `systems/knowledge/` | Approved logical Knowledge Management architecture (D-007); documentation only. |
| `systems/permissions/` | Approved logical Permission, Identity, and Authorization baselines (D-008, D-010, D-011); no enforcement implementation. |
| `systems/validation/` | Navigation and summary for the approved Validation Strategy (D-014); no validation execution capability. |
| `systems/logging/` | Proposed Logging architecture; no runtime implementation. |
| `tools/` | Tool registry and Proposed governance; registry remains empty. |
| `tests/` | Test organization and future evidence location; no test suite or test evidence. |
| `knowledge/` | Reserved knowledge-area entry point; no operational knowledge corpus. |
| `docs/` | Architecture, contracts, decisions, reviews, governance, status, plans, and changelog. |

For a file-by-file disposition and cleanup policy, see [Repository Content Inventory](docs/REPOSITORY_CONTENT_INVENTORY.md). For the GitHub workspace direction, see [GitHub-Native Project Environment](docs/GITHUB_NATIVE_ENVIRONMENT.md).

## Current milestone

**Stage 1 — Core foundation.** The future purpose and runtime remain undefined. Approved logical baselines and owner-approved documentation review dispositions are preserved. Proposed architecture and the GitHub-native environment design remain proposals unless explicitly listed as approved in the decision/status records.

The GitHub-native environment proposal covers information organization, change traceability, pre-upload privacy and secret screening, GitHub Actions defense-in-depth, UTC event timestamps, and independent recovery. **None of those controls is implemented by the proposal.** GitHub Actions alone cannot prevent content reaching GitHub before a workflow runs.

No operational agent, runtime, integrations, privacy scanner, upload gate, timestamp service, automated Actions checks, repository rulesets, or backup system is implemented or authorized by the current Stage 1 scope.

## Status and decisions

- [Project Status](docs/PROJECT_STATUS.md) — authoritative current state, approvals, holds, and open questions.
- [Decision Register](docs/DECISIONS.md) — owner decisions and approval scope.
- [Changelog](docs/CHANGELOG.md) — chronological repository changes.
- [Development Guide](docs/DEVELOPMENT_GUIDE.md) — repository work and review process.
- [Foundation Completion Plan](docs/FOUNDATION_COMPLETION_PLAN.md) — staged foundation review plan.

## Change discipline

Read `AGENT.md`, `AGENTS.md`, and `docs/PROJECT_STATUS.md` on live `main` before repository work, in that order. Inspect relevant files and exact approved baselines. Keep changes atomic, preserve evidence, re-fetch and verify committed files, and report missing checks honestly.

Do not add secrets or unnecessary personal data. Do not treat a clean scan as proof that no personal data exists. No privacy control may be described as preventive until an enforced pre-upload gate covers every authorized write path.
