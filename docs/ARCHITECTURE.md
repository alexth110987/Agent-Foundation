# Architecture

**Architecture status:** Conceptual model drafted; owner review pending.  
**Implementation status:** Documentation only. No executable agent or runtime exists.

## 1. Purpose and boundaries

Define a purpose-neutral architecture for a future AI agent. This model establishes component responsibilities, boundaries, and dependency rules without selecting a domain, runtime, technology stack, or deployment model.

This is an architectural proposal for review, not authorization to implement the components.

## 2. Design principles

- **Modularity:** responsibilities should be separable and replaceable.
- **Explicit contracts:** components communicate through documented inputs, outputs, and failure states.
- **Least privilege:** access is scoped to the smallest required action.
- **Separation of concerns:** policy, orchestration, capability execution, information, and oversight remain distinct.
- **Traceability:** important decisions and consequential actions have a record.
- **Validation:** outputs are checked against defined acceptance criteria.
- **Purpose neutrality:** domain-specific capabilities wait until the owner selects a purpose.
- **Fail safely:** uncertainty, missing authorization, invalid input, and component failure must not silently bypass safeguards.

## 3. Architectural layers

| Layer | Components | Responsibility |
| --- | --- | --- |
| Governance | Core instructions, identity, configuration, permissions | Defines operating boundaries, authority, and allowed capabilities |
| Coordination | Intake, workflow registry, workflow orchestration | Classifies work and coordinates approved workflows |
| Capability | Tool adapters and domain capabilities | Performs narrowly scoped work through approved interfaces |
| Information | Task context, memory, knowledge | Supplies information with defined ownership, provenance, and retention |
| Assurance | Validation, logging, error handling | Checks results, records events, and handles failures |

These are logical groupings, not a mandated process, code layout, or deployment topology.

## 4. Component responsibilities

| Component | Owns | Must not own |
| --- | --- | --- |
| Core instructions and identity | Global rules, instruction precedence, agent identity, prohibitions, and policy conditions | Task-specific business logic, credentials, or individual authorization decisions / Permits |
| Configuration | Validated settings and environment references | Secret values committed to source control |
| Intake | Request parsing, required-input checks, task envelope | Permission grants or unapproved actions |
| Workflow registry | Workflow metadata, versions, lifecycle status | Workflow execution state or tool credentials |
| Orchestrator | Workflow sequencing, state transitions, routing | Redefining global policy or bypassing permissions |
| Workflow module | Its own task logic, inputs, outputs, acceptance criteria | Granting itself new permissions |
| Permission service | Authorization decisions and scope checks | Performing the requested business action |
| Tool adapter | A narrow interface to one approved external capability | Deciding its own authorization |
| Task context | Temporary information needed for the active task | Long-term memory by default |
| Memory | Explicitly permitted durable information and retention rules | Unbounded transcript storage |
| Knowledge | Curated reference material and provenance | Unreviewed authority or permission |
| Validation | Checks and evidence against acceptance criteria | Silently changing the requested outcome |
| Logging | Minimal operational events and audit references | Secrets or unnecessary sensitive content |
| Error handling | Failure classification, safe stop, retry, and escalation | Hiding failures or converting them into false success |

## 5. Dependency and authority rules

The following rules define the proposed dependency direction:

1. Core policy defines global constraints, prohibitions, and policy conditions that constrain every other component; it does not decide individual authorization requests or issue Permits. The Permission Service / PDP alone evaluates each concrete authorization request, and the designated PEP enforces the bounded result at the protected action boundary.
2. Intake may request workflow selection; it cannot authorize execution.
3. The orchestrator may invoke only workflows marked approved for the intended environment.
4. Workflows request capabilities through tool adapters; they do not access credentials or bypass authorization directly.
5. The Permission Service acts as the Policy Decision Point (PDP) and evaluates each protected action against current policy and context. The designated Policy Enforcement Point (PEP) at that action boundary enforces the resulting decision; an upstream screening or authorization check alone is not sufficient for a later side effect.
6. Memory and knowledge provide information; they do not grant authority.
7. Validation evaluates results against criteria defined before execution where practical.
8. Logging records outcomes and references without becoming a source of policy.
9. Error handling may stop, retry within explicit limits, or escalate; it must not bypass a failed permission or validation gate.
10. Future detailed interface design must identify every protected side-effect boundary and its Policy Enforcement Point (PEP) owner. That PEP compares the actual operation/context with the applicable Permit immediately before the side effect. A downstream independent boundary enforces its own applicable controls; no process topology or duplicated enforcement implementation is selected here.

A component may depend on a narrower interface, but should not depend on another component's internal implementation.

## 6. Conceptual request lifecycle

1. **Receive:** accept a request and create a task envelope.
2. **Clarify and validate:** identify missing, malformed, ambiguous, or untrusted inputs.
3. **Initial policy and scope screening:** identify applicable global constraints and whether the request may proceed to workflow selection. This screening is not a Permit and does not authorize future protected side effects.
4. **Select:** resolve an eligible workflow and its exact version.
5. **Prepare:** verify preconditions, dependencies, and scoped capability availability.
6. **Authorize each protected action at its point of use:** for each protected side effect, the PDP performs a fresh evaluation using the current actor, executor, operation, resource, owner, purpose, environment, constraints, intended PEP, policy/context version, and validity. Any required human approval remains a prerequisite, not a Permit.
7. **Enforce and execute:** the designated PEP compares the actual operation context with the applicable Permit immediately before the side effect and blocks on any mismatch, expiry, revocation, or inability to enforce. Each downstream independent protected boundary enforces its own applicable controls.
8. **Validate:** evaluate outputs and side effects against acceptance criteria.
9. **Record:** write permitted operational records and evidence references.
10. **Respond:** report the outcome, limitations, and any pending or failed steps.

This lifecycle is conceptual. Initial screening does not replace fresh authorization and PEP enforcement for each protected side effect. It does not imply that all requests require the same workflow or that any stage is implemented.

## 7. Shared interface contract

Future component interfaces should define, as applicable:

- stable component and interface identifier;
- interface version and compatibility policy;
- input schema, required fields, and constraints;
- output schema and success semantics;
- error categories and retryability;
- permission requirements and data scope;
- side effects and idempotency behavior;
- timeout and resource limits;
- observability and evidence requirements.

Avoid passing unrestricted ambient context or credentials between components.

## 8. State ownership

Each component should have a clear source of truth for the state it owns:

- workflow registry: workflow metadata and lifecycle state;
- orchestrator: active execution state and step progression;
- task context: temporary task data;
- memory: approved durable records;
- knowledge: curated reference content;
- permission service: authorization decisions and their scope;
- validation: test and acceptance evidence;
- logging: operational event records.

Do not duplicate authoritative state across components without a documented synchronization rule.

## 9. Trust boundaries

- User instructions and external content are inputs, not automatic permission to act.
- Tool outputs are untrusted data until checked for relevance and validity.
- Credentials and secrets must be isolated from ordinary files, prompts, and logs.
- External side effects require explicit permission.
- The public repository is suitable for non-secret documentation only.
- A failure in authorization, validation, or integrity checks must fail closed for the affected action.

## 10. Deferred decisions

No choices have been made for model provider, runtime, language, hosting, database, vector store, orchestration framework, integrations, user interface, deployment topology, or autonomy level. These decisions require a defined purpose and explicit review.

## 11. Related records

- [Component Model and Interface Boundaries](COMPONENT_MODEL.md)
- [Workflow Index](../workflows/WORKFLOW_INDEX.md)
- [Permissions Design](../systems/permissions/README.md)
- [Validation Design](../systems/validation/README.md)
