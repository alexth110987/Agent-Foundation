# 09 — Validation Engine

**ID:** AF-09  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Validate definitions, inputs, state transitions, outputs, and evidence.

## Responsibilities
Validate schemas, configuration, inputs, preconditions, outputs, acceptance criteria; coordinate unit, component, integration, security, privacy, compatibility and smoke tests where authorized; preserve exact versions/environments.

## Inputs
Specifications, artifacts, test definitions, environment, expected results.

## Outputs
Gate report PASS/FAIL/BLOCKED/INDETERMINATE with evidence.

## Security and operational boundaries
Design review, feature, integration, security/privacy, compatibility, smoke and release gates remain separate. Smoke PASS is version/environment-specific, not release approval.

## Open design decisions
Test schema, isolation, fixtures, evidence retention, gate dependencies, coverage.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
