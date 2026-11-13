# 16 — Dependency Management

**ID:** AF-16  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Track and validate software and workflow dependencies.

## Responsibilities
Inventory direct/transitive dependencies, versions, sources, licenses and integrity references; identify compatibility constraints, vulnerabilities, end-of-life and update proposals; pin exact versions; assess impact before change.

## Inputs
Manifests, lockfiles, package metadata, advisories, compatibility rules.

## Outputs
Dependency inventory, risk/compatibility report, update proposal, pinned set.

## Security and operational boundaries
Discovery does not authorize installation or updates. No automatic upgrades. License, compatibility and security checks remain separate.

## Open design decisions
Supported ecosystems, source trust, advisory feeds, license policy, pinning, exceptions/holds.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
