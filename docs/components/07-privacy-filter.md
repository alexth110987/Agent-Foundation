# 07 — Privacy Filter

**ID:** AF-07  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Prevent secrets and unnecessary sensitive data entering or leaving protected workflows.

## Responsibilities
Inspect files, metadata, filenames, logs, diagnostics and outbound payloads; detect sensitive material; safely redact or block; rescan sanitized output; minimize collection/retention.

## Inputs
Candidate content, metadata, classification rules, destination.

## Outputs
Sanitized content or block decision, findings and scan evidence.

## Security and operational boundaries
Detect → Sanitize/Redact or Block → Validate. Fail closed if sensitive data may remain. Clean scan is risk reduction, not proof. Post-commit scan is not pre-upload protection.

## Open design decisions
Detection rules, false positives, data classes, redaction, scanner coverage, retention.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
