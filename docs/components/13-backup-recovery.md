# 13 — Backup and Recovery

**ID:** AF-13  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 3

## Purpose
Controlled recovery of approved project data and state.

## Responsibilities
Define backup scope, cadence, authorization, version/integrity metadata; retain recovery points and provenance; verify integrity and restoration in isolation; record attempts and outcomes.

## Inputs
Approved backup plan, source data, recovery target, integrity references.

## Outputs
Backup inventory, restore report, recovery evidence and residual risk.

## Security and operational boundaries
Backup/restore requires authorization and must not touch production. Backup is not valid until integrity and recoverability are tested. Never overwrite only known-good copy.

## Open design decisions
Format, encryption/key custody, retention, off-site strategy, restore objectives, immutable storage.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
