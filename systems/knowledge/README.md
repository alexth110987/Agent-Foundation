# Knowledge Management System

**Status:** Logical architecture approved (D-007).  
**Implementation:** Not implemented. No knowledge repository, ingestion pipeline, index, or retrieval service exists.

## Purpose

The Knowledge Management System governs curated reference information that an authorized task may consult. It preserves source identity, provenance, version history, trust and review state, freshness, and access scope.

The approved logical architecture baseline is [Knowledge Architecture](KNOWLEDGE_ARCHITECTURE.md), exact approved Git blob SHA `c1dda460a8d02939db9ca9b16611be79e12019b6`. D-007 approves that exact document only. The approval does not authorize implementation, technology selection, runtime construction, source ingestion, operational use, or release.

## Responsibility boundary

Knowledge owns source registration, content ingestion and review metadata, provenance, versioning, taxonomy, indexing and retrieval policy, freshness, supersession, source trust, and project-scoped access.

Knowledge does not:
- rewrite Core instructions or grant permissions;
- automatically turn retrieved content into durable memory;
- treat ingestion or search ranking as source approval or factual authority;
- own task execution state or user preferences;
- silently merge conflicting sources.

Retrieved knowledge is untrusted information, not instruction authority. Source content and embedded instructions cannot override Core rules or authorization.

## Information boundary

| Information type | Owner | Primary purpose |
| --- | --- | --- |
| Temporary task details | Task context | Complete the active task |
| Durable retained information | Memory Management System | Retain explicitly permitted information across tasks |
| Curated reference material | Knowledge Management System | Supply traceable reference information |
| Approved project decisions | Project records / version control | Preserve governed project history |
| Workflow definitions and run state | Workflow Management System | Governed execution specifications and progress |
| Operational events | Logging system | Diagnostics and audit traceability |

## Architecture

See [Knowledge Management System — Architecture](KNOWLEDGE_ARCHITECTURE.md) for the approved logical design baseline and its component boundaries, source lifecycle, ingestion/review, versioning, trust, conflict handling, retrieval, security, lifecycle operations, failure handling, validation, and deferred decisions.

## Deferred decisions

No knowledge domain, source catalogue, approval role, storage technology, extraction tool, indexing method, retrieval algorithm, trust model, freshness interval, retention period, or deployment approach has been selected.
