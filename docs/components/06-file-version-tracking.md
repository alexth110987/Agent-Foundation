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

Repository-relative paths use forward-slash separators and preserve case. Reject paths that escape the repository root or are ambiguous under the repository's path rules; do not silently merge distinct paths through case folding or lossy Unicode normalization. A rename is recorded as a relationship between the prior and new path/identity and is not automatically classified as deletion plus unrelated creation when continuity can be established.

For binary files, track digest and metadata without duplicating file contents solely for tracking. Do not infer device creation time from Git. Tombstones must not copy deleted file contents or unnecessarily retain personal data. Actor references should use stable internal identifiers and avoid unnecessary personal information. Only authorized actions may create or alter tracking records; historical records must not be silently rewritten.

Path identity must be based on the exact path representation supplied by the authoritative repository source, not on a display string. Do not case-fold, Unicode-normalize, or otherwise rewrite the identity. Preserve original path bytes when the source interface exposes them. If an interface exposes only decoded text, retain that exact source string and encoding context; if lossless identity or round-trip equivalence cannot be established, treat the path as ambiguous and block identity-sensitive changes. Non-UTF-8 paths, where supported by the source, remain opaque identities and must be rendered only through a safe reversible encoding. A platform that cannot distinguish or faithfully represent two repository paths must not perform a potentially colliding write, rename, or delete.

Integrity verification must rely on an independently protected reference or trust anchor; a digest stored only beside the file or in the same mutable trust domain is insufficient to establish integrity. The authoritative anchor location, protection model, verification protocol, and recovery process remain subject to separate design approval. Any digest-algorithm migration must be explicit and versioned, retain the algorithm identifier and historical digest, and establish a verifiable link between old and new digests for the same exact file version. Never silently replace, reinterpret, or discard historical digest evidence. If the independent anchor or migration linkage cannot be verified, mark integrity as unverified and block integrity-dependent protected operations.

## Approved design decisions
- **AF-06:** SHA-256 is the initial supported content-digest algorithm, with the algorithm identifier stored alongside each digest. Repository-relative paths use forward-slash separators and preserve case; paths escaping the repository root or ambiguous under repository path rules are rejected rather than silently normalized into another identity. Renames are represented as lineage relationships when continuity can be established. Binary files are tracked by digest and metadata without unnecessary content duplication. Actor references use stable internal identifiers and minimize personal data. This approval supplements AF-06's durable tombstone decision.
- **AF-12:** File identity is based on the exact path representation from the authoritative repository source, without case folding, Unicode normalization, or lossy rewriting. Preserve original path bytes when exposed by the source; where only decoded text is available, retain the exact source string and encoding context. If lossless identity or round-trip equivalence cannot be established, treat the path as ambiguous and block identity-sensitive changes. Non-UTF-8 paths, where supported, remain opaque identities with safe reversible display encoding. A platform unable to distinguish or faithfully represent paths must block potentially colliding writes, renames, and deletions.
- **AF-13:** File integrity requires an independently protected reference or trust anchor; a digest stored only beside the file or in the same mutable trust domain is insufficient. Digest-algorithm migration must be explicit and versioned, preserve historical algorithm identifiers and digests, and establish a verifiable link between old and new digests for the same exact file version. Unverifiable anchors or migration linkage result in an unverified integrity state and block integrity-dependent protected operations.

These approvals do not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Define the independent anchor's authoritative location and protection model, verification protocol, and recovery process. Specify digest-migration evidence format and transition procedure. Rename-continuity criteria, tombstone schema, actor-reference lifecycle, access controls, and any exceptional redaction or purge procedure remain open. Platform-specific path capability detection and the exact reversible display encoding also remain to be specified and tested.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Tombstones capture what was deleted, when it was deleted, and which authorized action caused the deletion.
- Digest algorithm identifiers and historical digests are retained; path traversal and ambiguous identity cases are rejected.
- Independent integrity anchors and digest-migration linkage are verifiable; missing or invalid evidence fails closed for integrity-dependent actions.
- Path identity preserves source representation; cross-platform collisions and non-round-trippable paths are safely blocked.
- Rename lineage and binary-file handling are defined and tested.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
