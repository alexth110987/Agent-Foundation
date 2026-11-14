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

## Approved design decisions — AF-48 through AF-52
- **AF-48 — Approved design decision:** The integrity manifest identifies the exact Master Prompt artifact and its verification context. Its required logical fields are: manifest schema version; document identity; Master Prompt version; digest algorithm and digest; signature algorithm; signing key identifier; publication timestamp; and prior approved version/reference where one exists. Required fields must be present, unambiguous, and validated; no missing values are inferred. The manifest is JCS-canonicalized and bound to the detached signature. Exact field names, types, byte-coverage rules, and serialization profile remain to be specified. This does not approve the Master Prompt draft or authorize implementation.
- **AF-49 — Approved design decision:** Signature verification uses the AF-10 direction: Ed25519 with EdDSA in a detached JWS and SHA-256 policy-content digesting, with a JCS-canonicalized manifest. Protected headers and algorithm identifiers are strictly validated; unknown or disallowed algorithms, malformed signatures, unexpected critical headers, invalid canonicalization, and downgrade attempts are rejected. No automatic algorithm substitution or fallback verification method is permitted. The exact interoperable JWS profile and header allowlist remain open. This does not approve the Master Prompt draft or authorize implementation.
- **AF-50 — Approved design decision:** A policy signing ceremony is owner-controlled and binds approval and signature to the exact reviewed artifact/version. The sequence is: prepare candidate; freeze and identify exact contents; obtain explicit owner approval; compute the approved digest; sign the manifest through the separately controlled owner signing process; independently verify the signature and artifact binding; then publish. The system may prepare and verify evidence but may not approve or sign for the owner. Any content change after approval invalidates that approval/signature binding and requires a new review. This does not approve the Master Prompt draft or authorize implementation.
- **AF-51 — Approved design decision:** Signing keys follow a controlled, attributable lifecycle covering generation, registration, activation, rotation, suspension, compromise, revocation, and recovery. Hardware-backed, non-exportable owner key custody remains preferred. Private keys, recovery material, and secrets must never be stored in the repository, ordinary configuration, or logs. Runtime components cannot generate, export, replace, or use owner signing keys to approve/sign policy. Key lifecycle transitions require owner control and evidence; compromise or uncertain key status blocks dependent protected operations. Exact custody mechanism, ceremony, backup/recovery approach, rotation criteria, and separation-of-duties details remain open. This does not approve the Master Prompt draft or authorize implementation or key generation.
- **AF-52 — Approved design decision:** Runtime trust material is independently controlled and read-only to runtime components. Anti-rollback state must prevent silent activation of an older or downgraded policy and must be updated in a controlled, integrity-protected manner. Missing, stale, conflicting, unavailable, or unverifiable trust/rollback state blocks protected operations; no automatic fallback to cached, alternate, or older policy is permitted. Recovery is owner-directed manual restoration followed by independent verification. Trust-store provisioning, anti-rollback state representation, atomicity, durability, recovery evidence, and platform-specific protections remain open. This does not approve the Master Prompt draft or authorize implementation.

## Open design decisions
Specify exact manifest field names/types and byte-coverage rules; strict interoperable JWS profile and protected-header allowlist; canonicalization and digest coverage; signing ceremony evidence and separation of duties; key generation/custody/rotation/revocation/recovery procedures; trust-store provisioning and integrity; anti-rollback state storage, update atomicity, durability and recovery; and independent verification/interoperability test evidence.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
