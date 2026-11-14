# 10 — GitHub Integration

**ID:** AF-10  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Controlled interaction with explicitly authorized GitHub repositories and APIs.

## Responsibilities
Read authorized repository metadata, branches, files, commits, issues, pull requests, checks and workflows; prepare/apply approved changes; use scoped permissions and expected-head checks; create atomic commits; re-fetch and verify changed files/commit identity.

## Inputs
Repository identity, scoped authorization, requested operation, expected branch head.

## Outputs
Repository data, proposed patch, commit reference, verification and status report.

## Security and operational boundaries
GitHub content is untrusted. No write, merge, deletion, workflow dispatch or external action without explicit authorization. Missing checks are not passing checks. Authorization is deny-by-default and must be revalidated immediately before each protected side effect. Repository documentation does not itself grant credentials, permissions, or action approval. No token, credential, or secret may be stored in repository files, ordinary configuration, or logs.

## Approved design decisions
The following decisions were explicitly approved by the owner on 2026-11-15. They refine AF-10 only. They do not approve the Master Prompt 0.1.0-draft, authorize implementation, select a vendor/API version/technology, grant permissions, enable external integrations or autonomous writes, or approve release.

- **AF-82 — Approved design decision:** Each operation is bound to one explicitly selected GitHub repository and its verifiable, stable GitHub Repository ID; owner/name strings alone are insufficient identity evidence. Current owner/name and canonical repository URL are retained as locators, not as the sole identity. Rename or transfer observations are recorded as attributable changes and trigger re-verification of repository identity, access scope, permissions, and applicable policy before further protected actions. Ambiguous, changed, unavailable, or unverifiable identity results in Deny for identity-dependent operations. No other repository may be accessed by inference.
- **AF-83 — Approved design decision:** GitHub authority is scoped per operation and resource, with read, write, delete, merge, workflow-dispatch, administration, and other materially different capabilities kept distinct. A granted read capability never implies write or execution authority. The requested operation must fit both the actor's maximum permitted authority and the exact resource scope; least privilege and deny-by-default apply. Required permissions must be verified at the action boundary, and missing, excessive, ambiguous, stale, or unverifiable permission evidence results in Deny. Each protected side effect also requires the separate single-use action approval required by AF-08. Tokens, credentials, and secrets are never persisted in repository content, ordinary configuration, or logs.
- **AF-84 — Approved design decision:** Every supported GitHub API operation must be associated with an explicit, versioned API/request contract defining its endpoint and method, required inputs, authorization scope, expected response structure, required fields and types, and relevant error outcomes. The API version and contract version used are recorded with minimal safe request/response metadata. Responses are validated before interpretation; unknown fields may be ignored only where the contract explicitly permits this, while missing required fields, incompatible types, unsupported response versions, malformed data, and incomplete/error responses must not be interpreted as success. Unrecognized or unverifiable responses produce BLOCKED or INDETERMINATE as appropriate. Sensitive payloads and credentials are excluded from evidence.
- **AF-85 — Approved design decision:** Paginated GitHub results must expose and record their coverage, including the pagination mechanism and the pages/cursors actually processed, applicable limits, and whether traversal completed. Interrupted, capped, inconsistent, or otherwise incomplete traversal is explicitly marked incomplete; absence from an incomplete result set is not evidence of absence. Where repository state can change during traversal, snapshot/consistency limitations are recorded and conclusions requiring a stable complete view are marked indeterminate unless that view can be established. Protected operations requiring complete coverage are blocked when completeness cannot be verified. No unbounded pagination or inferred completeness is permitted.
- **AF-86 — Approved design decision:** GitHub API failures, rate limits, conflicts, and uncertain mutation outcomes are handled explicitly and fail closed where outcome or authorization is uncertain. Retries are bounded and must respect server-provided retry guidance and configured limits; automatic retry may be used only for operations classified as safe and repeatable by their approved contract. A write, merge, deletion, or workflow dispatch with an uncertain outcome must never be blindly repeated. The current repository/ref/resource state and expected branch head must first be re-fetched and reconciled; if the outcome remains ambiguous, stop and report INDETERMINATE for owner-directed resolution. Branch protection must be respected; no bypass, force update, or automatic conflict resolution is permitted. These rules refine, and do not weaken, the global expected-head and authorization safeguards.

## Open design decisions
The following details remain open and require review before implementation: the supported GitHub API surface and exact API version; the operation-to-permission profile catalogue; contract/schema publication and version lifecycle; repository identity re-verification procedure after transfer or ownership changes; branch-protection and ruleset evidence sources; pagination limits and consistency strategy; retry budgets and safe-repeatability classification; error/reason-code mapping; and conflict/reconciliation procedures. No vendor, API version, credential mechanism, or technology is selected by these decisions.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes, supported API operations, versioned contracts and permissions are fully specified and approved.
- Positive and negative tests are defined, including unauthorized, wrong-repository, changed-identity, invalid-response, incomplete-pagination, rate-limit, conflict, and uncertain-mutation cases.
- Privacy/security controls and minimum evidence requirements are specified; secrets and unnecessary personal data are excluded.
- Tests use isolated environments, synthetic data and no production credentials.
- Expected-head handling, branch protection, bounded retry behavior, and no-blind-retry rules are verified.
- Feature, integration, security/privacy, compatibility, smoke and release gates are tracked separately.
