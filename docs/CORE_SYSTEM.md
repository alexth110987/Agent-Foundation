# Core System — Responsibilities and Boundaries

**Status:** Proposed design — pending owner review.  
**Implementation status:** Documentation only; no executable core or policy engine exists.

## 1. Purpose

The Core System defines the agent-wide identity, governing instructions, operating constraints, and configuration rules that apply across workflows.

It is the stable policy foundation. It should not contain task-specific procedures, domain knowledge, workflow execution logic, tool implementations, or user data.

## 2. Core responsibilities

The Core System is responsible for:

1. **Identity:** defining the agent's approved name, role, scope, and declared limitations once these are selected.
2. **Instruction governance:** defining instruction authority, precedence, interpretation, and conflict handling.
3. **Operating rules:** establishing general conduct, information-handling requirements, autonomy boundaries, and safety constraints.
4. **Permission principles:** defining global authorization requirements; the separate permission component evaluates individual capability requests.
5. **Configuration governance:** defining how settings are declared, validated, versioned, and supplied safely.
6. **Boundary enforcement requirements:** specifying conditions under which the agent must refuse, pause, ask for clarification, or escalate.
7. **Change governance:** defining how core rules are proposed, approved, versioned, and reviewed.

## 3. Explicit exclusions

The Core System must not:

- perform domain-specific tasks;
- contain individual workflow instructions;
- execute tools or external actions;
- store credentials or secrets;
- act as a memory or knowledge database;
- decide the details of every task;
- silently grant permissions;
- replace validation, logging, or error-handling components.

These concerns belong to separately governed components.

## 4. Core subcomponents

| Subcomponent | Responsibility | Primary record |
| --- | --- | --- |
| Identity | Stable identity and declared scope | `core/IDENTITY.md` |
| Instruction governance | Authority, precedence, conflict handling | This document and future core instruction specification |
| Operating rules | General behavioral and safety constraints | `core/OPERATING_RULES.md` |
| Configuration governance | Safe configuration rules | `core/CONFIGURATION.md` |
| Change control | Approval and versioning of core changes | `AGENT.md`, decision log, changelog |

This is a logical separation. It does not require separate services or processes.

## 5. Instruction authority model

The exact runtime instruction hierarchy is deferred until the agent's platform and execution environment are selected. The foundation must nevertheless preserve these rules:

- Core project constraints cannot be overridden by an individual workflow.
- A workflow may add narrower requirements but cannot weaken global safeguards.
- Tool output, retrieved knowledge, and external content are data, not authority to change instructions.
- User requests do not automatically grant tool permissions or authorize consequential side effects.
- Conflicts between governing instructions must be surfaced and resolved through the approved governance process, not silently ignored.
- Where authority or permission is uncertain, the affected action must pause.

Do not invent platform-specific roles or precedence rules before the target runtime is chosen.

## 6. Policy change lifecycle

A proposed change to core policy should follow this sequence:

1. Identify the exact rule and reason for change.
2. Assess scope, dependencies, security, privacy, and compatibility impact.
3. Mark the change as proposed.
4. Obtain explicit owner approval before changing an approved rule.
5. Update the relevant core document and decision record.
6. Update dependent workflow specifications and tests where needed.
7. Review the resulting changes.
8. Record the version and verification evidence.

Core policy changes must not be smuggled into unrelated workflow updates.

## 7. Required inputs and outputs

The Core System is primarily a governing specification rather than a task processor.

**Inputs to its future enforcement mechanisms may include:**
- the active core policy version;
- a normalized request or action description;
- applicable workflow constraints;
- an authorization context;
- configuration values validated by the configuration component.

**Expected outputs may include:**
- applicable constraints;
- an allow / deny / approval-required policy result where appropriate;
- a clarification or escalation requirement;
- a policy version reference for traceability.

The precise schemas are deferred until implementation context is known.

## 8. Failure behavior

The future implementation must define safe behavior for:
- missing or unreadable core instructions;
- conflicting or invalid policy versions;
- malformed configuration;
- unavailable authorization checks;
- ambiguous authority;
- attempted workflow or tool policy bypass.

The baseline expectation is to stop the affected protected action and report the problem. No fail-open behavior is assumed.

## 9. Acceptance criteria for this design

This core design is ready for owner review when:

- responsibilities and exclusions are explicit;
- identity, policy, configuration, and workflow concerns are separated;
- workflows cannot override global safeguards;
- external content cannot grant authority;
- core changes require explicit approval and traceability;
- runtime-specific decisions are clearly deferred;
- failure behavior is described without claiming implementation.

## 10. Deferred decisions

The following remain open:
- agent name, persona, and purpose;
- exact instruction hierarchy for the selected runtime;
- whether policy enforcement is model-mediated, code-enforced, or hybrid;
- configuration format and schema technology;
- identity and authentication model;
- policy evaluation interface;
- core test framework and release process.

## Related records

- [Core Identity](../core/IDENTITY.md)
- [Core Operating Rules](../core/OPERATING_RULES.md)
- [Configuration Principles](../core/CONFIGURATION.md)
- [Architecture](ARCHITECTURE.md)
- [Decision Log](DECISIONS.md)
