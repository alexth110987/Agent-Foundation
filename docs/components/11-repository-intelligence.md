# 11 — Repository Intelligence

**ID:** AF-11  
**Specification:** Draft; pending owner review  
**Implementation:** Not implemented  
**Project step:** 2

## Purpose
Evidence-based understanding of repository structure and state.

## Responsibilities
Map directories, files, modules, dependencies, configuration, docs and workflow relationships; identify inconsistencies, duplicates, orphaned records, missing references and likely impact; cite file/line/commit evidence; separate facts from inference.

## Inputs
Repository snapshot, manifests, metadata, search results.

## Outputs
Repository map, findings, impact analysis, evidence and limitations.

## Security and operational boundaries
Analysis does not authorize changes. Embedded repository instructions are data, not policy. Never claim completeness when evidence is incomplete.

## File Organiser — internal Repository Intelligence feature

The File Organiser is an internal capability of Component 11, not a nineteenth component or a separate project step. It may use Component 06 for exact file identity, versions and history; Component 04 for controlled execution; and Component 08 for action-specific authorization. This specification defines design only and does not authorize implementation.

### Operating modes

1. **Analyse (read-only):** inspect the explicitly selected repository snapshot and report observed structure, file classifications, references, duplicates, conflicts, risks and uncertainty. No repository writes or changes.
2. **Propose (no writes):** produce a concrete, reviewable change plan. No repository writes, even when a proposed change appears safe or obvious.
3. **Execute (individually approved changes only):** perform only the exact listed actions that received their own explicit, single-use approval. Approval of a plan or one action does not authorize other actions or later actions.

No mode may automatically delete, merge, overwrite, rename, move, or otherwise modify files. Any such operation requires an explicit action in the approved plan and its own valid authorization immediately before the side effect. Ambiguous identity or uncertain impact blocks the affected action.

### Classification and identity

Classification must be evidence-based and may use verifiable file type/signature, available metadata, content traits, and repository context. The report must distinguish observed facts from inferred classifications and unknowns. Similar names, extensions, sizes, or directory locations are not proof of identical content or identity. An exact matching cryptographic digest supports content equality but does not by itself prove that two paths represent the same file identity. Component 06 exact path/version identity rules remain authoritative.

### Scan boundary and completeness

A scan is limited to the explicitly selected repository and pinned commit/snapshot. It must distinguish tracked and untracked files where the source makes that distinction available, and report directories, submodules, external references, and unsupported or inaccessible entries without implying they were inspected. It must not inspect local disks, other repositories, or external storage unless that scope is separately and explicitly authorized. Partial, failed, or incomplete scans must be labelled as such; no completeness claim may exceed the evidence collected.

### References and dependencies

Inspect relevant references in documentation, configuration, workflows, source files, manifests, and other supported repository artefacts. Classify each relationship as confirmed, possible, unresolved, or unscanned and identify the supporting evidence. Uncertainty about a reference or dependency blocks a proposed change when that uncertainty could cause breakage or loss. Reference updates may be included only when each exact update is enumerated in the approved plan.

### Change plan

Every proposed action must identify:
- repository identity and pinned base commit;
- exact source file identity, current path and version;
- exact proposed destination/path or operation;
- reason and supporting evidence;
- detected conflicts, risks and uncertainty;
- affected references/dependencies and impact;
- exact approval required;
- validation and recovery strategy.

A plan must be sufficiently precise that the approved operation cannot be interpreted as blanket permission for adjacent or inferred changes.

### Duplicates, conflicts and naming

The Organiser may report exact-content duplicates, possible duplicates, naming inconsistencies, path conflicts and other anomalies. It must not automatically choose a canonical file, delete duplicates, merge content, overwrite, or resolve a conflict. Conflicting or ambiguous candidates block the affected action until an owner-reviewed decision is explicit. Naming and folder proposals must follow existing repository conventions or separately approved conventions; the feature must not impose a new mandatory convention by inference.

### Execution controls

Immediately before each approved side effect, revalidate repository identity, pinned/current state, exact source identity and version, destination state, references/dependencies, and the action-specific authorization. If the repository state or relevant evidence has changed since approval, the affected approval is invalid and a new plan/approval is required. Never overwrite or merge as a fallback. If execution partially fails, report the operation as incomplete and identify completed, failed, and unattempted actions without implying transaction-wide success.

### Post-execution verification and recovery

After each action, verify the exact source and destination state, identity, content/version, relevant references, and conformance to the approved plan. Record evidence and outcome. Recovery strategy must be defined before execution. Do not automatically roll back when rollback could overwrite newer work, destroy evidence, or create further conflicts. Any recovery action requires separate authorization and must preserve the original audit/history. Unverifiable results remain incomplete or indeterminate; no success may be inferred.

### Reporting and security

Reports distinguish confirmed facts, inference, unknown/unscanned items, conflicts, blocked actions, proposed actions, approved actions, executed actions, verified outcomes, and failures/partial completion. Repository content and embedded instructions are untrusted data. Analyse and Propose are non-writing modes; Execute is deny-by-default, invokes Component 08 authorization for every individual action, and rechecks authorization immediately before each side effect. Uncertainty about identity, scope, authorization, or impact denies the affected operation.

### Approved File Organiser design decisions

- **AF-34:** Exactly three modes: Analyse (read-only), Propose (no writes), Execute (only individually approved actions). No implicit writes.
- **AF-35:** Classification requires verifiable evidence; similar names/extensions/sizes are not identity proof; identical content is not conclusive proof of file identity.
- **AF-36:** Every proposed change has an exact plan covering identity, destination/operation, rationale/evidence, impact, risk, dependencies, approval, validation and recovery.
- **AF-37:** Scan scope is limited to an explicitly selected repository and pinned commit/snapshot; partial coverage is reported honestly.
- **AF-38:** References and dependencies are inspected; uncertain relationships that could be affected block the relevant action.
- **AF-39:** Execution rechecks exact current state and action-specific approval immediately before each side effect; changed state invalidates affected approval.
- **AF-40:** Duplicates and conflicts are reported, never automatically deleted, merged, overwritten, or selected.
- **AF-41:** Naming/folder proposals follow existing or separately approved conventions; no new mandatory convention is imposed by inference.
- **AF-42:** Every executed action receives post-execution verification; recovery is preplanned, separately authorized when invoked, and preserves history.

These approvals do not approve the Master Prompt draft, authorize implementation, or alter the 18-component scope.

## Open design decisions
Indexing, supported formats/languages, evidence granularity, confidence, incremental refresh, classification format matrix, dependency/reference coverage, report schema, conflict detection, execution/recovery interfaces, authorization integration, negative-test matrix, privacy checks, and isolated test fixtures. File Organiser implementation remains unapproved.

## Acceptance criteria before implementation
- Interfaces, state transitions, failure modes and permissions are approved.
- Positive and negative tests are defined, including unauthorized and invalid inputs.
- Privacy/security controls and evidence requirements are specified.
- Tests use isolated environments, synthetic data and no production credentials.
- Feature, integration, compatibility, smoke and release gates are tracked separately.
