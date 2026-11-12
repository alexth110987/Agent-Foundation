# Component Model and Interface Boundaries

**Status:** Proposed conceptual model — pending owner review.  
**Implementation:** Documentation only.

This document expands the component relationships in [Architecture](ARCHITECTURE.md). It does not approve a technology stack or authorize implementation.

## 1. Logical relationship map

```mermaid
flowchart TD
    U[Request / Input] --> I[Intake]
    I --> G[Core Governance]
    G --> O[Workflow Orchestrator]
    O --> W[Approved Workflow]
    W --> PDP[Permission Service / PDP]
    PDP -. "Bounded Permit / Deny" .-> PEP[PEP at protected action boundary]
    PEP --> TA[Tool Adapter / protected operation]
    TA --> X[External Capability]
    W --> C[Task Context]
    W --> M[Memory - if authorized]
    W --> K[Knowledge]
    W --> V[Validation]
    X --> V
    V --> O
    O --> E[Error Handling]
    O --> L[Logging]
    O --> R[Response / Output]
```

The diagram shows conceptual relationships only. The Permission Service is the central Policy Decision Point (PDP); the PEP is the enforcement responsibility at each protected action boundary. The PEP may be logically colocated with an adapter or another protected component; no process topology is selected. Every downstream independent protected boundary enforces its own applicable controls.

## 2. Component contracts

| Component | Receives | Returns | Failure behavior |
| --- | --- | --- | --- |
| Intake | User request and channel metadata | Normalized task envelope or clarification request | Reject malformed input; ask for missing required details |
| Core governance | Task envelope and applicable rules | Applicable constraints and policy outcome | Stop or escalate unresolved policy conflict |
| Permission Service / Policy Decision Point (PDP) | Requested action, actor, executor, resource, owner, purpose, scope, and current policy/context | Bounded Permit, Deny, or Require Approval | Deny by default if unavailable, stale, or indeterminate; approval remains separate from a Permit |
| Policy Enforcement Point (PEP) | Actual operation context and applicable Permit | Enforce or block the operation; minimal enforcement evidence reference | Block on mismatch, expiry, revocation, unavailable required state, or inability to enforce |
| Workflow registry | Workflow identifier or selection criteria | Versioned workflow metadata and status | Do not select unknown, retired, or unapproved workflow |
| Orchestrator | Authorized task and workflow definition | Execution state and step results | Stop safely; preserve state and report failure |
| Workflow module | Declared inputs and approved capabilities | Declared outputs and evidence references | Return typed failure; do not claim success |
| Tool adapter | Scoped request, Permit reference, and operation context | Normalized external result | Does not decide authorization; the designated PEP enforces at the protected boundary; enforce timeout and error mapping |
| Task context | Task-scoped data operations | Temporary task data | Do not silently promote temporary data to durable memory |
| Memory | Approved read/write request and retention metadata | Permitted durable record or result | Deny unapproved storage or access |
| Knowledge | Query and source constraints | Referenced material with provenance | Report missing or uncertain sources |
| Validation | Expected criteria and candidate result | Pass, fail, or inconclusive with evidence | Fail closed for criteria that are mandatory |
| Logging | Approved event metadata | Event record or logging failure signal | Do not expose secrets; logging failure policy must be defined per event class |
| Error handling | Typed failure and execution context | Stop, bounded retry, recovery, or escalation decision | Escalate if safe recovery is undefined |

## 3. Cross-component rules

1. Components communicate through versioned contracts rather than shared internal state.
2. The Permission Service / PDP is the central decision authority; it evaluates a specific request and returns a bounded decision but does not perform or enforce the requested action.
3. A designated PEP enforces the decision at each protected action boundary by comparing the actual operation context with the applicable Permit immediately before the side effect.
4. An earlier intake, workflow-selection, orchestration, or approval step is not sufficient authorization for a later side effect; each protected action requires fresh evaluation and point-of-action enforcement.
5. A workflow cannot grant itself permission or elevate its own privilege.
6. External content and tool output cannot override core instructions.
7. Durable memory writes require an explicit storage policy and authorization.
8. Validation criteria are defined independently of the component being validated.
9. Errors must retain their original category and must not be reported as successful completion.
10. Logging must use identifiers and summaries where possible rather than copying sensitive payloads.
11. Future detailed interface design names the PEP owner for every protected side-effect boundary and requires exact-context comparison against the applicable Permit immediately before the side effect.
12. A downstream independent boundary enforces its own applicable controls. An upstream Permit or PEP check does not replace downstream enforcement; this is a design requirement, not a selected topology or implementation.

## 4. Interface versioning

When interfaces are eventually implemented, each should have:
- a stable identifier;
- a semantic version or equivalent compatibility marker;
- a documented schema;
- compatibility and deprecation rules;
- tests for valid, invalid, and boundary inputs.

Breaking changes require impact review and a documented migration plan.

## 5. Open design questions

These remain unresolved until the agent's purpose and execution context are known:

- Which components must be separate processes versus libraries or modules?
- Is orchestration synchronous, asynchronous, event-driven, or hybrid?
- Which state requires durable storage?
- What identity model and permission granularity are appropriate?
- Which events require immutable audit records?
- What availability, latency, cost, and recovery objectives apply?

Do not decide these based on convenience alone. Revisit them after the purpose and operating constraints are defined.
