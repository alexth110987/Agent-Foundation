# 18 — Module Manager

**ID:** AF-18  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 3

## Purpose
Manage lifecycle and compatibility of internal modules within the 18-component scope.

## Responsibilities
Inventory module versions, interfaces, dependencies, permissions and lifecycle; validate manifests/compatibility before activation; support controlled enable/disable/upgrade/rollback/retirement proposals; record provenance and verification.

## Inputs
Module manifests, versions, dependency graph, policy and approvals.

## Outputs
Module inventory, compatibility decision, lifecycle proposal and provenance record.

## Security and operational boundaries
Modules cannot self-authorize, expand scope or override Master Prompt. No untrusted activation. Changes require approval and isolated validation.

## Open design decisions
Interface contract, manifest schema, signing integration, compatibility, rollback semantics.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
