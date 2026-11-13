# 07 — Privacy Filter

**ID:** AF-07  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Prevent secrets and unnecessary sensitive data entering or leaving protected workflows.

## Responsibilities
Inspect files, metadata, filenames, logs, diagnostics and outbound payloads before protected transfer or publication; detect sensitive material; safely redact or block; rescan sanitized output; minimize collection and retention.

By default, detected secrets and credentials are blocked. Detection coverage includes credentials, API tokens, private keys, passwords, session tokens, and recognizable personal data. Use pattern-based and contextual detection together where applicable; neither method nor any single scanner is assumed complete. Detection rules are versioned so scan evidence can identify the rule set used.

Redaction is permitted only through an explicitly authorized sanitization action scoped to the exact content, destination, and purpose. Redaction must not be treated as permission to disclose the original secret. If redaction cannot be validated or sensitive material may remain, block the operation.

## Inputs
Candidate content, metadata, classification rules and their version, destination, scanner status, and narrowly scoped redaction authorization where applicable.

## Outputs
Sanitized content or block decision, findings, rule-set identifier, and minimum safe scan evidence.

## Security and operational boundaries
Detect → Sanitize/Redact or Block → Validate. Fail closed if sensitive data may remain, detection is materially uncertain, or the required scanner is unavailable or fails. Clean scan is risk reduction, not proof. Post-commit scan is not pre-upload protection. Never log or reproduce secret values in findings or diagnostics; retain only minimum safe evidence. Rescan sanitized output before transfer or publication.

## Approved design decision
Detected secrets and credentials are blocked by default. Redaction requires explicit, narrowly scoped authorization and successful post-redaction validation. Detection covers credentials, API tokens, private keys, passwords, session tokens, and recognizable personal data, using pattern and contextual methods where applicable. Uncertain detections and scanner failures block the protected operation. Detection rules are versioned; findings and diagnostics do not expose secret values and retain only minimum safe evidence. This approval supplements AF-07's original default-block/redaction decision; it does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Detailed classification taxonomy and jurisdictional requirements, pattern/rule source governance, false-positive review process, scanner diversity and coverage, redaction format, rule update approval, evidence retention, and exception handling.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Secrets are blocked by default; redaction requires explicit, narrowly scoped authorization and successful post-redaction validation.
- Scanner unavailability, failure, and materially uncertain detection fail closed.
- Rule-set version is included in safe scan evidence.
- Positive and negative tests are defined, including unauthorized, missed-pattern, false-positive, and scanner-failure cases.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
