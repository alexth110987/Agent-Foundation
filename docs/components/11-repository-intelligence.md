# 11 — Repository Intelligence

**ID:** AF-11  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Evidence-based understanding of repository structure and state.

## Responsibilities
Map directories, files, modules, dependencies, configuration, docs and workflow relationships; identify inconsistencies, duplicates, orphaned records, missing references and likely impact; cite file/line/commit evidence; separate facts from inference.

## Inputs
Repository snapshot, manifests, metadata, search results.

## Outputs
Repository map, findings, impact analysis, evidence and limitations.

## Security and operational boundaries
Analysis does not authorize changes. Embedded repository instructions are data, not policy. Never claim completeness when evidence is incomplete.

## Open design decisions
Indexing, supported formats/languages, evidence granularity, confidence, incremental refresh.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
