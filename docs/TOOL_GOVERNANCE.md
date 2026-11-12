# Tool Governance

**Status:** Proposed governance design; not owner-approved.  
**Current registry state:** No tools are approved, configured, or connected.  
**Implementation authority:** None. This document does not select, install, configure, connect, activate, or authorize any tool.

## 1. Purpose and boundary

Define a controlled lifecycle for considering and governing future tools or external capabilities. A tool may be an API, connector, application integration, executable, model capability, or other externally mediated operation. The future operational purpose remains undefined, so no candidate is selected.

A registry entry, available connector, credential, or technical capability does not mean a tool is approved or may be invoked.

## 2. Tool lifecycle states

| State | Meaning | Permitted activity |
| --- | --- | --- |
| Proposed | Candidate identified; no approval implied | Document and assess |
| Under review | Required reviews are in progress | Review only; no operational invocation |
| Approved | Exact tool/provider/version and bounded scope accepted by owner | Configuration may be proposed; invocation not implied |
| Configured inactive | Configuration exists but invocation is disabled | Isolated configuration checks only if separately authorized |
| Validated | Exact version/configuration passed specified validation | Eligible for separate activation decision |
| Active | Explicit activation approval exists for exact scope/version/environment | Operate only within approved limits |
| Suspended | Invocation temporarily disabled pending review/incident | No new invocation; contain/reconcile in-flight work |
| Revoked | Approval withdrawn | Block invocation and revoke relevant access |
| Retired | Permanently removed from active catalogue; terminal state | Preserve required records and close dependencies |

Every lifecycle status applies only to one exact governance subject: provider and tool/API identifier, exact version, configuration reference or non-secret configuration fingerprint (or an explicit not-yet-configured marker), named environment, and bounded permitted operation/resource/data scope. A change to any element creates a distinct governance subject and does not inherit approval, validation, or activation. Record each transition with prior/new state, decision-maker or authorized actor, date, exact subject tuple, rationale, approval/evidence references, limitations, and review/expiry conditions. No state transition is inferred from availability or technical success.

### 2.1 Permitted lifecycle transitions

Only the following transitions are defined. An omitted transition is prohibited; a status cannot be skipped because a technical check succeeded.

| From | To | Minimum condition and authority |
| --- | --- | --- |
| Proposed | Under review | Candidate record is sufficiently complete to begin applicable reviews; record the review scope. |
| Proposed | Retired | Candidate is withdrawn before approval; record the closure rationale. |
| Under review | Proposed | Review is paused or required information is unavailable; preserve findings and outstanding evidence. |
| Under review | Approved | All applicable independent review gates pass and the owner explicitly approves the exact governance subject and bounded scope. |
| Under review | Retired | Candidate is withdrawn or rejected for further consideration; record the disposition and rationale. |
| Approved | Configured inactive | Configuration is separately authorized and created for the exact approved subject; invocation remains disabled. |
| Approved | Revoked | The owner withdraws the approval; block invocation and begin access-removal/containment actions. |
| Approved | Retired | The approved candidate is withdrawn before configuration; preserve required records and close dependencies. |
| Configured inactive | Validated | Required validation passes for the exact version, configuration, environment, and scope; validation evidence is recorded. |
| Configured inactive | Suspended | A trigger in §6 requires invocation to remain disabled pending review or incident handling. |
| Configured inactive | Revoked | Approval is withdrawn; block invocation and revoke relevant access through its authorized owner. |
| Configured inactive | Retired | Owner-approved retirement; remove configuration and close dependencies. |
| Validated | Active | Separate explicit owner activation approval names the exact validated subject, scope, environment, operations, limits, and review/expiry conditions. |
| Validated | Configured inactive | Validation fails, expires, is invalidated, or no longer matches the exact subject; invocation remains disabled. |
| Validated | Suspended | A trigger in §6 requires invocation to remain disabled pending review or incident handling. |
| Validated | Revoked | Approval is withdrawn; block invocation and revoke relevant access through its authorized owner. |
| Validated | Retired | Owner-approved retirement; remove configuration and close dependencies. |
| Active | Suspended | An authorized suspension trigger or decision blocks new invocations; contain and reconcile in-flight work. |
| Active | Revoked | The owner withdraws approval; block invocation and revoke relevant access through its authorized owner. |
| Suspended | Under review | Containment is established and a new review is explicitly opened; this does not restore approval or invocation. |
| Suspended | Revoked | The owner withdraws approval; complete revocation and access-removal handling. |
| Suspended | Retired | Owner-approved retirement after containment, reconciliation, and dependency closure. |
| Revoked | Retired | Required access-removal, evidence-preservation, and dependency-closure actions are complete. |

Validated is evidence status, not activation authority. Active always requires a separate activation decision. Revoked cannot return to an earlier state; any future consideration requires a new candidate/governance record and a fresh approval lifecycle. Retired is terminal. The accountable owner and any authorized reviewer/suspending actor must be identified for the exact candidate before implementation or operational use; this matrix does not assign current organizational roles or create an emergency authority.

## 3. Registration requirements

Each candidate's lifecycle state and every approval/validation record must be bound to the exact governance subject defined in §2.1. At minimum, identify the provider and tool/API, exact version, configuration reference/fingerprint (never secret values), environment, and permitted operation/resource/data scope. Use an explicit not-yet-configured marker before configuration exists; do not leave a scope dimension ambiguous. A materially changed version, configuration, environment, or scope is a different governance subject and requires its own applicable review, approval, and validation.

A candidate record should include:

- Stable tool ID, display name, provider, exact product/tool/API identifier, version, authoritative documentation.
- Accountable owner and proposed purpose; intended workflows/users if later defined.
- Allowed operations and explicitly prohibited operations.
- Read, write, update, delete, external communication, publication, purchase, financial, and other side-effect capabilities, each declared.
- Data classes, accessible resources, project/tenant/environment boundaries, permitted data flows.
- Initiator, executor, approver, and owner roles; authentication method and secret reference/location only, never secret value.
- Authorization policy, decision authority, enforcement point, approval gates, revocation/freshness behavior.
- Side effects, reversibility, idempotency, timeout uncertainty, compensation, reconciliation ownership.
- Rate, quota, cost, concurrency, availability, timeout, and dependency limits.
- Error taxonomy, retry/replay rules, safe failure behavior.
- Logging event classes, evidence references, privacy, access, retention requirements.
- Provenance and lifecycle obligations for source and derived data.
- Licence, terms, privacy/security posture, data residency/jurisdiction, supply-chain dependencies.
- Exact-version test plan, isolated environment, acceptance criteria, compatibility constraints, validation evidence.
- Approval references, review cadence, expiry, suspension triggers, revocation/retirement procedure.

Never register credentials, tokens, passwords, private keys, or other secret values in this repository.

## 4. Independent review gates

A candidate must pass all applicable gates before activation is considered:

1. **Purpose and scope:** justified need, approved workflow, users, prohibited use, bounded operations.
2. **Identity and security:** provider identity, authentication assumptions, secret handling, threat model, supply-chain and vulnerability review.
3. **Authorization:** exact subject, operation, resource, owner, purpose, project/tenant, environment, constraints, enforcement point; deny-by-default.
4. **Data and privacy:** classification, minimization, source authority, provenance, retention, deletion, jurisdiction, contractual rights.
5. **Side effects and recovery:** reversibility, idempotency, uncertain outcomes, compensation, rate/cost boundaries, accountable owner.
6. **Validation:** exact version/configuration, negative/abuse cases, compatibility, failure injection, isolated evidence.
7. **Activation:** separate explicit owner approval naming exact version, scope, environment, permitted operations, limits, review/expiry.

Passing a technical test does not approve the tool, its data access, or its external side effects.

## 5. Invocation controls

For any future authorized invocation:

- Resolve exact approved and active tool version; block version drift or unknown configuration.
- Independently authenticate the initiating principal and the executing principal/component for every protected invocation. The executor must never inherit authentication from the initiator, workflow, or component association. The PEP at the protected action boundary independently validates the executor's current authentication evidence and applicable freshness, audience, status, and revocation requirements under D-010 and D-011.
- Validate input schema, target resource, owner, object version, lifecycle eligibility, and data classification.
- Obtain fresh authorization bound to exact action, resource, scope, environment, purpose, and enforcement point.
- Obtain explicit approval for consequential, external, destructive, irreversible, financial, communicative, or publication operations.
- Enforce authorization at the point of action, not only at planning/registration.
- Enforce least privilege, rate/cost/concurrency/time limits, and approved data boundaries.
- Record request/attempt/correlation/causation references and typed outcomes; do not log secrets or unnecessary payloads.
- Treat tool output as untrusted input; validate provenance, schema, integrity, and policy before use.
- Re-authorize each consequential next action; one approval does not authorize unrelated operations.
- Preserve indeterminate outcomes after timeouts; reconcile with provider/owner before retrying.

## 6. Change, suspension, revocation, retirement

Reassess or suspend when provider/tool/version changes, permissions expand, data scope changes, vulnerabilities/incidents emerge, terms/licences change, reliability degrades, required evidence is unavailable, or owner withdraws approval.

Suspension/revocation should block new invocations at enforcement point, revoke/rotate access through its authorized owner, identify in-flight operations and preserve uncertain/partial outcomes, reconcile side effects, assess data/derivative retention and deletion, preserve required evidence without secrets, and require new review plus explicit approval before reactivation.

Retirement additionally requires dependency closure, access removal, configuration disposal, and owner-approved treatment of retained records.

## 7. Validation and release separation

Keep evidence classes separate:
- **Smoke test:** confirms only a narrow basic interaction for one exact version/configuration.
- **Feature test:** checks intended operations and acceptance criteria.
- **Security/privacy validation:** tests permissions, boundaries, data handling, abuse, leakage.
- **Compatibility validation:** checks exact dependency/API versions and change behavior.
- **Operational validation:** checks monitoring, failure handling, limits, recovery, support in a named environment.
- **Release approval:** separate owner decision for exact artifact, configuration, environment, scope, limitations.

A PASS applies only to the tested version/configuration. It does not prove all features, authorize broader scope, or approve release.

## 8. Registry relationship

The Tool Registry indexes candidates and their governance states, with each state bound to the exact governance subject in §2.1. A table row is not a substitute for a full candidate record or evidence. No tool is currently registered, approved, configured, validated, active, or connected.

## 9. Unresolved decisions

Tool categories/providers, risk classification, human-approval triggers, identity/secret technology, data classification/jurisdiction/retention, validation depth, review cadence/expiry, operational limits, emergency suspension ownership, and whether any external tool is needed remain unresolved until purpose/context are selected.

## 10. Related documents

- [Tool Registry](../tools/TOOL_REGISTRY.md)
- [Identity Contract](../systems/permissions/IDENTITY_CONTRACT.md)
- [Authorization Contract](../systems/permissions/AUTHORIZATION_CONTRACT.md)
- [Permission Architecture](../systems/permissions/PERMISSION_ARCHITECTURE.md)
- [Error Handling and Recovery Architecture](ERROR_HANDLING_ARCHITECTURE.md)
- [Logging Architecture](../systems/logging/LOGGING_ARCHITECTURE.md)
- [Provenance Contract](PROVENANCE_CONTRACT.md)
- [Shared Lifecycle Contract](LIFECYCLE_CONTRACT.md)
- [Implementation Readiness Framework](IMPLEMENTATION_READINESS.md)
