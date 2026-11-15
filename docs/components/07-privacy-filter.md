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

## Additional approved design decisions

The owner approved the following five refinements on 2026-11-14. These are design decisions only; they do not approve the Master Prompt 0.1.0-draft, authorize implementation, expand source access, or approve release.

- **AF-67 — Approved design decision:** Sensitive-data classification must consider relevant context, data category, processing purpose, and processing environment rather than relying on pattern matches or file type alone. Conflicting or insufficient context is classified as unknown/indeterminate, not safe. Protected transfers depending on a safe classification are blocked until the uncertainty is resolved through an authorized process. This does not approve the Master Prompt draft or authorize implementation.
- **AF-68 — Approved design decision:** Privacy inspection covers relevant embedded and non-obvious metadata exposed by the explicitly selected content, including document properties, image metadata, hidden fields, and comparable metadata-bearing structures where applicable. A scan must state which relevant metadata classes were actually inspected. Uninspected or inaccessible relevant metadata is reported as unknown/incomplete; the content must not be described as fully sanitized, and dependent protected transfers fail closed where sensitive metadata may remain. This does not authorize access beyond the selected content or approve implementation.
- **AF-69 — Approved design decision:** Encoded, compressed, or obfuscated content is not presumed clean merely because ordinary text or pattern scanning finds no match. Inspection is limited to explicitly supported and safely inspectable representations. If relevant content cannot be decoded or inspected safely, record that limitation as unknown/incomplete and block the dependent protected transfer. No unsafe decoding, execution, or automatic interpretation is authorized. This does not approve implementation.
- **AF-70 — Approved design decision:** Nested files and archives are inspected only within explicit, approved nesting-depth, expanded-size, and processing limits. The scan records applicable limits and whether the full relevant content was covered. If a limit, parser failure, or inaccessible member prevents complete inspection, the result is incomplete rather than clean; dependent protected transfers fail closed where uninspected content may contain sensitive material. This does not authorize unbounded extraction or approve implementation.
- **AF-71 — Approved design decision:** A positive privacy-scan result is bound to the exact content/version, destination, purpose, and detection-rule-set version assessed. Any change to one of these dimensions requires a new assessment before protected transfer or publication; a prior positive result must not be reused for a different transfer context. If the exact binding or current applicability cannot be verified, the result is indeterminate and the protected operation is blocked. This does not approve implementation.


## Additional approved design decision — trust-boundary privacy inspection

- **AF-87 — Approved design decision (2026-11-15):** Apply routine privacy inspection at trust-boundary crossings, not to every internal operation. Inspect incoming data when it enters the controlled environment from an external or otherwise untrusted source, before it is admitted to protected internal workflows. Inspect outgoing data before it crosses from the controlled environment to an external, untrusted, or otherwise unapproved destination. Internal processing, editing, copying, renaming, and movement between resources that remain within the same explicitly controlled trust boundary do not, by themselves, trigger a new privacy scan. This exemption is limited to the internal operation: it does not waive the required inspection of a later outbound transfer. Every distinct outbound transfer is assessed for its exact content/version, destination, purpose, and current rule-set under AF-71; changes to those dimensions require a new assessment. A change in trust boundary, destination trust status, processing purpose, or uncertainty about whether an operation is internal or external requires reassessment; ambiguous boundary classification is treated as untrusted and protected transfer is blocked until resolved. This decision does not exempt secret handling, access control, integrity verification, malware/security controls, authorization, or other applicable safeguards; it does not approve implementation or expand the 18-component scope.

## Open design decisions
Detailed classification taxonomy and jurisdictional requirements, pattern/rule source governance, false-positive review process, scanner diversity and coverage, supported metadata classes, safe decoding/parser support, archive nesting/expanded-size/processing limits, redaction format, rule update approval, evidence retention, and exception handling.

## Acceptance criteria before implementation
- Classification considers context, data category, purpose, and processing environment; unresolved classification is not treated as safe.
- Scan evidence identifies relevant metadata classes inspected and reports uninspected metadata honestly.
- Encoded/compressed/obfuscated content and nested archives are either inspected through explicitly supported safe handling or reported incomplete; applicable limits and coverage are recorded.
- Privacy inspection is performed on ingress and egress across trust boundaries; internal operations within the same controlled boundary do not independently trigger routine privacy scans.
- A positive scan is bound to exact content/version, destination, purpose, and rule-set version; any change requires reassessment.
- Interfaces, state transitions, failure modes and permissions are approved.
- Secrets are blocked by default; redaction requires explicit, narrowly scoped authorization and successful post-redaction validation.
- Scanner unavailability, failure, and materially uncertain detection fail closed.
- Rule-set version is included in safe scan evidence.
- Positive and negative tests are defined, including unauthorized, missed-pattern, false-positive, and scanner-failure cases.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
