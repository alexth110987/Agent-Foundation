# Workflow Index

## Purpose
This index is the catalogue of workflow specifications in this repository. A workflow document does not mean that it is executable, tested, active, or approved for operational use.

## Management architecture
The proposed Workflow Management System is documented in [WORKFLOW_MANAGEMENT.md](WORKFLOW_MANAGEMENT.md). It defines registry, selection, validation, orchestration, permission coordination, execution state, and result validation responsibilities. Its architecture is pending owner review and is not implemented.

## Definition lifecycle

Definition status describes the workflow specification and its authorization for use:

1. **Proposed** — need and intended outcome are being considered.
2. **Draft** — specification is being written.
3. **Under review** — checked for clarity, permissions, safety, and testability.
4. **Approved** — owner accepts the specification.
5. **Implemented** — workflow exists in its intended execution environment.
6. **Verified** — it passed defined tests and evidence is recorded for the exact version and environment.
7. **Active** — separately authorized for selection in a named environment.
8. **Deprecated / Retired** — not eligible for new execution.

Approval, implementation, verification, and activation are separate gates. A workflow document's presence in this index does not make it executable.

## Execution lifecycle

Execution status describes one individual run, not the workflow definition:

- Queued
- Validating
- Ready
- Running
- Waiting for approval
- Paused
- Blocked
- Succeeded
- Failed
- Cancelled

A run must pin an exact workflow version. Terminal runs are not silently restarted; a new attempt receives a new run ID and retains a relationship to the previous attempt where appropriate.

## Current workflows

No operational workflows have been defined or approved.

| ID | Name | Definition status | Specification |
| --- | --- | --- | --- |
| — | No workflows yet | — | — |

## Adding a workflow

1. Copy `_template/WORKFLOW_TEMPLATE.md`.
2. Assign a unique, stable ID and version.
3. Define scope, trigger, inputs, preconditions, permissions, steps, outputs, validation, and failure handling.
4. Identify side effects, approval gates, dependencies, and safe recovery behavior.
5. Submit the specification for review.
6. Record owner approval, implementation evidence, verification, and activation as separate events.
7. Add the workflow and its accurate definition status to this index.
8. Never mark a workflow Active without explicit activation authorization for the intended environment.
