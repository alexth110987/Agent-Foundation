# Agent Foundation — Project Status

**Status:** Initial documentation bootstrap; Step 1 design review in progress  
**Master Prompt:** 0.1.0-draft — pending owner approval  
**Implementation:** 0%  
**Release:** Not approved

## Scope baseline
Exactly 18 component specification files are established under `docs/components/`. The roadmap has exactly three project steps:
1. Foundation and Security — components 1, 2, 5, 6, 7, 8.
2. Workflow and Execution — components 3, 4, 9, 10, 11, 16.
3. Management, Recovery and Monitoring — components 12, 13, 14, 15, 17, 18.

## Approval state
The owner authorized creation of the initial documentation baseline. This does not approve the exact Master Prompt version/content, authorize implementation, or approve release.

## Recorded design decisions
- **AF-01 — Approved design decision:** The Master Control System may propose additional policy rules, but proposals remain non-operative until the owner explicitly approves them and the approved policy source is updated through the authorized process. The system may not approve or activate its own proposals. This decision does not approve the Master Prompt draft or authorize implementation.
- **AF-02 — Approved design decision:** Integrity verification failure, uncertainty, or unavailable trust material blocks all protected operations (Fail Closed). No automatic fallback to older, alternate, cached, or unverified policy is permitted. Recovery requires owner-directed manual restoration and independent re-verification. This decision does not approve the Master Prompt draft or authorize implementation.
- **AF-05 — Approved design decision:** UTC is the canonical internal storage and interchange representation for system event timestamps. Local time is display-only. This decision does not approve the Master Prompt draft or authorize implementation.
- **AF-06 — Approved design decision:** File deletions create durable historical tombstones recording the file identity and last-known path, deletion timestamp in canonical UTC, and a reference to the actor and action that caused the deletion; the last-known version or digest is included when available. Tombstones do not retain deleted file contents and must not be silently expired or removed. Any future retention, redaction, or purge mechanism requires a separately reviewed and approved policy addressing privacy, legal, security, and audit requirements. This decision does not approve the Master Prompt draft or authorize implementation.
- **AF-07 — Approved design decision:** Detected secrets and credentials are blocked by default. Redaction is allowed only through an explicitly authorized sanitization action scoped to the exact content, destination and purpose, followed by successful validation; if sensitive material may remain or redaction cannot be validated, the operation is blocked. Findings and diagnostics must not expose secret values and must retain only minimum safe evidence. This decision does not approve the Master Prompt draft or authorize implementation.
- **AF-08 — Approved design decision:** Every write, execute, delete and publish action requires its own explicit approval bound to the exact requested operation and relevant context, checked immediately before the side effect. No standing or blanket approval substitutes for per-action approval; tool access or approval for one action grants no unrelated permission. This decision does not approve the Master Prompt draft or authorize implementation.

## Implementation state
All 18 items are specifications only. No executable component, runtime, workflow, integration, scanner, permission enforcement, backup, monitor, queue, or module manager is implemented.

## Holds and safeguards
- No production-world, live-server, production-configuration, or installed-mod changes.
- No real secrets, credentials, tokens, passwords, private keys, or recovery material in the repository.
- No technology stack or vendor selected.
- No external integrations, scheduled execution, autonomous write permissions, or deployment enabled.
- Tests must be isolated and use synthetic data without production credentials.
- No scope expansion beyond the exact 18 components without explicit owner approval.

## Next gate
Step 1 design review for components 06, 07 and 08 is recorded. Review the remaining open design details before implementation planning.
