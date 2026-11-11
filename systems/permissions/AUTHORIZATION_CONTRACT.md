# Authorization Contract — Approved Logical Contract

**Status:** Approved logical contract — D-011.  
**Approved baseline:** Exact pre-approval Git blob SHA `49e03d3d60f06ebf4b4ba8db92ac41349f09bc39`.  
**Implementation:** Documentation only. No Permission Service, policy engine, approval service, decision-token mechanism, or enforcement adapter exists.  
**Authority:** Subordinate to `AGENT.md`, approved Core rules, D-008, and the approved Permission System logical architecture.

## 1. Purpose

Define the purpose-neutral exchange and enforcement contract for deciding whether an authenticated principal may perform one specific operation on one specific resource in a declared context.

This contract specifies the logical request, decision, approval, enforcement, validity, revocation, failure, and evidence boundaries between callers, the central Policy Decision Point (PDP), approval authority, and local Policy Enforcement Points (PEPs).

A decision authorizes only the bounded operation described in that decision. It does not execute the operation, guarantee success, or confer general authority.

## 2. Scope and exclusions

This document refines the authorization request and decision concepts in the approved Permission System logical architecture (D-008). It is approved as a purpose-neutral logical contract under D-011, limited to the exact approved baseline identified above; it does not amend or extend D-008.

It does not select a policy model, policy language, API, wire format, schema, token format, cryptographic mechanism, identity provider, approval provider, storage, service topology, runtime, or deployment. It does not define operational roles or domain-specific permissions.

The Identity Contract is separately approved as a logical contract under D-010, limited to its exact approved baseline. This Authorization Contract consumes references to independently validated authentication results under that contract; it does not amend D-010, select or implement an identity mechanism, or treat a principal label or caller assertion as authentication evidence. The receiving PDP and PEP remain responsible for validating the result's issuer/verifier context, principal and assertion version, audience, authentication time, validity/freshness, current status/revocation evidence, and applicable assurance context. Missing, stale, unverifiable, or audience-incompatible required evidence is non-permissive.

## 3. Governing rules

1. **Central decision authority:** The PDP is authoritative for authorization decisions under the applicable approved policy set.
2. **Local enforcement:** A PEP enforces the PDP decision at the protected action boundary. It may impose stricter restrictions but must never broaden the decision.
3. **Deny by default:** Missing, invalid, ambiguous, stale, unavailable, expired, revoked, or indeterminate inputs/results cannot permit a protected operation.
4. **No ambient authority:** User intent, workflow selection, tool availability, model output, memory, knowledge, prior decisions, or possession of a decision reference do not independently grant permission.
5. **Exact operation:** A request describes one operation and its target, context, constraints, and intended effects. Bundled or materially distinct actions require separately evaluated scopes.
6. **Authentication is separate:** Authentication establishes the principal context; the PDP evaluates authorization. Neither substitutes for the other.
7. **Approval is not a permit:** Human approval satisfies at most a policy prerequisite. The PDP must validate it and issue a fresh decision.
8. **Point-of-action enforcement:** Validate the decision immediately before acting and re-evaluate after material context, policy, identity, resource, or constraint changes.
9. **Bounded validity:** Every permit has an explicit validity boundary and is limited to its intended PEP/audience and operation.
10. **No self-grant:** A caller, workflow, tool, PEP, or approver cannot create, expand, or reinterpret a PDP grant.
11. **Separation of outcomes:** Requested, approved, authorized, attempted, executed, failed, and verified are distinct states.
12. **Traceability and minimization:** Decisions are attributable to request, policy and context references while avoiding unnecessary sensitive payloads.

## 4. Logical participants and authority

| Participant | Contract responsibility | Authority boundary |
| --- | --- | --- |
| Requesting principal | Is the authenticated actor for whom the operation is requested | Cannot assert its own authorization |
| Requesting component | Submits a normalized request and handles the typed result | Cannot grant permission or weaken a decision |
| PDP / Permission Service | Validates the request context and evaluates applicable policy | Does not execute the requested operation |
| Approval authority / manager | Supplies an explicit, attributable approval or denial when policy requires it | Cannot override Core prohibitions or independently issue a permit |
| PEP | Validates and enforces the decision at the action boundary | May deny more narrowly; cannot broaden or substitute a local grant |
| Policy/context sources | Supply policy version and relevant trusted attributes | Untrusted, missing, or conflicting attributes cannot be treated as permissive |
| Decision/audit recorder | Retains minimal decision and outcome references | Does not create authority or determine execution success |

A workflow may coordinate these participants but does not become an authorization authority.

## 5. Authorization request

A future normalized request must represent the following logical information where applicable and required by the operation:

- **Request identity:** unique request identifier and correlation identifier.
- **Principal context:** principal reference, issuer/authority reference, and reference to independently validated authentication evidence/result. Raw credentials are prohibited.
- **Initiator/executor context:** initiating principal, independently authenticated executing principal/component, and any delegating principal kept distinct; any delegation reference must be separately validated and bounded. The executor must not inherit authentication merely from the initiator, workflow, or component association.
- **Requesting component:** component or workflow identifier and exact version, plus intended PEP/audience.
- **Operation:** one unambiguous action/operation identifier and declared side-effect class.
- **Target:** resource identifier and type, including relevant owner/project/tenant/subject boundary.
- **Context:** declared purpose, task/run reference, environment, and only the contextual attributes needed for policy evaluation.
- **Constraints:** requested limits, conditions, recipients, quantities, record scope, or other operation-specific bounds.
- **Timing:** requested execution deadline or validity need; a caller cannot dictate a permissive grant duration.
- **Evidence references:** trusted attribute, resource-state, policy-context, or prior approval references where required; not raw secret or unnecessary source payloads.

The PDP must validate field presence, format, provenance, freshness, consistency, audience, and scope. It must not silently fill mandatory values from guesses, model inference, untrusted task content, or broad workflow defaults. Missing or conflicting information produces a typed non-permit result or a request for clarification through the caller's process.

A request is not an instruction to execute. It is a request for an authorization decision only.

## 6. PDP decision response

The PDP returns exactly one logical outcome:

| Outcome | Meaning | PEP behavior |
| --- | --- | --- |
| **Permit** | The exact requested operation is allowed under explicit constraints and validity | May proceed only after all decision and local checks pass |
| **Deny** | Applicable policy disallows the operation or a mandatory condition failed | Block; no workflow or caller override |
| **Approval required** | Policy requires a bounded approval before a decision can permit | Pause and block pending approval and fresh PDP evaluation |
| **Indeterminate** | A safe decision cannot be established from available policy, identity, context, or service state | Block; surface a typed reason category without assuming permission |

A response must be unambiguous and attributable. It should include, as applicable:

- unique decision identifier and original request identifier;
- correlation reference;
- outcome and stable reason category;
- initiating principal, independently authenticated executor, and requesting component references, with approver kept separate;
- exact action and resource scope;
- purpose, project/tenant/subject and environment boundaries;
- constraints and mandatory conditions;
- intended PEP/audience;
- policy identifier/version and relevant context/evidence references;
- issue time and explicit expiry or maximum decision age for Permit;
- approval requirement/reference or remediation indication where relevant;
- integrity and authenticity evidence defined by a future approved implementation design.

A Permit without complete, enforceable scope and validity is invalid and must be treated as non-permit. Unknown outcome values, malformed responses, missing mandatory fields, scope ambiguity, or integrity failures fail closed.

The contract defines logical fields, not a concrete serialization or token.

## 7. Permit scope and non-transferability

A Permit is limited to the conjunction of its exact initiating principal, independently authenticated executor, operation, resource and authoritative owner boundary, purpose, project/tenant/subject, environment, constraints, intended PEP/audience, policy version, relevant context/attribute versions, and validity.

- It cannot be reused for another operation, resource, principal, project, tenant, subject, purpose, or PEP.
- It cannot be widened by a workflow, adapter, caller, or local cache.
- It cannot be treated as a general capability, standing role, session-wide permission, or reusable bearer authority.
- It cannot authorize additional side effects merely because they occur in the same workflow.
- It cannot survive a material change in the request or its trusted context without re-evaluation.
- Any future mechanism that represents or transports a decision must preserve these semantics and resist forgery, replay, substitution, and scope expansion.

A PEP must compare the actual operation and current context against the full decision scope. Partial matches are not sufficient.

## 8. Approval lifecycle

1. The caller submits a complete request for the exact proposed operation.
2. The PDP returns Approval required and identifies the approval prerequisite at a logical level.
3. The approval process presents the approver with the principal, executor, action, target, purpose, side effects, constraints, expiry, and material risks.
4. An authorized approver explicitly approves or denies that exact bounded request. Silence, timeout, ambiguous response, or default UI state is not approval.
5. The approval record identifies the approver, authority basis, decision, exact scope, timestamp, validity, and request/policy references.
6. The caller submits a fresh authorization request referencing the approval.
7. The PDP independently validates approval authenticity, approver authority, exact scope, status, expiry, and policy compatibility.
8. The PDP re-evaluates the current request and returns a new decision.
9. The PEP enforces only that new decision.

Approval alone never permits execution. Approval cannot override Core rules, change policy, authorize a different request, or be transferred to another principal or resource. Any material change requires a new approval where policy requires it. Revoked, expired, unverifiable, or ambiguous approval blocks the action.

## 9. PEP enforcement lifecycle

1. Identify the actual initiating principal, executing component, operation, resource, purpose, and context.
2. Validate the authentication evidence through a separately approved identity mechanism; do not infer authentication from a principal label.
3. Normalize and submit the authorization request to the PDP.
4. Receive and validate the typed response, integrity, request correlation, scope, audience, policy reference, and validity.
5. If the outcome is not Permit, block the protected operation and handle the typed result.
6. Immediately before execution, compare the actual operation and current context to the complete Permit scope.
7. Re-evaluate if identity status, resource state, policy, purpose, executor, constraints, or other material context changed.
8. Enforce the Permit without widening it; if the PEP cannot enforce every condition, block.
9. Record decision reference and enforcement result separately from action outcome.
10. Independently verify the action result where required; never infer success from Permit.

A check performed by one component does not authorize a later operation at another component. Every protected boundary must enforce its own applicable decision.

## 10. Validity, revocation, and policy changes

- Permit validity must be bounded by explicit expiry or maximum age; no default duration is selected here.
- The PEP must reject expired, withdrawn, revoked, stale, or unverifiable decisions.
- Revocation must prevent subsequent protected operations according to a future defined propagation and enforcement contract.
- Long-running, paused, resumed, or retried operations must define re-check points and safe behavior when authorization expires or is revoked. A prior Permit does not automatically authorize a resumed or materially changed operation.
- Policy changes must be versioned. A decision must remain attributable to the policy/context used when issued.
- A withdrawn, incompatible, or no-longer-applicable policy/context requires fresh evaluation.
- Material changes to initiator or executor identity/status, authentication result freshness or revocation, delegation, resource ownership/classification/version, project boundary, purpose, environment, policy or trusted attributes, and constraints require fresh PDP evaluation. The PEP must independently revalidate the actual executor at the protected boundary.
- Caches cannot extend validity or conceal revocation. Cache semantics, maximum age, invalidation, and consistency must be defined before implementation.

A cached decision or cached authentication result is usable only if a future approved design establishes that its age, status, audience, policy/context version, and revocation guarantees satisfy the current operation. If required freshness or revocation state cannot be established, block.

No cache technology, duration, revocation interval, propagation guarantee, or long-running operation policy is selected by this contract.

## 11. Failure, retry, and recovery

| Condition | Required behavior |
| --- | --- |
| PDP unavailable, timeout, or transport failure | Indeterminate/non-permit; block protected action |
| Authentication evidence missing or invalid | Block; preserve distinction from policy denial |
| Authentication status, revocation, freshness, issuer, audience, or assurance cannot be validated where required | Indeterminate/non-permit; block and require valid identity evidence |
| Initiator, executor, or approver identity is conflated or executor is not independently authenticated | Reject request/approval; block |
| Required attribute missing, stale, untrusted, or conflicting | Indeterminate/non-permit; do not infer permissive value |
| Policy or attribute source is unavailable, withdrawn, or has an unknown current version | Indeterminate/non-permit; block pending authoritative current context |
| Response malformed, unknown, unsigned/unverifiable, or mismatched to request | Reject and block |
| Scope, audience, principal, executor, or resource mismatch | Reject and block; obtain fresh decision |
| Permit expired, revoked, withdrawn, or too old | Reject; re-evaluate if safe and appropriate |
| Approval absent, denied, expired, revoked, or unverifiable | Block; no inferred consent |
| PEP cannot enforce a constraint | Block; do not partially apply a broader grant |
| Policy version unavailable or incompatible | Block pending valid re-evaluation |
| Decision recorder unavailable | Follow separately approved event-class rules; never bypass authorization. Consequential operations default to block where required evidence cannot be preserved |
| Approval authority, approval status, or approval lifecycle evidence is unavailable or ambiguous | Approval is not established; block and do not infer consent |
| Operation result uncertain or partially applied | Stop blind retries; reconcile actual side effects and report uncertainty |

Retry behavior must distinguish transport retries from re-submitting or repeating the protected operation. A request may be retried only under bounded, defined conditions. The implementation design must define idempotency, duplicate suppression, correlation preservation, retry limits, timeout behavior, and recovery. An authorization retry never implies permission to repeat an action.

## 12. Decision and audit evidence

The logical record should allow later reconstruction of which request was evaluated, which decision was returned, which policy/context version was used, which approval was relied upon, which PEP enforced the result, and what action outcome was independently observed.

Record references and minimal metadata rather than raw credentials, secrets, full task content, or unnecessary sensitive attributes. Access, integrity, retention, correction, and deletion requirements remain to be designed.

Maintain distinct records/states for:
- authentication result reference and its validation outcome (without duplicating raw credentials);
- request received and validated;
- decision outcome;
- approval requested and resolved;
- PEP enforcement accepted or rejected;
- operation attempted, completed, failed, or uncertain;
- result independently verified or not verified.

Audit availability must not silently create a bypass. The treatment of recorder outages must be explicitly defined for each class of protected operation before implementation.

## 13. Privacy and data minimization

- Send only attributes necessary for the specific decision.
- Prefer opaque identifiers and references to source evidence over copying full source payloads.
- Do not include credentials, access tokens, private keys, or secret values in requests, responses, audit records, or repository documentation.
- Avoid copying authorization context into durable memory or general knowledge stores unless a separate approved purpose and lifecycle contract permits it.
- Keep sensitive decision metadata access-controlled under a future approved design.
- Do not use unrelated personal/task content as policy attributes without an approved, validated source and purpose.

## 14. Security considerations

Detailed design and validation must address at least:
- confused-deputy and privilege-escalation paths;
- forged, replayed, substituted, stale, or scope-expanded decisions;
- time-of-check/time-of-use races and context mutation;
- direct invocation or bypass of PEPs;
- caller-controlled or injected policy attributes;
- cross-project, cross-tenant, and cross-subject confusion;
- compromised approvers and approval phishing/coercion;
- policy rollback, withdrawal, and inconsistent versions;
- cache poisoning and delayed revocation;
- duplicate requests and uncertain side effects;
- audit tampering, repudiation, and sensitive metadata leakage;
- fail-open behavior during partial outages.

Prompt content, model output, workflow metadata, memory, and knowledge records must never be accepted as authority to grant or expand permission.

## 15. Validation requirements

Before implementation authorization, define isolated tests and acceptance criteria for:
- all four outcomes, stable reason categories, and unknown outcome handling;
- unavailable, stale, revoked, or audience-incompatible identity evidence and independent executor authentication;
- required request fields, provenance, validation, and malformed input;
- principal, executor, action, resource, purpose, audience, and scope mismatches;
- narrow Permit enforcement and attempts to broaden or reuse it;
- approval authority, explicitness, exact scope, expiry, revocation, and re-evaluation;
- expired, revoked, stale, forged, replayed, substituted, and withdrawn decisions;
- policy/context/attribute version changes, withdrawal, and material context mutation;
- cache freshness, revocation uncertainty, paused/resumed operations, and revalidation boundaries;
- PDP, identity, attribute-source, approval, and audit-recorder outages;
- PEP bypass attempts and direct component/adapter invocation;
- time-of-check/time-of-use races and long-running operations;
- duplicate requests, idempotency, retry bounds, partial effects, and recovery;
- cross-project/tenant/subject isolation;
- data minimization, audit integrity, and secret leakage;
- separation of authorization decisions from execution and verification evidence.

Tests must use isolated environments and synthetic or explicitly authorized data. Smoke tests, contract tests, security tests, feature tests, compatibility tests, and release approval remain separate. A test pass for one version does not approve another version or authorize release.

## 16. Deferred decisions and readiness gates

Still unresolved:
- future purpose, users, operating context, and protected operation catalogue;
- identity proofing, principal taxonomy, authentication assurance profiles, and any delegation model;
- exact request/response schema, API/protocol, and compatibility/versioning rules;
- policy model, policy language, policy administration, and policy conflict precedence;
- approval authority, authentication strength, emergency handling, and separation of duties;
- decision integrity mechanism, token/transport semantics, replay protection, and audience binding;
- validity, cache, revocation propagation, and long-running operation semantics;
- audit schema, integrity, access, retention, correction, and deletion;
- availability, latency, recovery, operational ownership, runtime, and deployment.

Readiness sequence:
1. Reconcile the approved logical contract with approved Core rules, D-008, and D-010; preserve their exact approved baselines.
2. Define purpose-dependent operations, policy authority, approval model, and trusted attribute sources after purpose selection.
3. Complete threat model, PEP boundary inventory, acceptance criteria, and isolated test plan.
4. Prepare a bounded implementation proposal with explicit dependencies and operational safeguards.
5. Obtain separate explicit implementation authorization.

These are design prerequisites, not defects in an implemented service. This approved logical contract authorizes no implementation, integration, tool access, policy administration, credential handling, runtime, or operational use.

## 17. Related records

- [Permission System — Logical Architecture](PERMISSION_ARCHITECTURE.md)
- [Identity Contract — Approved logical contract](IDENTITY_CONTRACT.md)
- [Permissions overview](README.md)
- [Core System](../../docs/CORE_SYSTEM.md)
- [Component Model](../../docs/COMPONENT_MODEL.md)
- [Workflow Management System](../../workflows/WORKFLOW_MANAGEMENT.md)
- [Implementation Readiness Framework](../../docs/IMPLEMENTATION_READINESS.md)
