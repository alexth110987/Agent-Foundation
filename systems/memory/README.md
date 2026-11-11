# Memory Management System

**Status:** Cross-system principles accepted in principle; architecture remains proposed pending detailed design and separate owner approval.  
**Implementation:** Not implemented. No memory technology has been selected.

## Purpose

The Memory Management System governs information deliberately retained across tasks. It is separate from temporary task context, project records, workflow execution state, operational logs, and curated knowledge.

See [Memory Architecture](MEMORY_ARCHITECTURE.md) for the proposed component model, record lifecycle, access boundaries, retention, deletion, failure handling, and validation requirements, and the accepted-in-principle cross-system contract with Knowledge.

## Baseline categories

- **Task context:** temporary information used during a task; owned by the task context component.
- **Durable memory:** explicitly permitted information retained across tasks; owned by this system.
- **Project records:** approved project decisions and artifacts; owned by project documentation and version control.
- **Curated knowledge:** sourced reference material; owned by the Knowledge Management System.
- **Workflow state:** run progress and checkpoints; owned by the Workflow Management System.
- **Operational logs:** minimal event records; owned by the logging system.

These categories must remain distinct and have separate ownership and lifecycle rules.

## Baseline safeguards

- Do not retain information merely because it appeared in a conversation.
- Require an approved purpose, applicable authorization, minimal content, provenance, scope, and retention rule before writing.
- Enforce project and subject isolation for both reads and writes.
- Memory cannot modify Core instructions, grant permissions, or create workflow authority.
- Provide controlled inspection, correction, export, expiry, and deletion capabilities.
- Never store credentials or authentication secrets as ordinary memory.
- Do not claim a record was stored, corrected, or deleted until the operation is verified.

## Deferred decisions

No database, vector store, embedding model, retrieval algorithm, memory provider, retention period, or deployment approach is selected.

The design remains a proposal. No operational memory functionality has been introduced.
