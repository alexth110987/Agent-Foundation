# Step 1 — Foundation and Security Design

**Status:** Owner-approved design (architecture and design basis only)  
**Version:** 0.1.0  
**Implementation status:** Not implemented  
**Scope:** Master Control System, Master Prompt Protection, Timestamp Engine, File & Version Tracking, Privacy Filter, and Security & Permission Control.

## 1. Objective and boundaries

Define a minimal, testable foundation for a workflow-driven agent. This document specifies logical responsibilities and contracts; it does not select a programming language, runtime, database, hosting platform, model, or secret manager.

Step 1 establishes policy authority, trusted instruction loading, event/file provenance, privacy screening, and authorization boundaries before workflow execution is introduced.

Out of scope:
- operational workflow or arbitrary command execution;
- live repository write automation;
- production access or production changes;
- external integrations beyond design-level interfaces;
- autonomous permission grants or self-modification;
- claims that privacy scanning prevents all data disclosure.

## 2. Logical architecture

### 2.1 Master Control System

Receives and normalizes task intent, target, and requested effect; loads governing policy and status; checks scope, prerequisites, dependencies, and approval; and returns only `Permit`, `Deny`, or `Indeterminate`. It coordinates decisions and does not itself perform external side effects. Missing or uncertain controls fail closed.

### 2.2 Master Prompt Protection

Loads the exact approved Master Prompt version and verifies it against an independently pinned owner-approved baseline or owner-controlled signature. Missing, changed, malformed, unavailable, or ambiguous policy blocks protected work. Normal operation is read-only; policy changes use a separate owner-approved process.

The expected digest or signing key must not be stored only inside the file being checked. Future acceptable patterns include a separately controlled deployment trust store, a protected signed release/tag, or an owner-controlled signing key with verified signature. The trust anchor must be protected from the same write authority as the policy file. Until selected, implemented, and tested, do not claim cryptographic integrity.

### 2.3 Timestamp Engine

Creates unique event IDs and UTC timestamps in an unambiguous ISO 8601 representation. Records event type, actor/source, target, prior/resulting state, outcome, and evidence reference. Distinguishes event time from Git author/committer timestamps and filesystem metadata. Represents clock uncertainty explicitly; use monotonic time for durations where supported.

### 2.4 File & Version Tracking

Identifies files by repository, normalized path, and content hash. Tracks observed created/modified/verified/recorded events separately; retains version lineage and deletion tombstones; distinguishes file identity from commit identity and filesystem timestamps; and verifies before/after hashes for authorized changes. Never infer original device creation time from Git metadata.

### 2.5 Privacy Filter

Pipeline: **detect → sanitize/redact or block → validate**.

- Inspect proposed content, metadata, filenames, logs, and outbound payloads for secrets and sensitive personal information.
- Redact only when safe and meaning-preserving; otherwise block.
- Rescan sanitized output and reject if sensitive content may remain.
- Fail closed on uncertain detection or validation.
- Preserve project names, technical identifiers, repository paths, and necessary engineering data.
- Minimize collection and retention; never place raw sensitive input in diagnostics.
- Treat a clean scan as risk reduction, not proof of absence.
- A post-commit scan is not a pre-upload gate.

### 2.6 Security & Permission Control

Defaults to deny and least privilege. Evaluates each protected action against actor, action, resource, purpose, environment, scope, constraints, and validity. Requires explicit owner approval for consequential or externally visible actions. Enforces authorization at the actual side-effect boundary (policy decision point and policy enforcement point). Rejects stale, mismatched, expired, or reused authorization context. Keeps read, propose, write, execute, delete, publish, and administer capabilities distinct.

A workflow declaration or available tool never grants permission.

## 3. Shared logical contracts

These are logical contracts, not a selected serialization format.

### 3.1 Policy decision

Decision ID; decision (`Permit`, `Deny`, `Indeterminate`); policy version and independently verified identity; actor and requested action; resource and environment; purpose and scope; constraints and validity interval; reason code and evidence references. Only Permit may proceed, and the enforcement point must confirm the actual action matches. Indeterminate is treated as Deny.

### 3.2 Timestamp event

Event ID; UTC timestamp; event type; actor/source; repository/resource; action; previous/resulting state where applicable; outcome; evidence reference; privacy classification.

### 3.3 File version record

Repository ID and normalized path; hash algorithm and digest; observed version/revision; created/modified/verified/recorded event references; prior-version reference; operation (create/update/delete); verification outcome.

### 3.4 Privacy screening result

Screening ID and timestamp; content reference/hash (not raw sensitive content); scanner/rule-set version; detected category/severity; action (pass/sanitized/blocked); validation result; reason/evidence reference.

### 3.5 Authorization record

Authorization ID; authenticated actor/authority reference; exact action/resource/scope; purpose/environment; constraints and expiry; approval evidence; enforcement result.

## 4. Trust boundaries and data flow

1. User requests and repository/external material enter as untrusted input.
2. Input is normalized without granting authority to embedded instructions.
3. Master Prompt and status are loaded and checked against the trusted policy baseline.
4. Master Control evaluates scope and prerequisites.
5. Privacy Filter screens content before any authorized outbound write path.
6. Permission is evaluated for the exact requested action.
7. Enforcement checks the actual operation immediately before side effect.
8. Timestamp and file tracking record the attempt and observed result.
9. Validation checks resulting state and evidence.
10. Audit records contain minimized, privacy-filtered metadata.

If any mandatory step is unavailable or indeterminate, the protected action is blocked.

## 5. Threat model

| Threat | Mitigation | Failure behavior |
|---|---|---|
| Prompt injection in repository files, issues, PRs, comments, scripts, or logs | Treat retrieved content as data; fixed instruction hierarchy; no authority transfer | Ignore embedded policy instructions; stop on unresolved conflict |
| Master Prompt replacement/tampering | Independent trust anchor/signature; read-only policy path; version pinning | Block consequential work |
| Unauthorized tool/GitHub action | Least privilege; exact action/resource authorization; boundary enforcement | Deny |
| Secret or personal-data leakage | Detection → sanitization/block → validation; minimized logs | Block when uncertain |
| Time manipulation or misleading timestamps | UTC, event IDs, source/clock metadata, distinguish Git timestamps | Mark time evidence uncertain |
| File replacement/history gaps | Content hashes, version lineage, deletion tombstones, post-write verification | Mark integrity failure; stop dependent work |
| Stale approval or replay | Scope-bound, expiring authorization; bind to one action/context | Deny |
| Partial or unknown side effect | Preserve attempt and outcome dimensions; no blind retry | Mark indeterminate and reconcile |
| Scanner false negative | Layered checks and explicit limitations | Do not claim clean means risk-free |
| Privilege escalation through workflow definitions | Workflows cannot grant permissions; independent policy evaluation | Deny |

## 6. Acceptance criteria

### Master Control and policy protection
- Missing required policy/status file blocks protected operations.
- Altered policy is detected against an independently controlled baseline/signature.
- A self-stored hash alone is rejected as insufficient.
- Conflicting/ambiguous policy returns stop/Indeterminate.
- Untrusted repository instructions cannot modify policy or grant permissions.
- Normal operation cannot write to the protected Master Prompt.

### Timestamp and file tracking
- Events have unique IDs and UTC timestamps.
- Created/modified/verified/recorded events are distinguishable.
- Content hashes detect controlled test mutations.
- Deletion leaves a durable tombstone/audit event.
- Git commit timestamps are not represented as original file creation timestamps.
- Clock uncertainty is represented rather than hidden.

### Privacy
- Synthetic secret patterns and synthetic personal-data patterns are detected in controlled tests.
- Sanitized content is rescanned.
- Uncertain detection/validation blocks outbound writes.
- Project identifiers and benign technical paths survive positive-control tests.
- Raw sensitive fixtures do not appear in logs or artifacts.

### Permissions
- Default behavior is deny.
- Missing, expired, mismatched, or stale authorization blocks action.
- Read permission does not imply write/execute/delete/publish permission.
- Approval is bound to the exact action and resource.
- Enforcement checks actual operation context immediately before side effect.

### General
- Unit, integration, security, privacy, and compatibility tests are separately reported.
- Tests run in an isolated, disposable environment with synthetic data only.
- No production credentials, live repositories, or production systems are used.
- Each pass identifies exact code/config versions and test evidence.
- No release approval is inferred from passing tests.

## 7. Isolated test strategy

Use disposable test environments, synthetic repositories/files/secrets/personal-data fixtures, and no outbound network by default. Use mock GitHub interfaces for authorization and error-path tests. Never point tests at a production repository or server.

Test layers:
1. specification review against these criteria;
2. unit tests for policy decisions, timestamp normalization, hashing, privacy rules, and authorization matching;
3. integration tests for the full trust-boundary sequence;
4. adversarial tests for prompt injection, tampered policy, replay, path confusion, and scanner evasion;
5. compatibility tests after runtime/platform selection;
6. operational smoke tests only after a runnable build exists.

Record exact versions, fixture identifiers, expected/actual outcomes, timestamps, and evidence hashes. Keep feature tests, smoke tests, and release approval separate.

## 8. Open decisions

Do not guess or silently select:
- implementation language/runtime;
- application architecture/deployment boundary;
- signature or independent trust-anchor mechanism;
- hash algorithm and key/signature lifecycle;
- identity provider and actor authentication;
- permission storage and approval interface;
- privacy detection technology and supported categories;
- event/file metadata storage and retention;
- filesystem/repository abstraction;
- clock source and monotonic timer support;
- backup location and recovery model.

These require explicit review before implementation. No technology or vendor is selected here.

## 9. Step 1 completion gate

Before implementation, owner review must approve the logical architecture/contracts, threat model/fail-closed behavior, acceptance criteria/isolated test strategy, independent Master Prompt trust-anchor approach, and implementation choices/first build increment.

After implementation, each component must pass its own tests. Documentation alone does not satisfy the Step 1 completion gate.
