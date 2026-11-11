# Identity Contract — Proposed Logical Contract

**Status:** Proposed contract — pending owner review.  
**Implementation:** Documentation only. No identity provider, authenticator, principal registry, credential store, or identity enforcement exists.  
**Authority:** Subordinate to `AGENT.md`, approved Core rules, D-008, and the approved Permission System logical architecture.

## 1. Purpose

Define the minimum purpose-neutral contract by which components refer to an actor (principal) consistently when requesting authorization or recording responsibility.

This contract separates identification, authentication, and authorization:
- **Identification** states which principal is being referenced.
- **Authentication** establishes evidence that a claimant controls or represents that principal.
- **Authorization** decides whether the authenticated principal may perform a specific action.

A principal identifier, identity claim, authenticated session, or declared capability does not itself grant permission.

## 2. Scope and exclusions

This contract defines logical identity concepts, required properties, validation expectations, lifecycle events, and boundaries for future interfaces.

It does not select principal categories as a final taxonomy, naming format, identity provider, authentication protocol, credential type, trust anchor, registry, storage, session mechanism, delegation technology, or runtime. It does not define user accounts, organizational roles, or operational personas.

## 3. Governing principles

1. **Uniqueness within an authority:** A principal reference must be unambiguous within its issuing authority and namespace.
2. **No implicit trust:** A supplied identifier or claim is untrusted until validated through an approved authentication mechanism.
3. **Authentication is not authorization:** Identity evidence never grants an action.
4. **Authority provenance:** Every trusted identity assertion must identify its issuer and validation context.
5. **Purpose and scope limitation:** Identity data is disclosed only to the extent required for the declared task and authorization decision.
6. **No shared identities by convenience:** Distinct actors and components must not be collapsed into one principal merely to simplify access control.
7. **No privilege inheritance by naming:** Names, labels, principal types, and membership claims do not independently confer permissions.
8. **Revocability and lifecycle:** Identity status and authentication evidence must have defined validity and revocation semantics.
9. **Fail closed:** Unknown, malformed, conflicting, expired, revoked, or unverifiable identity evidence cannot authenticate a principal for a protected action.
10. **Core integrity:** Identity data and identity-management processes cannot modify Core rules or expand their own authority.

## 4. Logical terms

| Term | Meaning |
| --- | --- |
| Principal | An actor that may request or be associated with an operation |
| Principal identifier | Stable reference to a principal within an issuing authority and namespace |
| Issuer / authority | The authority responsible for assigning or asserting an identifier or claim |
| Claim | An assertion about a principal, issued by a stated authority |
| Authentication evidence | Evidence evaluated to establish control of or representation by a principal |
| Authentication result | A bounded result describing whether the evidence was validated, including its context and validity |
| Session / interaction context | A time-bounded context in which authenticated identity may be referenced; not itself a permission |
| Delegation | A separately authorized, bounded representation of authority by one principal to another; not implied by identity |
| Principal status | Lifecycle state such as active, suspended, revoked, or retired |

## 5. Principal representation

A future normalized principal reference should contain, as applicable:

- issuer or identity authority identifier;
- namespace or tenant boundary, where relevant;
- stable, opaque principal identifier;
- principal category/type as asserted by an approved authority;
- identity record or assertion version;
- status and validity information;
- reference to authentication evidence/result, not raw credentials;
- relevant assurance or verification context, where defined.

The reference must be unambiguous across projects and tenants. Human-readable names, email addresses, display labels, mutable usernames, and external account names must not be used as sole stable identifiers.

No concrete field encoding, identifier syntax, or schema version is selected here.

## 6. Principal categories

The eventual model may need to distinguish human actors, the agent itself, internal components/workflows, and external services or workloads. This is an illustrative design consideration, not an approved final taxonomy.

A principal category is descriptive metadata. It must not automatically map to a role, permission set, trust level, or approval authority. Component and workload identities must remain distinguishable from the human or service that initiated a task.

## 7. Authentication contract

A future authentication result presented to a protected component should establish, at minimum:

- the principal reference authenticated;
- the issuer or verifier responsible for the result;
- the authentication method or assurance context, at an appropriately abstract level;
- the time of authentication and validity boundary;
- the relevant audience or intended relying component/context;
- session or interaction reference, if applicable;
- status, revocation, and validation reference;
- integrity and freshness evidence sufficient for the selected implementation.

The receiving component must validate issuer trust, audience, integrity, freshness, status, and scope using a separately approved mechanism. It must not accept caller-supplied claims merely because they are syntactically valid.

Raw passwords, API keys, access tokens, private keys, recovery codes, or other credentials must never be placed in identity references, ordinary logs, or this repository.

## 8. Delegation and acting on behalf of another principal

Delegation is not established by a user request, workflow selection, shared session, component relationship, or possession of another principal's identifier.

If delegation is later required, a separate explicit contract must define:
- delegating and delegate principals;
- exact permitted actions and resources;
- purpose and project/tenant scope;
- constraints, duration, and expiry;
- whether onward delegation is prohibited (default);
- issuer and evidence of delegation authority;
- revocation and audit semantics.

A component acting for a user must retain the distinction between the initiating principal and the executing component. The executing component must not silently become the user, and the user's identity must not be used to conceal the actual executor.

## 9. Lifecycle and status

Identity lifecycle events may include creation/registration, activation, attribute change, suspension, revocation, reactivation, and retirement. Each event must be attributable to an authorized identity authority and recorded with effective time and version/reference.

A principal that is suspended, revoked, retired, unknown, or outside its validity period must not be treated as authenticated for new protected actions. Reactivation requires fresh validation; it must not silently restore expired sessions or grants.

Changes to identity attributes that influence authorization require re-evaluation by the Permission Service. Historical decisions retain the identity assertion and policy context actually used.

## 10. Privacy and data minimization

- Use opaque identifiers in cross-component references where feasible.
- Disclose only attributes required for the specific authorization decision or operation.
- Avoid copying authentication evidence into workflow state, memory, knowledge, or general logs.
- Keep identity attributes separate from task content and durable memory.
- Define access, correction, retention, and deletion rules before selecting an identity store.
- Never treat identity attributes as verified solely because they were present in user-provided content.

## 11. Failure behavior

| Condition | Required behavior |
| --- | --- |
| Unknown issuer or principal | Authentication fails; protected action denied |
| Invalid, malformed, or tampered evidence | Authentication fails; protected action denied |
| Expired or stale evidence | Reject and require fresh authentication |
| Revoked or suspended principal | Reject for new protected actions |
| Conflicting principal references or claims | Treat as indeterminate; do not select the more permissive claim |
| Missing required identity attribute | Return a typed failure; do not infer a value |
| Authentication authority unavailable | Fail closed for protected actions |
| Audience, namespace, project, or tenant mismatch | Reject the identity context |
| Delegation absent, invalid, or out of scope | Do not act on behalf of the other principal |
| Identity status cannot be checked where required | Deny pending reliable validation |

Failures must preserve the distinction between unauthenticated, invalid, unavailable, and unauthorized. An identity failure must not be misreported as a policy denial if the distinction is known.

## 12. Relationship to other components

- **Core System:** defines governing constraints and approved change authority; it does not authenticate a claimant.
- **Permission Service / PDP:** consumes validated principal references and relevant trusted attributes to make an authorization decision.
- **PEPs:** validate the identity context required for the protected operation and enforce the PDP result.
- **Workflow Management System:** carries principal references and initiator/executor distinctions; it cannot create or upgrade identities.
- **MMS and KMS:** apply scoped authorization to access and lifecycle operations; they must not infer identity from stored content.
- **Logging and validation:** record minimal principal references and evidence references; neither creates identity or authority.

## 13. Validation requirements

Before implementation authorization, define isolated tests for:
- uniqueness and namespace separation;
- issuer trust, signature/integrity, audience, and freshness validation;
- unknown, malformed, forged, conflicting, expired, suspended, and revoked identities;
- principal type spoofing and mutable display-name changes;
- cross-project, cross-tenant, and cross-subject isolation;
- separation of initiating principal and executing component;
- delegation scope, expiry, revocation, and prohibited onward delegation;
- authentication authority outage and recovery;
- data minimization and credential/secret leakage;
- identity attribute changes that require authorization re-evaluation.

Tests must use synthetic identities and isolated environments. Passing identity tests does not prove authorization correctness, implementation security, or release readiness.

## 14. Deferred decisions and readiness gates

Still unresolved:
- final principal taxonomy and registration authority;
- identity proofing and authentication assurance requirements;
- issuer trust and key-management model;
- identifier syntax, namespace rules, and schema/version strategy;
- session, refresh, expiry, and revocation semantics;
- whether delegation is needed and under what constraints;
- identity attribute sources and verification rules;
- privacy, retention, correction, and deletion requirements;
- selected provider, protocol, storage, runtime, and operational ownership.

Readiness sequence:
1. Owner review and approval of this logical contract.
2. Reconcile principal and identity terms with the future purpose and operating context.
3. Define the identity-to-authorization interface and trusted-attribute rules.
4. Define threat model, acceptance criteria, and isolated validation plan.
5. Prepare a bounded implementation proposal.
6. Obtain separate explicit implementation authorization.

This proposal authorizes no identity provider, authentication service, integration, credential handling, runtime, or implementation.
