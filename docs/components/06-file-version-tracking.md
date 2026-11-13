# 06 — File and Version Tracking

**ID:** AF-06  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Track repository file identity, versions, verified changes, and historical deletion records.

## Responsibilities
Identify files by repository and normalized path; calculate digests; maintain lineage and change events; distinguish created/modified/verified/recorded/deleted events; link changes to tasks, approvals, commits, and evidence.

A deletion creates a durable historical tombstone rather than silently removing the file's history. At minimum, the tombstone records the file identity and last-known path, the deletion event timestamp in canonical UTC, and a reference to the actor and action that caused the deletion. Include the last-known version or digest when available. A tombstone is metadata about the deletion; it does not require retaining the deleted file contents.

Tombstones remain available as historical records. Automatic expiry or silent removal is not permitted by this design decision. Any future retention, redaction, or purge mechanism requires a separately reviewed and approved policy that accounts for privacy, legal, security, and audit requirements.

## Inputs
Repository snapshots, file contents, commit metadata, authorization, deletion events.

## Outputs
Inventory, version lineage, change record, digest, deletion tombstone, verification evidence.

## Security and operational boundaries
Digest is only as trustworthy as its reference. A colocated hash is not an independent trust anchor. Do not infer device creation time from Git. Tombstones must not copy deleted file contents or unnecessarily retain personal data. Only authorized actions may create or alter tracking records; historical records must not be silently rewritten.

## Open design decisions
Digest algorithm, path normalization, rename/binary handling, tombstone schema and actor-reference format, storage integrity, access controls, and any exceptional redaction or purge procedure.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Tombstones capture what was deleted, when it was deleted, and which authorized action caused the deletion.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
