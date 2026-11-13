# 02 — Master Prompt Protection and Integrity Validation

**ID:** AF-02  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 1

## Purpose
Ensure protected operations use the exact owner-approved policy.

## Responsibilities
Load approved policy; verify against independent trust anchor; bind verification to policy identity/version; detect tampering, substitution, downgrade, rollback, missing trust material; separate runtime verification from owner signing/update.

## Inputs
Policy bytes, signed manifest, trusted public key, anti-rollback state.

## Outputs
Verification result with policy/version, digest coverage, trust anchor and failure reason.

## Security and operational boundaries
Fail closed. If integrity verification fails, is indeterminate, or required trust material is unavailable, stop all protected operations and report the verification failure. Do not fall back to an older, alternate, cached, or otherwise unverified policy. Recovery requires explicit owner-directed manual restoration and re-verification against the independently controlled trust anchor before protected operations resume. Never store private keys or recovery material in repo/config. No integrity claim before implementation/testing.

## Approved design decision — AF-02
Integrity verification failure or uncertainty blocks all protected actions (Fail Closed). The system must report the failure and wait for owner-directed manual recovery and independent re-verification. It must not automatically trust an older or alternate Master Prompt. This approval applies only to this design decision; it does not approve the Master Prompt draft or authorize implementation.

## Approved design direction — AF-10
The integrity design uses SHA-256 for policy-content digesting, Ed25519 for digital signatures, a detached JWS using the EdDSA algorithm, and a JCS-canonicalized manifest. Runtime verification uses an independently controlled, read-only trust store and rollback-resistant state. Owner-directed manual recovery remains mandatory; runtime components cannot sign, replace trust anchors, or weaken anti-rollback state. Hardware-backed, non-exportable owner key custody is preferred. This records the high-level direction and does not select an exact JWS serialization/profile, manifest schema, key custody product, signing ceremony, or recovery procedure. It does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Define the exact manifest schema and covered byte representation; strict JWS profile and protected headers; canonicalization and digest coverage rules; signing ceremony and separation of duties; key generation, hardware custody, rotation, revocation, backup and recovery procedures; trust-store provisioning and integrity; anti-rollback state storage, update atomicity and recovery; and independent verification/test evidence. Confirm interoperability and downgrade-resistance requirements before implementation.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
