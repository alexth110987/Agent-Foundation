# Documentation and Version Governance — Approved

**Status:** Approved under D-014 (2026-11-12).  
**Approved source baseline blob SHA:** `beb94648d9308fed79695ccb1d9184c0f0725c89`  
**Version:** 0.1.0  
**Last updated:** 2026-11-12  
**Scope:** Repository documentation and decision-record governance only. This proposal does not change the authority of AGENT.md or approve a runtime document hierarchy.

## 1. Purpose

Establish a consistent way to identify, review, approve, version, preserve, supersede, and retire project documents. Git history remains the authoritative record of content changes; this proposal adds document-level controls so approval and baseline scope are unambiguous.

## 2. Authority and sources of truth

1. `AGENT.md` remains the highest-authority project/repository instruction document.
2. `AGENTS.md` remains subordinate and governs repository maintenance only.
3. `docs/DECISIONS.md` records owner-approved decisions and their scope.
4. `docs/PROJECT_STATUS.md` reports current state; it does not create approval.
5. `docs/CHANGELOG.md` summarizes material repository changes; it does not replace Git history or decision records.
6. An approved artifact is defined by the exact path, Git blob SHA, decision reference, and approved scope recorded in its decision.
7. A current file may contain a clearly separated status annotation while the approved source blob remains the immutable design baseline. Such an annotation must not be presented as part of the original approved content.

Conflicts between records must be surfaced and resolved through an explicit owner decision. A newer timestamp or later commit does not by itself supersede an approval.

## 3. Document identity and metadata

Assign each durable, independently governed document a stable identifier that does not change when its title or path changes. Proposed identifier pattern: `AF-[AREA]-[NNN]`. Area labels such as CORE, ARCH, VAL, DOC, WF, MEM, KNOW, PERM and OPS are examples only, not a finalized taxonomy.

A document header or registry entry should record, where applicable: stable document ID; title and current repository path; status (Proposed, Approved, Implemented, Verified, Deferred/On hold, or Rejected); document version; accountable owner or review role; creation and last-updated dates; applicable decision IDs; approved baseline blob SHA, if any; supersession references; scope, exclusions and known limitations.

Do not assign approval status merely because a document exists in the repository. Keep document status distinct from the status of the design it describes.

## 4. Proposed versioning model

Use a document-level MAJOR.MINOR.PATCH version for durable documents if the owner adopts this policy:

- **MAJOR:** an owner-approved substantive change that alters purpose, scope, responsibilities, interfaces, constraints, or compatibility assumptions.
- **MINOR:** an approved additive change that preserves existing meaning and compatibility but adds requirements, sections, or defined coverage.
- **PATCH:** editorial corrections, formatting, links, metadata, or non-substantive status annotations that do not change approved meaning.

A version increment does not grant approval. Approval is separately recorded against an exact blob SHA and decision ID.

The repository changelog's existing release-like numbering is a separate repository change-history convention. Do not assume it is the version of every document. Whether to adopt tags, automated version checks, or a central document registry is deferred for owner review.

## 5. Change classes and required handling

| Change class | Examples | Required handling |
| --- | --- | --- |
| Editorial | Typo, formatting, broken link | Review for meaning preservation; record in changelog when material |
| Status/metadata | Approved annotation, owner/status/date correction | Keep annotation clearly separated from approved baseline content; preserve baseline SHA |
| Additive non-substantive | Clarifying examples that do not change normative meaning | Review for scope/meaning impact; document version increment if adopted |
| Substantive | Requirements, permissions, interfaces, lifecycle semantics, data handling, ownership, acceptance criteria | Impact review, explicit owner approval, new exact baseline SHA and decision record |
| Supersession/retirement | Replaced contract or withdrawn proposal | Explicit decision, successor/reason/effective scope, migration or retention notes |

If it is unclear whether a change is substantive, treat it as substantive until reviewed. Never rewrite an approved baseline in place and silently retain its former approval label.

## 6. Approval and review lifecycle

1. **Draft:** author prepares a clearly marked Proposed document.
2. **Internal consistency review:** check authority, scope, terminology, references, dependencies, exclusions, and conflicts.
3. **Owner review:** record findings, unresolved questions, and exact artifact/version under review.
4. **Decision:** owner approves, rejects, defers, or requests revision. Approval must identify the exact Git blob SHA and scope.
5. **Registration:** record the decision in `docs/DECISIONS.md`; update Project Status and changelog where relevant.
6. **Post-approval changes:** classify the change, assess impact, obtain required authorization, and create a new baseline when substantive.
7. **Periodic/event-driven review:** review when dependencies, architecture, risks, or assumptions change. A fixed cadence is not selected while the project has no operating context.
8. **Supersession/retirement:** preserve history and record what replaces the artifact, why, and which scope remains valid.

A review comment, merge, commit, or changelog entry alone does not constitute owner approval unless the decision record explicitly says so.

## 7. Baseline integrity and traceability

For every approved artifact, retain its canonical path and stable document ID; exact approved Git blob SHA; decision ID and approval date; approved scope and explicit exclusions; later annotation or successor references; and change/impact history.

Do not force-push or rewrite canonical history to conceal a superseded or incorrect artifact. Corrections should be new commits with traceable rationale. If an approved artifact must change, preserve the old blob reference and record a new approval against the new exact blob.

Cross-document references should use stable IDs where available, human-readable titles, and repository-relative paths. References to exact approved baselines should include decision and blob SHA where the distinction matters.

## 8. Repository records

- **README:** orientation and navigation; concise current milestone and document map.
- **Decision Log:** durable owner decisions, rationale, scope, impact, exact baselines, and unresolved conditions.
- **Project Status:** current confirmed/proposed/implemented/verified/deferred state and next milestone.
- **Changelog:** dated summary of material additions, changes, corrections and removals.
- **Document metadata/registry:** stable identity, status, version and baseline references, if adopted.
- **Git history:** exact content evolution, commit authorship and chronology.
- **Validation evidence:** exact check/version/environment and result, governed by the Validation Strategy.

No one record substitutes for the others. Keep entries concise and cross-reference rather than duplicating entire specifications.

## 9. Review checklist

- [ ] Is the document ID stable and unique?
- [ ] Is its status accurate and distinct from implementation/verification?
- [ ] Is the proposed version appropriate to the change class?
- [ ] Does it conflict with AGENT.md, an approved decision, or another contract?
- [ ] Are exact approved blob SHAs preserved?
- [ ] Is the change editorial, metadata-only, additive, or substantive?
- [ ] Is explicit owner authorization required and present?
- [ ] Are scope, exclusions, open questions and downstream impacts recorded?
- [ ] Are README, Project Status, Decision Log and Changelog updated where affected?
- [ ] Can a future reviewer reconstruct the decision and exact artifact from repository history?

## 10. Current limitations and open decisions

- No stable document-ID registry or document-level versioning convention is currently implemented.
- The approved policy leaves the ID taxonomy, metadata format, version increments, review roles, review cadence, retention, tagging, and automation open for later decisions where stated; approval does not silently finalize those deferred choices.
- This proposal does not alter D-005 through D-013, any approved baseline, or the Core authority map recorded under FCR-04.
- This approved policy does not select a runtime, tool, repository automation, or document-management service.
- The project remains Stage 1; no executable capability is authorized.

## 11. Review and change control

Review this approved policy alongside the Validation Strategy when considering a formal document registry or automated governance checks. D-014 records owner approval of the exact source baseline blob and scope. Any substantive revision after approval requires a new owner decision and exact baseline reference.
