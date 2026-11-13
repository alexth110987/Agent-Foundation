# 06 — File and Version Tracking

**ID:** AF-06  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Track repository file identity, versions, and verified changes.

## Responsibilities
Identify files by repository and normalized path; calculate digests; maintain lineage, change events, deletion tombstones; distinguish created/modified/verified/recorded events; link changes to tasks, approvals, commits, evidence.

## Inputs
Repository snapshots, file contents, commit metadata, authorization.

## Outputs
Inventory, version lineage, change record, digest, verification evidence.

## Security and operational boundaries
Digest is only as trustworthy as its reference. A colocated hash is not an independent trust anchor. Do not infer device creation time from Git.

## Open design decisions
Digest algorithm, path normalization, rename/binary handling, tombstone retention.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
