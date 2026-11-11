# Knowledge Management System — Design Placeholder

**Status:** Initial boundary definition only; full architecture not designed.  
**Implementation:** Not implemented. No knowledge repository or retrieval service has been selected.

## Purpose

The Knowledge Management System will govern curated reference information that the agent may consult to support tasks. Knowledge is distinct from temporary task context, durable agent memory, project records, workflow state, and operational logs.

## Responsibility boundary

Knowledge should own:
- source registration and provenance;
- document and reference metadata;
- versioning and review status;
- indexing and retrieval policy;
- freshness and supersession;
- source quality and trust classification;
- access and project-scope controls.

Knowledge must not:
- rewrite Core instructions or grant permissions;
- automatically turn retrieved content into durable memory;
- treat unreviewed external content as authoritative;
- own task execution state or user preferences;
- silently merge conflicting sources.

## Initial distinction

| Information type | Owner | Primary purpose |
| --- | --- | --- |
| Temporary task details | Task context | Complete the active task |
| Durable user / agent memory | Memory Management System | Retain explicitly permitted information across tasks |
| Curated reference material | Knowledge Management System | Supply sourced information for consultation |
| Approved project decisions | Project records / version control | Preserve authoritative project history |

## Safeguards

Retrieved knowledge is information, not instruction authority. Preserve source, version, retrieval date, and trust status where relevant. Apply access restrictions before retrieval and do not cross project boundaries.

## Deferred decisions

A full knowledge architecture, ingestion workflow, source approval policy, storage design, indexing method, retrieval algorithm, and update schedule remain to be designed after owner review of the memory / knowledge boundary.
