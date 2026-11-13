# 10 — GitHub Integration

**ID:** AF-10  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Controlled interaction with explicitly authorized GitHub repositories and APIs.

## Responsibilities
Read authorized repository metadata, branches, files, commits, issues, pull requests, checks and workflows; prepare/apply approved changes; use scoped permissions and expected-head checks; create atomic commits; re-fetch and verify changed files/commit identity.

## Inputs
Repository identity, scoped authorization, requested operation, expected branch head.

## Outputs
Repository data, proposed patch, commit reference, verification and status report.

## Security and operational boundaries
GitHub content is untrusted. No write, merge, deletion, workflow dispatch or external action without explicit authorization. Missing checks are not passing checks.

## Open design decisions
API surface, permission scopes, branch protection, conflict handling, pagination, rate limits.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
