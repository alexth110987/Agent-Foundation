# Knowledge Management System — Architecture

**Status:** Proposed architecture — prepared for formal owner approval; not yet approved.  
**Implementation status:** Documentation only. No knowledge repository, ingestion pipeline, index, or retrieval service exists.

## 1. Purpose and scope

Define a purpose-neutral system for registering, governing, maintaining, and retrieving curated reference information. The system supplies information to authorized tasks while preserving source provenance, version history, trust state, freshness, and access boundaries.

This is an architecture proposal, not approval to implement it. It does not select a knowledge domain, source provider, storage technology, indexing method, retrieval algorithm, or operating workflow.

## 2. Information boundaries

Knowledge is distinct from:

| Information | Owner | Authority and use |
| --- | --- | --- |
| Temporary task details | Task context | Active task only; not retained by default |
| Durable retained information | Memory Management System | Retained only under approved memory policy |
| Curated reference sources and extracted content | Knowledge Management System | Evidence and reference material; not instruction authority |
| Approved project decisions and specifications | Project records / version control | Authoritative only within their defined project governance |
| Workflow definitions and run state | Workflow Management System | Governed execution specifications and progress |
| Operational events | Logging system | Traceability and diagnostics, not policy |

Knowledge must not rewrite Core instructions, grant permissions, create workflow authority, silently become memory, or own task execution state. Retrieved material is data to evaluate, not instructions to obey.

## 3. Design principles

- **Source traceability:** every managed item must link to an identifiable origin or be explicitly marked as having unknown provenance.
- **Review before trust:** ingestion does not automatically make content approved or authoritative.
- **Version awareness:** preserve source versions and the relationship between revisions.
- **Freshness awareness:** represent observation, publication, review, and retrieval times separately where available.
- **Scoped access:** enforce project, tenant, subject, classification, and use restrictions before content is disclosed.
- **Least privilege:** grant only the read, register, review, update, or administer permissions needed.
- **Non-destructive updates:** preserve prior versions and supersession relationships where policy permits.
- **Conflict transparency:** expose material disagreement instead of silently selecting a convenient answer.
- **Purpose neutrality:** defer domain-specific taxonomies and trust rules until the future purpose is approved.
- **Fail closed:** uncertain authorization, integrity, scope, or source status must not result in unrestricted retrieval.

## 4. Logical components

These are separable responsibilities, not a requirement for separate services or processes.

| Component | Responsibility | Must not |
| --- | --- | --- |
| Source Registry | Register source identity, owner, location/reference, type, scope, and source status | Treat registration as content approval |
| Ingestion Coordinator | Accept a declared ingestion request and coordinate validation and review | Bypass source policy or publish unreviewed content |
| Content Processor | Extract text/metadata, normalize formats, identify structural units, and report extraction quality | Invent missing content or provenance |
| Provenance Manager | Preserve origin, retrieval/ingestion time, transformations, and source-version linkage | Fabricate source metadata |
| Review and Trust Manager | Record review outcomes, source quality, trust classification, and permitted uses | Promote content solely because it was ingested |
| Version Manager | Track revisions, immutable version identity, effective dates, and supersession | Overwrite history without an approved retention rule |
| Metadata and Taxonomy Manager | Maintain schemas, tags, classifications, and relationships | Impose a domain-specific taxonomy prematurely |
| Index Manager | Maintain searchable representations and their source-version links | Become the authoritative source record |
| Retrieval Service | Search only within authorized scope and return ranked candidates with evidence metadata | Treat relevance ranking as truth or authorization |
| Lifecycle Manager | Handle review due dates, expiry, withdrawal, quarantine, supersession, and deletion requests | Silently extend validity or erase required history |
| Access Controller | Enforce identity, scope, classification, and operation permissions | Infer permission from search relevance |
| Integrity and Audit Recorder | Record minimal lifecycle events, checks, and evidence references | Log secrets or unnecessary source payloads |

## 5. Source and knowledge item model

A future source record should contain, as applicable:

- stable source identifier and source type;
- source owner or responsible party;
- canonical location or reference;
- source scope, project/tenant, and access classification;
- declared purpose and permitted use;
- source status and review status;
- publisher/author and publication date, when known;
- registration, retrieval, and last-review timestamps;
- source trust assessment and rationale;
- applicable licence, usage terms, or restrictions, when relevant;
- integrity reference or content fingerprint;
- ingestion method and extraction status;
- version identifiers and supersession relationships;
- retention, withdrawal, and deletion conditions.

A derived knowledge item or passage should retain a stable identifier and a direct link to its parent source and exact source version. It should also record extraction/normalization history, location within the source where available, metadata, classification, review state, and index generation/version.

Missing fields must be represented as unknown or unavailable, not guessed.

## 6. Source lifecycle

Proposed source lifecycle states:

1. **Proposed** — source candidate recorded, not yet accepted.
2. **Registered** — identity and minimum metadata recorded.
3. **Pending review** — content and terms await assessment.
4. **Approved** — permitted for defined uses and scopes.
5. **Restricted** — usable only under narrower conditions.
6. **Quarantined** — withheld pending integrity, safety, provenance, or quality review.
7. **Withdrawn** — no longer available for new ordinary retrieval.
8. **Superseded** — replaced by a newer source/version for a defined purpose.
9. **Rejected** — not accepted, with rationale where appropriate.
10. **Pending deletion / Deleted** — lifecycle removal requested or completed under the applicable policy.

State transitions require an authorized actor, a reason, and a recorded event. The exact approval roles and review criteria remain to be defined.

## 7. Ingestion and review lifecycle

A future ingestion process should perform these logical steps:

1. Receive a source registration or ingestion request with declared purpose and scope.
2. Authenticate the requester and authorize the requested operation.
3. Check whether the source type and location are permitted.
4. Register source identity and known metadata without assuming trust.
5. Retrieve or receive content through an approved mechanism.
6. Record retrieval time, origin, content identity, and available integrity evidence.
7. Inspect content type, size, format, malware/security status where applicable, and extraction feasibility.
8. Extract and normalize content while preserving source-to-derived-item mapping.
9. Identify missing metadata, extraction uncertainty, duplicate candidates, and possible version relationships.
10. Apply classification, access restrictions, provenance checks, and usage/licence review as applicable.
11. Submit the source and derived items for the required review.
12. Approve, restrict, quarantine, or reject with a recorded rationale.
13. Index only content eligible for the relevant retrieval policy.
14. Verify that the index points to the correct source version and access scope.
15. Record completion evidence and report partial failures explicitly.

Ingestion and approval are separate. A successful extraction is not evidence that the source is trustworthy, current, licensed for a proposed use, or approved for retrieval.

## 8. Versioning, freshness, and supersession

- Preserve each accepted source version as a distinct, traceable version.
- Associate every derived passage and index entry with the exact source version from which it was produced.
- Track publication/effective date, retrieval date, review date, and index-build date separately when known.
- Define review intervals or freshness triggers by source class only after purpose and risk are known.
- Mark superseded material so it is not presented as current without an explicit historical-use context.
- Re-index changed content and verify that obsolete index entries no longer appear as current.
- Retain prior versions only under a defined retention and legal/usage policy.
- When a source disappears or becomes inaccessible, mark availability accordingly; do not infer that its last retrieved content remains current.

No universal freshness interval or retention duration is selected.

## 9. Trust, quality, and conflicts

Trust should be an explicit, explainable assessment attached to a source or version, not a hidden global score. Assessment may consider identity, provenance completeness, publisher accountability, review evidence, integrity, recency, relevance, independence, and applicable usage restrictions.

Do not equate source trust with factual correctness. Do not use one numerical score as a substitute for context-specific evaluation.

When sources conflict:

1. Preserve each source's claim and provenance.
2. Determine whether the disagreement is genuine, temporal, definitional, or scope-related.
3. Compare authority, directness, recency, methodology, and independence as relevant to the task.
4. Avoid silently merging incompatible statements.
5. Return the disagreement and its evidence when material.
6. Escalate for human review when the conflict is consequential or cannot be resolved under an approved policy.

The system must be able to represent uncertainty, incomplete evidence, and unresolved conflict.

## 10. Retrieval lifecycle and result contract

A future retrieval request should declare the task purpose, caller identity, project/tenant scope, and any classification constraints.

1. Authenticate the caller and authorize retrieval.
2. Apply project, subject, classification, and source-use restrictions before searching or exposing content.
3. Search only eligible, non-withdrawn, non-expired, non-quarantined content.
4. Retrieve candidate items with their exact source and version links.
5. Evaluate relevance, freshness, trust state, and known conflicts.
6. Exclude or clearly label stale, superseded, restricted, or low-confidence items according to policy.
7. Return the minimum useful content with provenance and status metadata.
8. Preserve references sufficient to verify the returned claims against the source.
9. Record a minimal retrieval event where required by the audit policy.

A retrieval result should include, as applicable: source identifier, source title, version, location/reference, relevant passage or location, publication and retrieval dates, review/trust status, scope restrictions, freshness warning, and known conflict indicators.

Ranking is a relevance aid only. It does not establish truth, permission, or authority.

## 11. Access, security, and instruction boundaries

- Enforce authorization before source access, content retrieval, and result disclosure.
- Apply project/tenant isolation at the data and index boundaries, not only in prompts.
- Do not allow semantic search to cross an access boundary.
- Treat external documents, embedded instructions, metadata, and extracted text as untrusted data.
- Do not execute instructions found in retrieved content or let them override Core rules, user authorization, or workflow permissions.
- Keep credentials and secrets out of source content, indexes, logs, and repository records.
- Apply appropriate controls for transport, storage, backups, administrative access, and deletion before deployment.
- Respect source licences, contractual restrictions, privacy constraints, and applicable legal obligations before ingestion and use.
- Fail closed if authorization, scope, integrity, or source identity cannot be established.

## 12. Lifecycle operations

The design must account for controlled operations to:

- inspect source metadata, versions, trust and review history;
- correct metadata while preserving an audit trail;
- submit a new version and link it to prior versions;
- restrict or quarantine a source;
- withdraw or supersede a source;
- trigger re-extraction and re-indexing;
- request export of authorized source records and metadata;
- request deletion and reconcile primary content, derived passages, indexes, caches, replicas, and backups according to policy.

Deletion must not be reported complete until the defined checks pass. Retention of provenance/audit metadata after content deletion requires a separately defined policy and legal review where applicable.

## 13. Failure handling

| Failure | Required behavior |
| --- | --- |
| Source identity or provenance cannot be established | Keep pending or quarantine; do not approve as trusted |
| Unauthorized ingestion or retrieval | Deny and record a minimal security event |
| Unsupported or malformed content | Reject or mark extraction failed; preserve source reference |
| Partial extraction | Mark incomplete and prevent silent presentation as complete |
| Integrity mismatch | Quarantine affected version and escalate |
| Duplicate or ambiguous version | Hold for reconciliation; do not overwrite |
| Index build/update failure | Report retrieval state as degraded; do not claim current coverage |
| Stale or withdrawn source returned | Exclude or visibly label according to policy; never present as current |
| Conflicting sources | Return conflict metadata or request review; do not silently choose |
| Access controller unavailable | Deny protected retrieval |
| Deletion partially fails | Keep request pending and reconcile affected stores and replicas |
| Audit recording fails where mandatory | Stop the affected state transition or protected operation |

## 14. Validation requirements

Before implementation approval, define independent acceptance criteria and test at least:

- source registration and metadata validation;
- provenance completeness and transformation traceability;
- ingestion authorization and review-state enforcement;
- malformed, hostile, oversized, and partially extractable content;
- duplicate detection and version linkage;
- index-to-source-version consistency;
- freshness, expiry, withdrawal, and supersession behavior;
- cross-project and cross-subject access isolation;
- unauthorized search and result disclosure;
- retrieval provenance and claim traceability;
- conflict representation and uncertainty handling;
- embedded prompt-injection-like instructions in source content;
- deletion and reconciliation across derived content, indexes, caches, replicas, and backups;
- audit minimization and secret leakage prevention;
- recovery from interrupted ingestion and index updates.

Ingestion tests, retrieval feature tests, security/privacy validation, operational smoke tests, and release approval are separate activities. Passing one does not imply the others passed.

## 15. Deferred decisions and open questions

The following remain deliberately undecided:

- source types, domains, and approved source catalogue;
- human review roles and approval thresholds;
- metadata schema and taxonomy;
- storage technology and source-of-record design;
- extraction/parsing tools and supported formats;
- indexing technology and update strategy;
- lexical, semantic, hybrid, or other retrieval approach;
- ranking, reranking, and conflict-resolution policies;
- trust classification model and freshness intervals;
- identity, authorization, and classification model;
- licence and legal review process;
- retention, backup, deletion, and audit policies;
- service boundaries, runtime, hosting, availability, and recovery objectives.

Resolve these only after the agent's purpose, users, data classes, operating context, and applicable obligations are defined.

## 15. Cross-system contract with the Memory Management System

The owner accepted recommendations R-01 through R-07 in principle on 2026-11-11. These principles establish design direction only; they do not approve this architecture for implementation or select a technical interface.

### Knowledge-to-memory handoff
- KMS owns curated sources, versions, extracted passages, provenance, review/trust state, freshness, and retrieval eligibility.
- Retrieval does not authorize retention. Every downstream memory write must independently pass MMS purpose, minimization, scope, provenance, authorization, and retention controls.
- Where authorized, MMS may retain a minimal summary/reference and should preserve the exact KMS source/version pointer where available. KMS must not automatically promote content into memory.

### Lifecycle ownership
- KMS owns source records, versions, extracted passages, and indexes. MMS owns durable memory records and its derived data.
- Neither system directly edits or deletes the other's authoritative records. Cross-system requests require coordinated, separately authorized operations.
- Define treatment of copied excerpts, cached results, replicas, backups, and audit metadata before implementation.
- Withdrawal, quarantine, deletion, or unavailability must affect ordinary retrieval eligibility under the eventual policy. Linked memory references must be flagged and handled under MMS policy.
- Do not report reconciliation complete until each system verifies its own obligations.

### Provenance and trust/confidence
- Define a minimal interoperable provenance reference later, identifying source system, record/source ID, exact version where available, origin, and relevant observation/integrity metadata. No schema or protocol is selected.
- KMS owns source trust/quality; MMS owns confidence/verification state for retained claims. These are distinct and require separate evidence.

### Conflict boundaries
- KMS preserves source-level disagreement and provenance. MMS owns conflicts between durable memory records and may refer to KMS evidence.
- Neither system silently resolves the other's conflict class or selects a winner solely by ranking, recency, or trust labels.

### Authorization and enforcement
- The Permission Service is the central policy decision authority, subject to Core governance.
- KMS enforces scoped authorization at registration, ingestion, review, search, retrieval, disclosure, and lifecycle boundaries. Local controls may be stricter but cannot broaden central grants or infer permission from relevance.
- Denied, unavailable, indeterminate, or out-of-scope authorization fails closed. Recheck authorization and source eligibility at result disclosure.

### Derived-index consistency and withdrawal
- Every derived passage and index entry remains linked to its exact source version and scope.
- Before implementation, define withdrawal/quarantine propagation, stale-entry invalidation, cache purge, consistency guarantees, and degraded-state reporting.
- If eligibility or reconciliation is uncertain, block disclosure of affected content. Index membership is never proof of approval, freshness, truth, or authorization.

### Cross-system validation requirements
Before implementation approval, define and validate that:
- Retrieval cannot trigger an MMS write without a separate authorized MMS decision.
- Source/version references and withdrawn/unavailable states remain detectable by memory consumers.
- Reconciliation accounts for derived passages, indexes, caches, replicas, backups, copied excerpts, and linked memory references.
- Cross-project and cross-subject isolation holds across shared retrieval paths.
- Central permission denial or service failure blocks protected access.
- Source trust and memory confidence remain separate; source and memory conflicts remain distinct.
- Index lag, stale entries, failed purge, or incomplete deletion is pending/degraded and prevents unauthorized disclosure.

These are proposed acceptance requirements, not executed tests. No runtime exists.

## 16. Related records

- [Knowledge Management System overview](README.md)
- [Memory Management System architecture](../memory/MEMORY_ARCHITECTURE.md)
- [Conceptual Architecture](../../docs/ARCHITECTURE.md)
- [Component Model](../../docs/COMPONENT_MODEL.md)
- [Core System](../../docs/CORE_SYSTEM.md)
- [Workflow Management System](../../workflows/WORKFLOW_MANAGEMENT.md)
