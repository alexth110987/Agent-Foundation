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

### Tombstone record structure

Each confirmed deletion creates a distinct historical tombstone record. The following logical fields define the approved minimum record structure; they do not prescribe a storage technology or serialization format:

| Field | Requirement |
|---|---|
| `tombstone_id` | Unique, non-semantic identifier for this tombstone. |
| `repository_id` | Stable identifier of the repository in which deletion occurred. |
| `file_identity` | Exact file identity as established by the authoritative repository source; never reconstructed from a lossy display path. |
| `last_known_path` | Exact last-known repository-relative path representation, subject to AF-12. |
| `deleted_at` | Deletion event timestamp in canonical UTC, subject to AF-05; preserve source precision and clock-quality metadata. |
| `actor_reference` | Stable internal actor identifier, with no unnecessary personal data. |
| `action_reference` | Verifiable reference to the specific authorized deletion action and its approval/evidence. |
| `last_known_version` | Last-known version reference, when available; otherwise explicitly unknown/not available. |
| `digest_algorithm` | Explicit algorithm identifier for the associated digest, when a digest exists. |
| `last_known_digest` | Last-known content digest, when available; never fabricate or silently recompute a historical value. |
| `integrity_reference` | Reference to independently protected integrity evidence for the tombstone/record history; the exact anchor mechanism remains open under AF-13. |
| `record_version` | Version of the logical tombstone schema, so later schema evolution is explicit. |

A field that cannot be established must be represented as explicitly unknown or unavailable according to the eventual schema; it must not be guessed, silently omitted where that would imply certainty, or populated with fabricated data. The tombstone stores metadata and references only; it does not copy the deleted file contents.

Tombstones are append-only historical records. Corrections, supplemental evidence, reassessments, or schema-migration observations are added as separately attributable events/records that reference the original tombstone. They must not overwrite the original values or erase prior assessments. Automatic expiry and silent deletion remain prohibited. Any exceptional redaction or purge requires a separately reviewed and approved policy addressing privacy, legal, security, and audit obligations.

Creation of a tombstone is permitted only as part of a specifically authorized deletion action. Its actor and action references must be verifiable. If file identity, authorization, or the integrity of the tombstone record cannot be established, the affected protected deletion/recording operation is denied or remains unverified as appropriate; no false success state may be reported. Independent anchor location/protection, exact integrity construction, access-control details, actor-reference lifecycle, and exceptional redaction/purge procedure remain open design items.

### Rename-continuity assessment
A rename may be linked to an existing file lineage only when evidence supports continuity; a matching name, directory, or file size alone is insufficient. An identical cryptographic digest is strong evidence of content equality, but does not by itself prove that two paths represent the same file identity.

The assessment must:
- preserve the prior and new exact path identities as separate path observations and record the proposed lineage relationship between them;
- use authoritative repository history or event metadata where available, and record the specific evidence supporting the relationship;
- treat an unchanged digest as corroborating content evidence, not conclusive identity evidence;
- require additional reliable evidence when content changes as part of the rename; absent such evidence, do not automatically link the lineage;
- reject automatic linkage when multiple plausible source or destination candidates make the relationship ambiguous;
- record the actor or authoritative repository event responsible for the rename observation and retain a verifiable evidence reference without unnecessary personal data;
- mark the relationship unverified when evidence is insufficient, ambiguous, unavailable, or cannot be validated, and fail closed for protected operations that depend on that identity relationship.

An unverified relationship must not be silently promoted to verified or used to rewrite historical identity. Any later reassessment must retain the original assessment and add a separately attributable evidence-backed event.

## Approved design decisions
- **AF-06:** SHA-256 is the initial supported content-digest algorithm, with the algorithm identifier stored alongside each digest. Repository-relative paths use forward-slash separators and preserve case; paths escaping the repository root or ambiguous under repository path rules are rejected rather than silently normalized into another identity. Renames are represented as lineage relationships when continuity can be established. Binary files are tracked by digest and metadata without unnecessary content duplication. Actor references use stable internal identifiers and minimize personal data. This approval supplements AF-06's durable tombstone decision.
- **AF-12:** File identity is based on the exact path representation from the authoritative repository source, without case folding, Unicode normalization, or lossy rewriting. Preserve original path bytes when exposed by the source; where only decoded text is available, retain the exact source string and encoding context. If lossless identity or round-trip equivalence cannot be established, treat the path as ambiguous and block identity-sensitive changes. Non-UTF-8 paths, where supported, remain opaque identities with safe reversible display encoding. A platform unable to distinguish or faithfully represent paths must block potentially colliding writes, renames, and deletions.
- **AF-13:** File integrity requires an independently protected reference or trust anchor; a digest stored only beside the file or in the same mutable trust domain is insufficient. Digest-algorithm migration must be explicit and versioned, preserve historical algorithm identifiers and digests, and establish a verifiable link between old and new digests for the same exact file version. Unverifiable anchors or migration linkage result in an unverified integrity state and block integrity-dependent protected operations.
- **AF-14:** A rename is linked to an existing file lineage only when evidence supports continuity. Names, directory locations, and file sizes alone are insufficient. An identical cryptographic digest is strong evidence of content equality but not conclusive proof of identity. Changed-content renames require additional reliable evidence; ambiguous multiple-candidate matches are not automatically linked. The prior and new exact path identities remain separately recorded. Evidence, actor or authoritative repository event, and assessment outcome are retained. Insufficient, ambiguous, unavailable, or unverifiable evidence results in an unverified relationship; protected operations depending on that identity relationship fail closed. Reassessment adds a new attributable event and does not erase the prior assessment.

These approvals do not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Define the independent anchor's authoritative location and protection model, verification protocol, and recovery process. Specify digest-migration evidence format and transition procedure. Actor-reference lifecycle, access controls, and any exceptional redaction or purge procedure remain open. AF-15 defines the approved logical tombstone fields but does not select storage, serialization, or integrity-anchor implementation. Platform-specific path capability detection and the exact reversible display encoding also remain to be specified and tested. Rename evidence-source precedence, confidence/acceptance thresholds, and the precise event/evidence schema remain open for detailed design; AF-14 establishes the fail-closed decision criteria but does not select an implementation.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Tombstones use the approved AF-15 logical field set, explicitly mark unavailable information, retain no deleted content, and preserve original records through append-only corrections or supplemental events.
- Digest algorithm identifiers and historical digests are retained; path traversal and ambiguous identity cases are rejected.
- Independent integrity anchors and digest-migration linkage are verifiable; missing or invalid evidence fails closed for integrity-dependent actions.
- Path identity preserves source representation; cross-platform collisions and non-round-trippable paths are safely blocked.
- Rename continuity is evidence-backed; name/location/size-only matches are rejected; changed-content and multiple-candidate cases fail closed absent sufficient evidence; prior assessments remain auditable.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
