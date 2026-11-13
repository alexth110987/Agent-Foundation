# 07 — Privacy Filter

**ID:** AF-07  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Prevent secrets and unnecessary sensitive data entering or leaving protected workflows.

## Responsibilities
Inspect files, metadata, filenames, logs, diagnostics and outbound payloads before protected transfer or publication; detect sensitive material; safely redact or block; rescan sanitized output; minimize collection and retention.

By default, detected secrets and credentials are blocked. Redaction is permitted only through an explicitly authorized sanitization action scoped to the exact content, destination, and purpose. Redaction must not be treated as permission to disclose the original secret. If redaction cannot be validated or sensitive material may remain, block the operation.

## Inputs
Candidate content, metadata, classification rules, destination, and narrowly scoped redaction authorization where applicable.

## Outputs
Sanitized content or block decision, findings and scan evidence.

## Security and operational boundaries
Detect → Sanitize/Redact or Block → Validate. Fail closed if sensitive data may remain. Clean scan is risk reduction, not proof. Post-commit scan is not pre-upload protection. Never log or reproduce the secret value in findings or diagnostics; record only the minimum safe evidence needed.

## Open design decisions
Detection rules, false positives, data classes, redaction techniques, authorization representation, scanner coverage, evidence minimization, and retention.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Secrets are blocked by default; redaction requires explicit, narrowly scoped authorization and successful post-redaction validation.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
