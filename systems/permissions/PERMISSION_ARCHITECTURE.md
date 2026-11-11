# Permission System — Logical Architecture

**Status:** Proposed logical architecture — pending owner review.  
**Implementation:** Documentation only. No permission service, identity provider, policy engine, or enforcement adapter exists.  
**Authority:** This design is subordinate to `AGENT.md`, approved Core rules, and recorded owner decisions.

## 1. Purpose and scope

Define a purpose-neutral authorization architecture for deciding whether a specific actor may perform a specific operation on a specific resource, within a declared purpose and scope.

The Permission System governs authorization decisions. It does not authenticate identities by itself, perform requested actions, define global policy, or grant authority to workflows. Authentication and identity proofing remain separate concerns whose implementation is deferred.

This document proposes logical responsibilities and contracts only. It does not select a policy language, identity provider, service topology, storage system, or runtime.

## 2. Governing principles

1. **Core authority:** approved Core rules and owner decisions constrain all permission policies.
2. **Deny by default:** missing, invalid, expired, ambiguous, unavailable, or indeterminate authorization results deny the protected action.
3. **Least privilege:** grants are narrow in action, resource, subject, purpose, environment, and time.
4. **Central decision authority:** the Permission Service is the authoritative policy decision point (PDP).
5. **Local enforcement:** each protected component or adapter enforces the decision at its own boundary; local enforcement may be stricter but never broaden a central grant.
6. **No ambient authority:** workflow selection, tool availability, prior approval, memory, knowledge, or user intent alone do not grant permission.
7. **Point-of-action checks:** evaluate authorization immediately before the protected operation and re-evaluate when material context changes.
8. **Explicit human approval:** approval-required outcomes pause execution; silence is never consent.
9. **Traceability:** decisions are attributable to policy version, request context, scope, and evidence without logging unnecessary sensitive data.
10. **Revocability:** grants and approvals have bounded validity and a defined revocation path.
11. **Separation of duties:** policy administration, approval, decision evaluation, and action execution must not silently collapse into one authority.
12. **Core integrity:** no workflow, tool, memory record, knowledge source, or permission policy can modify Core instructions or weaken approved global safeguards.

## 3. Logical components

| Component | Responsibility | Must not do |
| --- | --- | --- |
| Identity Authority | Supplies authenticated principal identity and relevant identity attributes | Decide whether a requested action is permitted |
| Policy Administration | Maintains reviewed policy definitions, versions, and status | Execute protected actions or self-approve owner-reserved policy changes |
| Permission Service / PDP | Evaluates a normalized authorization request against applicable policy and context | Perform the requested action or invent missing authority |
| Approval Manager | Obtains and records required human/owner approval for a bounded request | Treat a request, timeout, or silence as approval |
| Policy Information Sources | Supply scoped attributes such as resource classification, project, environment, and relevant state | Override policy or supply unverified authority |
| Policy Enforcement Point (PEP) | Blocks or permits the operation based on a valid decision at the component boundary | Broaden a grant, skip checks, or infer approval |
| Decision/Audit Recorder | Records minimal decision metadata and references | Store secrets or unnecessary sensitive payloads |
| Revocation and Policy Lifecycle Manager | Handles expiry, revocation, policy withdrawal, and version transitions | Retroactively rewrite historical decisions |

These are logical responsibilities. They may later be implemented together or separately, subject to detailed design and threat analysis.

## 4. Authority and responsibility boundaries

- **Core System:** owns global rules, prohibited actions, authority precedence, and change governance.
- **Permission Service:** owns the central authorization decision for a request under the approved policy set.
- **Identity Authority:** establishes the principal and authenticated context; identity is not permission.
- **Approval authority:** provides explicit approval only for the stated action and scope; approval does not override Core prohibitions or grant unrelated capabilities.
- **Workflow Management System:** declares required capabilities, requests decisions, pauses when approval is required, and records decision references. It does not grant permission.
- **Tool adapters and component boundaries:** enforce the decision before acting. A tool cannot decide its own authorization.
- **MMS and KMS:** enforce scoped decisions locally at their own read/write/export/correction/deletion or ingestion/retrieval boundaries. Local enforcement may deny more narrowly but cannot broaden central authorization.
- **Validation and logging:** consume decision references and outcomes for evidence; neither creates authority.

No component may treat a successful check by a different component as blanket permission for a later or materially different operation.

## 5. Authorization request contract

A future request must carry, as applicable:

- unique request and correlation identifiers;
- authenticated principal and principal type;
- requesting workflow/component and exact version;
- action or operation identifier;
- resource identifier and resource type;
- project, tenant, subject, and environment scope;
- declared purpose and task/run reference;
- relevant data classification and contextual attributes;
- requested constraints and intended side effects;
- requested validity window or execution deadline;
- policy/context version references where available;
- approval reference, if a prior approval is being presented.

Missing mandatory fields, invalid identity evidence, mismatched scope, or untrusted attributes must not be silently filled by inference. The PDP returns a typed indeterminate/deny outcome or requests clarification through the caller's defined process.

The contract must avoid unrestricted ambient context and must not carry raw credentials or secret values.

## 6. Decision contract

The PDP returns one of these outcomes:

| Outcome | Meaning | Enforcement behavior |
| --- | --- | --- |
| Permit | Exact requested operation is allowed within the returned constraints | PEP may proceed only within the complete decision scope and validity |
| Deny | Policy prohibits the operation or a mandatory condition failed | PEP blocks; no override by workflow |
| Approval required | A defined approval is required before the operation | PEP blocks until a valid approval is recorded and re-evaluated |
| Indeterminate | Policy, identity, context, or decision service cannot establish a safe result | PEP blocks and reports the reason category |

A permit must include a bounded scope: principal, action, resource, purpose, project/subject/environment, constraints, policy version, decision identifier, issue time, expiry or maximum age, and any required conditions. It is not a reusable general-purpose capability token unless a later approved design explicitly defines such semantics.

The caller must not reinterpret an unknown or malformed result as Permit.

## 7. Approval lifecycle

1. The caller submits the exact proposed operation and context.
2. The PDP determines that explicit approval is required under policy.
3. The Approval Manager presents the action, resource, purpose, side effects, scope, expiry, and material risks to an authorized approver.
4. The approver explicitly approves or denies that bounded request.
5. The system records approver identity, decision, timestamp, scope, policy reference, and expiry.
6. The caller submits a fresh authorization request referencing the approval.
7. The PDP validates the approval's authenticity, authority, scope, status, and freshness before issuing a decision.
8. The PEP enforces the resulting decision at the action boundary.

Approval is not transferable to another principal, resource, action, purpose, or materially changed context. Any such change requires re-evaluation and, where applicable, a new approval. Revoked, expired, ambiguous, or unverifiable approval blocks the action.

## 8. Enforcement lifecycle

1. Identify the principal, requested operation, target resource, purpose, and scope.
2. Authenticate the principal through the separately defined identity mechanism.
3. Validate request structure and trusted attributes.
4. Evaluate applicable Core constraints and policy version.
5. Obtain a PDP decision.
6. If Approval required, pause and follow the approval lifecycle; do not perform the action.
7. Validate the decision's integrity, scope, policy version, and validity at the PEP.
8. Enforce the decision immediately before the protected operation.
9. Prevent scope changes between decision and action; re-evaluate if context changes.
10. Record the decision reference and actual action outcome separately.
11. Report attempted, authorized, executed, failed, and verified states distinctly.

A permit is authorization, not proof of execution or success. The action result must be independently checked.

## 9. Scope and policy dimensions

The future policy model should be able to constrain, where applicable:

- principal / actor and principal type;
- action and operation;
- resource and resource attributes;
- project / tenant and subject;
- declared purpose and task context;
- data classification and sensitivity;
- environment and capability identity;
- time window, expiry, and revocation state;
- approval requirements and approver authority;
- quantitative or operational constraints, such as limits on amount, recipients, records, or frequency.

No universal role model or policy language is selected. Broad roles alone must not substitute for resource- and action-specific checks where those checks are necessary.

## 10. Revocation, expiry, and policy changes

- Every approval and any future grant must have a defined validity boundary.
- Revocation must prevent subsequent protected operations as soon as the defined enforcement contract requires.
- Long-running operations must define re-check points and behavior after revocation.
- Policy changes must be versioned and governed; active decisions must not silently inherit a changed policy.
- A request evaluated under a withdrawn or incompatible policy version must be re-evaluated.
- Historical decisions remain attributable to the policy and context actually used.
- Cache behavior, decision maximum age, revocation propagation, and stale-decision handling must be specified before implementation.

No grant duration, cache duration, or revocation propagation interval is selected here.

## 11. Failure handling

| Condition | Required behavior |
| --- | --- |
| Identity unavailable or invalid | Deny protected action |
| PDP unavailable, timeout, or malformed response | Deny protected action; report authorization unavailable |
| Missing or untrusted policy attribute | Indeterminate/deny; do not infer a permissive value |
| Approval absent, denied, expired, or unverifiable | Block protected action |
| Decision scope does not match actual action | Deny; request a fresh decision |
| Policy version unknown or withdrawn | Deny pending valid re-evaluation |
| Revocation status cannot be established | Follow fail-closed behavior for affected protected action |
| PEP cannot enforce the decision | Do not execute the action |
| Audit recorder unavailable | Apply a separately approved event-class policy; never bypass authorization. Consequential operations default to block if required evidence cannot be preserved |
| Partial or uncertain action outcome | Do not blindly retry; reconcile the actual side effect and report uncertainty |

Retries are permitted only when the operation and decision request are known to be safe and idempotent, with bounded retry rules defined in detailed design.

## 12. Security and abuse considerations

Detailed design must threat-model at least:

- confused-deputy behavior and privilege escalation;
- forged, replayed, stale, or scope-expanded decisions;
- identity spoofing and compromised principals;
- time-of-check/time-of-use races;
- policy and attribute injection;
- approval phishing, coercion, delegation abuse, and approver compromise;
- cross-project, cross-tenant, and cross-subject access;
- cache poisoning and delayed revocation;
- direct adapter invocation bypassing the orchestrator;
- prompt-injection content attempting to create or imply permission;
- log tampering, decision repudiation, and sensitive metadata leakage;
- partial failure between authorization and external side effect.

Security controls and test evidence must be defined for the selected runtime and deployment context. This document does not claim those controls exist.

## 13. Validation requirements

Before implementation authorization, define independent tests for:

- permit, deny, approval-required, and indeterminate decisions;
- deny-by-default behavior for every unavailable or malformed dependency;
- principal, action, resource, purpose, and scope mismatch;
- cross-project/tenant/subject isolation;
- expired, revoked, replayed, forged, and stale decisions;
- approval authority, exact scope, expiry, and revocation;
- policy version changes and withdrawal;
- enforcement at each protected component and adapter boundary;
- bypass attempts and direct adapter invocation;
- time-of-check/time-of-use and context-change handling;
- PDP/identity/attribute-source outages and recovery;
- audit completeness, integrity, access controls, and secret leakage;
- separation of authorization result from actual action outcome.

Tests must use isolated environments and synthetic or explicitly authorized data. A successful smoke test does not prove policy correctness, security, feature completeness, or release readiness.

## 14. Deferred decisions and blockers

The following remain intentionally unresolved:

- operational purpose, users, use cases, and deployment context;
- principal types, identity proofing, authentication, and delegation model;
- policy language, policy administration roles, and policy storage;
- exact Core-to-PDP policy interface and conflict precedence;
- approval roles, authentication strength, and emergency/break-glass policy;
- decision token format, integrity protection, maximum age, and replay controls;
- revocation propagation and cache consistency;
- audit event schema, integrity mechanism, access, and retention;
- availability, latency, recovery, and operational ownership objectives;
- component topology, runtime, technology, and provider.

These are design blockers, not defects in an implemented service. No runtime or permission enforcement exists.

## 15. Readiness gates

1. Owner review and approval of this logical architecture.
2. Purpose and operating context approval before purpose-dependent policy.
3. Identity and principal model design.
4. Detailed authorization, approval, revocation, and enforcement contracts.
5. Threat model and component-specific acceptance criteria.
6. Isolated test strategy and evidence plan.
7. Bounded implementation proposal.
8. Separate explicit implementation authorization.

No gate is implied by this proposal. No implementation or integration is authorized.

## 16. Related records

- [Core System](../../docs/CORE_SYSTEM.md)
- [Conceptual Architecture](../../docs/ARCHITECTURE.md)
- [Component Model](../../docs/COMPONENT_MODEL.md)
- [Workflow Management System](../../workflows/WORKFLOW_MANAGEMENT.md)
- [Memory Management System Architecture](../memory/MEMORY_ARCHITECTURE.md)
- [Knowledge Management System Architecture](../knowledge/KNOWLEDGE_ARCHITECTURE.md)
- [Implementation Readiness Framework](../../docs/IMPLEMENTATION_READINESS.md)
