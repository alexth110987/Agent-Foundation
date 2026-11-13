# 06 — File and Version Tracking

**ID:** AF-06  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Track repository file identity, versions, verified changes, and historical deletion records.

## Responsibilities
Identify files by repository and repository-relative path; calculate versioned digests; maintain lineage and change events; distinguish created/modified/verified/recorded/deleted events; link changes to tasks, approvals, commits, and evidence.

A deletion creates a durable historical tombstone rather than silently removing the file's history. At minimum, the tombstone records the file identity and last-known path, the deletion event timestamp in canonical UTC, and a reference to the actor and action that caused the deletion. Include the last-known version or digest when available. A tombstone is metadata about the deletion; it does not require retaining the deleted file contents.

Tombstones remain available as historical records. Automatic expiry or silent removal is not permitted by this design decision. Any future retention, redaction, or purge mechanism requires a separately reviewed and approved policy that accounts for privacy, legal, security, and audit requirements.

## Inputs
Repository snapshots, file contents, commit metadata, authorization, deletion and rename events.

## Outputs
Inventory, version lineage, change record, versioned digest, deletion tombstone, verification evidence, and rename relationship.

## Security and operational boundaries
SHA-256 is the initial supported content-digest algorithm. Store the algorithm identifier with each digest so future algorithm migration is explicit and verifiable; do not silently reinterpret old digests. A digest is only as trustworthy as its reference. A colocated hash is not an independent trust anchor.

Repository-relative paths use a canonical forward-slash separator and preserve case. Reject paths that escape the repository root or are ambiguous under the repository's path rules; do not silently merge distinct paths through case folding or lossy Unicode normalization. A rename is recorded as a relationship between the prior and new path/identity and is not automatically classified as deletion plus unrelated creation when continuity can be established.

For binary files, track digest and metadata without duplicating file contents solely for tracking. Do not infer device creation time from Git. Tombstones must not copy deleted file contents or unnecessarily retain personal data. Actor references should use stable internal identifiers and avoid unnecessary personal information. Only authorized actions may create or alter tracking records; historical records must not be silently rewritten.

## Approved design decision
SHA-256 is the initial supported content-digest algorithm, with the algorithm identifier stored alongside each digest. Repository-relative paths use forward-slash separators and preserve case; paths escaping the repository root or ambiguous under repository path rules are rejected rather than silently normalized into another identity. Renames are represented as lineage relationships when continuity can be established. Binary files are tracked by digest and metadata without unnecessary content duplication. Actor references use stable internal identifiers and minimize personal data. This approval supplements AF-06's durable tombstone decision; it does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Exact path normalization and Unicode equivalence rules across supported Git platforms, digest migration and independent integrity-anchor design, rename-continuity criteria, tombstone schema, actor-reference lifecycle, access controls, and any exceptional redaction or purge procedure.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Tombstones capture what was deleted, when it was deleted, and which authorized action caused the deletion.
- Digest algorithm identifiers are retained; path traversal and ambiguous identity cases are rejected.
- Rename lineage and binary-file handling are defined and tested.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
