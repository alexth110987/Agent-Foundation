# Decision Register

## D-001 — Owner-approved Master Prompt baseline

- **Status:** Approved
- **Decision date:** 2026-11-13
- **Decision:** The owner confirmed that the current `docs/MASTER_PROMPT.md` is the approved governing baseline for Agent Foundation.
- **Approved file:** `docs/MASTER_PROMPT.md`
- **Approved Git blob SHA:** `6eae90336a810794a8b58c02c5f5d2797aa30856`
- **Containing commit:** `214ac7e9e164de6bb77fdffade0b541260917ee9`
- **Scope:** This approval confirms the contents of that exact version as the governing project instructions. It does not approve changes to the Master Prompt, operational deployment, production access, or capabilities beyond the agreed scope.
- **Integrity limitation:** This SHA is a reference value, not an independent cryptographic trust anchor merely because it is recorded in this repository. A future runtime must pin the owner-approved version in an independently controlled trust location or verify an owner-controlled signature/protected release reference. Until selected, implemented, and tested, integrity assurance remains a documented control, not an implemented control.
- **Evidence:** Owner confirmation in the project conversation on 2026-11-13; recorded in commit `4ce15123de729e22f2553dbf2743773fcd5b655a`.

## D-002 — Step 1 foundation and security design approval

- **Status:** Approved (design only)
- **Decision date:** 2026-11-13
- **Decision:** The owner approved `docs/STEP_1_FOUNDATION_DESIGN.md` version `0.1.0` as the design basis for Step 1.
- **Approved design content SHA before approval recording:** `9ee6dc3111b20599d40a2860c915d272c8db956d`
- **Approval evidence:** Explicit owner confirmation in the project conversation on 2026-11-13.
- **Approved scope:** The existing logical architecture, shared logical contracts, trust boundaries/data flow, threat model, fail-closed behavior, acceptance criteria, and isolated test strategy in that exact design version.
- **Not approved by D-002:** Runtime implementation; any technology/vendor selection; identity provider; deployment/hosting; storage/retention choices; or a specific first implementation increment. The trust-anchor design direction is separately decided in D-003; its detailed cryptographic and operational choices remain open subject to D-004.
- **Safeguards:** The approved design does not alter the Master Prompt, expand the 18 agreed scope areas, authorize production access/changes, enable external integrations, or permit autonomous writes. Implementation remains blocked until the remaining Step 1 completion-gate decisions are approved.
- **Evidence limitation:** This record captures the owner's design approval; it is not a cryptographic signature.

## D-003 — Independent Master Prompt signature trust-anchor direction

- **Status:** Approved (architecture direction only)
- **Decision date:** 2026-11-13
- **Decision:** Protect the approved Master Prompt using an owner-controlled digital signing key. The corresponding public verification key must be held outside the repository in a separately controlled trust store. The private signing key must never be committed to the repository, stored in ordinary project configuration, or exposed to the runtime's normal policy-read path.
- **Purpose:** At each protected operation, the future runtime must verify the Master Prompt's signature against the independently held public key and the approved policy identity/version. Missing, invalid, mismatched, unavailable, or ambiguous verification must fail closed.
- **Approval evidence:** Explicit owner confirmation of this proposed direction in the project conversation on 2026-11-13.
- **Still open after D-003:** Concrete cryptographic algorithms/formats, digest choices, key lifecycle details, trust-store implementation, signing ceremony, exact signature binding, and runtime integration. D-004 records the subsequently approved design directions without closing the remaining implementation choices.
- **Not authorized:** Creating or handling real signing keys; changing the Master Prompt; selecting a vendor/service; deploying a trust store; implementing runtime verification; production access; or enabling autonomous writes.
- **Integrity status:** This is an approved design direction, not an implemented or tested cryptographic control. The current repository SHA remains a reference only and is not itself an independent trust anchor.
- **Evidence limitation:** This record captures the owner's approval; it is not a cryptographic signature.

## D-004 — Detailed cryptographic trust-anchor design directions

- **Status:** Approved (design directions only; not implementation approval)
- **Decision date:** 2026-11-13
- **Decision:** The owner approved the following directions for the Master Prompt signing and verification design:
  1. Use **Ed25519** as the signing key algorithm.
  2. Use a **detached JWS signature with the EdDSA algorithm identifier** for the signed policy artifact.
  3. Represent the signed manifest using **JSON Canonicalization Scheme (JCS)** canonicalization.
  4. Keep private-key custody owner-controlled; prefer hardware-backed, non-exportable key storage.
  5. Define explicit key lifecycle controls, including rotation and revocation.
  6. Keep the public verification key in an independently controlled trust store; runtime access is read-only.
  7. Maintain rollback-resistant policy and key state so older policy/key state cannot silently replace newer approved state.
- **Approval evidence:** Explicit owner approval of these design directions in the project conversation on 2026-11-13.
- **Relationship to D-003:** D-004 refines the cryptographic design direction. It does not replace the independent trust-anchor, owner-control, read-only runtime, or fail-closed requirements in D-003.
- **Still open:** Manifest schema and required claims; exact JWS protected-header and payload profile; content-digest algorithm and digest coverage; key generation/signing ceremony; concrete hardware/key custody provider and recovery process; trust-store location, access controls and recovery; rotation overlap and revocation propagation; anti-rollback state storage and recovery; policy/repository identity binding details; and runtime integration and verification tests.
- **Not authorized:** Generating, importing, exporting, or handling real keys; selecting a vendor or service; deploying a trust store; implementing cryptographic verification or anti-rollback storage; changing the approved Master Prompt; production access; or enabling autonomous writes.
- **Integrity status:** These are approved design directions only. No signing key, signed manifest, trust store, verifier, or anti-rollback mechanism exists or has been tested. Do not claim cryptographic integrity or runtime enforcement.
- **Evidence limitation:** This record captures the owner's approval; it is not a cryptographic signature.

## Decision handling

New decisions require explicit owner approval where they affect scope, permissions, architecture, security, compatibility, holds, or release conditions. Proposals and research are not approvals.
