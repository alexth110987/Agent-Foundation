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
- **Not approved:** Runtime implementation; any technology/vendor selection; the independent Master Prompt trust-anchor mechanism; identity provider; deployment/hosting; storage/retention choices; or a specific first implementation increment. These remain open decisions requiring explicit owner review.
- **Safeguards:** The approved design does not alter the Master Prompt, expand the 18 agreed scope areas, authorize production access/changes, enable external integrations, or permit autonomous writes. Implementation remains blocked until the outstanding Step 1 completion-gate decisions are approved.
- **Evidence limitation:** This record captures the owner's design approval; it is not a cryptographic signature.

## Decision handling

New decisions require explicit owner approval where they affect scope, permissions, architecture, security, compatibility, holds, or release conditions. Proposals and research are not approvals.
