# Provenance Contract — Proposed Shared Contract

**Status:** Proposed logical contract — pending owner review.  
**Implementation:** Documentation only. No provenance service, registry, event store, schema, or runtime exists.  
**Authority:** Subordinate to `AGENT.md`, approved Core rules, and the approved logical architecture baselines. This contract does not amend D-006, D-007, or D-008 and does not authorize implementation.

## 1. Purpose

Define a purpose-neutral, interoperable basis for recording where information came from, how it was acquired, which actors or components handled it, and how derived information relates to its inputs.

Provenance supports traceability and later evaluation. It does **not** establish that information is true, correct, safe, complete, trustworthy, authorized, or suitable for a particular use.

## 2. Scope and exclusions

This contract applies conceptually to information that may be received, registered, extracted, transformed, summarized, copied, retained, retrieved, corrected, superseded, withdrawn, or deleted across components.

It defines provenance responsibilities and minimum evidence concepts. It does not select a domain, source catalogue, principal taxonomy, metadata schema, serialization, identifier format, API, protocol, storage technology, event platform, cryptographic mechanism, retention period, provider, or runtime.

It does not define:
- authentication or identity proofing;
- authorization policy or permission grants;
- source trust, factual confidence, quality scoring, or validation criteria;
- memory eligibility, retention policy, or knowledge approval;
- a universal requirement that every operation must have complete provenance.

## 3. Governing principles

1. **Traceable origin:** Record an identifiable origin or explicitly mark it unknown or unavailable.
2. **No fabricated lineage:** Missing, ambiguous, or conflicting metadata remains visible; it is never guessed.
3. **Actor separation:** Distinguish the original source/author, the actor submitting or acquiring information, and each component that processes or transforms it.
4. **Transformation traceability:** Derived information links to its immediate inputs and, where available, their upstream lineage.
5. **Version specificity:** Identify the exact source/content version where available; do not imply that a mutable source reference identifies immutable content.
6. **Temporal clarity:** Distinguish source publication/effective time, observation/acquisition time, processing time, and record time where known.
7. **Integrity awareness:** Preserve available integrity references and verification outcomes without claiming that a fingerprint proves truth or authenticity by itself.
8. **Scope isolation:** Provenance references and metadata remain subject to project, tenant, subject, classification, and access boundaries.
9. **Data minimization:** Store references and necessary metadata rather than duplicating source payloads, secrets, or unnecessary personal data.
10. **Lifecycle continuity:** Corrections, supersession, withdrawal, and deletion must not silently rewrite the historical account of what happened.
11. **Separation of concerns:** Provenance is evidence about origin and handling, not permission, trust, confidence, or correctness.
12. **Purpose neutrality:** The required level of provenance and its retention are determined by future approved context-specific policy.

## 4. Logical terms

| Term | Meaning |
| --- | --- |
| Source | An origin from which information is asserted to have been obtained |
| Source actor | Author, publisher, issuer, originating person, system, or process attributed with creating or issuing the source |
| Acquiring actor | Principal or component that received, retrieved, imported, or submitted the information |
| Processing actor | Principal or component responsible for a transformation or handling event |
| Provenance record | A record of origin, acquisition, handling, transformation, and lineage references |
| Artifact | An identifiable information item, source version, extracted passage, memory record, or other data object |
| Lineage | The set of relationships connecting an artifact to its inputs and preceding processing events |
| Transformation | An operation that changes, extracts, filters, normalizes, summarizes, translates, combines, or otherwise derives information |
| Integrity reference | A reference or result used to detect or assess alteration of identified content |
| Unknown provenance | A deliberate state indicating that origin or lineage is missing, unavailable, or not established |

These are logical concepts, not a final data model.

## 5. Minimum provenance information

A future provenance record should capture, where applicable and available:

### 5.1 Artifact identification
- stable artifact or record reference;
- artifact type and applicable version identifier;
- owning system/component and project/tenant/subject scope;
- content reference or location, without requiring payload duplication;
- classification or handling restrictions by reference to the authoritative policy.

### 5.2 Origin and acquisition
- source reference, source type, issuer/namespace, and source version;
- source actor attribution and the authority or evidence basis for that attribution, if known;
- acquisition method or channel;
- acquiring actor/component, separately identified from the source actor;
- acquisition/observation timestamp and relevant source-provided timestamps;
- source location/reference and available integrity evidence;
- explicit unknown, unavailable, or unverified markers for absent fields.

### 5.3 Processing and derivation
- processing event reference and operation category;
- processing actor/component identity reference;
- component or process version where available;
- processing timestamp and relevant execution context reference;
- input artifact references and exact versions where available;
- output artifact reference and version;
- relationship type, such as extracted-from, summarized-from, translated-from, normalized-from, combined-from, or copied-from;
- declared parameters or transformation description only to the extent necessary and safe;
- processing outcome, including partial, failed, or uncertain outcomes.

### 5.4 Integrity and lineage state
- content fingerprint or other integrity reference where applicable;
- integrity check method, time, and result, without overstating its meaning;
- lineage completeness state;
- unresolved gaps, conflicting claims, and unavailable upstream references;
- correction, supersession, withdrawal, or deletion relationship references.

No fixed field set or encoding is selected.

## 6. Actor attribution and identity boundary

Provenance must not collapse distinct roles into a single “author” or “owner” field.

At minimum, the design should distinguish:
- the actor or authority attributed with originating the source;
- the principal or component that acquired or submitted it;
- the component or process that transformed it;
- the principal that requested or authorized a relevant operation, where this is appropriate to record.

A provenance claim about an actor is an attribution, not proof of identity. Where identity assurance matters, the relevant identity assertion and its issuer/validation context must be referenced through the separately governed Identity Contract. Raw credentials or authentication secrets must never be copied into provenance.

A component version or process identifier describes the processing context; it does not independently authenticate that component.

## 7. Transformation and lineage rules

1. A transformation that creates a materially distinct artifact should produce a distinct artifact reference or version.
2. The derived artifact should link to its direct input artifacts and versions where known.
3. Multiple inputs must all be represented where they materially contribute to the output; do not select a single convenient parent for a combined result.
4. A transformation record should distinguish automated processing, human modification, and unknown processing where that distinction is available.
5. Partial extraction, lossy conversion, filtering, summarization, and inferred content must not be represented as verbatim source content.
6. A transformation must not upgrade source authority, trust, factual confidence, or authorization.
7. If lineage cannot be established, record the gap and its scope. Do not fabricate a complete chain.
8. A copied or derived artifact remains distinct from its source and is subject to its own access, lifecycle, and retention rules.

A chain of provenance records is evidence of recorded handling, not a guarantee that every unrecorded event is absent.

## 8. Relationship to identity, authorization, KMS, and MMS

### 8.1 Identity
Identity governs how actors are referenced and, where separately implemented and approved, authenticated. Provenance records actor attribution and references relevant identity evidence; it does not authenticate actors.

### 8.2 Authorization
Authorization determines whether an action is permitted. Where appropriate, a provenance event may reference the relevant authorization request/decision, policy context, or approval record. Such a reference records context only: it is not a permit, does not prove enforcement, and does not prove that the action succeeded.

### 8.3 Knowledge Management System
KMS owns its source records, source versions, extracted passages, ingestion/retrieval provenance, review and trust state, freshness, and knowledge lifecycle. KMS artifacts should expose interoperable provenance references without transferring ownership of KMS records to another system.

Source trust and review status remain KMS responsibilities. Provenance completeness does not itself establish source trust or factual correctness.

### 8.4 Memory Management System
MMS owns provenance references and transformation lineage for its durable memory records and MMS-managed derived data. A memory record derived from knowledge or task information should retain the minimal source/version references needed to trace its basis, subject to policy and minimization.

Memory confidence or verification state remains separate from source trust and provenance completeness. KMS retrieval does not authorize an MMS write; MMS must independently enforce its own purpose, minimization, authorization, scope, and retention requirements.

### 8.5 Other components
Other components may reference provenance records according to their responsibilities and approved interfaces. No component may treat provenance metadata as instruction authority or permission.

## 9. Trust, confidence, quality, and validation separation

These concepts must remain separately represented and separately governed:

| Concept | What it describes | What it does not establish |
| --- | --- | --- |
| Provenance | Recorded origin and handling lineage | Truth, trust, permission, or correctness |
| Identity assurance | Evidence supporting an actor identity | Permission for an action |
| Authorization | Decision about a bounded requested action | Execution or success |
| Source trust | Assessment of a source under KMS policy | Factual correctness of every claim |
| Memory confidence / verification | MMS assessment of a retained claim | Source authority or provenance completeness |
| Quality | Fitness against defined quality criteria | Authorization or factual truth in every context |
| Validation | Result against explicit acceptance criteria | General trust or approval beyond those criteria |

Do not derive one of these states mechanically from another. A high-confidence memory may have incomplete provenance; a well-documented lineage may lead to an unreliable source.

## 10. Lifecycle and change semantics

The design must account for provenance continuity through:
- initial registration and acquisition;
- additional metadata discovery;
- extraction and transformation;
- correction of an attribution or metadata field;
- source version changes and supersession;
- source withdrawal, quarantine, or unavailability;
- artifact restriction or access-scope change;
- retention expiry and deletion;
- reconciliation across copies, derivatives, indexes, caches, replicas, and backups.

Corrections should be represented as traceable amendments or successor records, not silent rewriting of previously recorded events. The original claim and correction history should remain distinguishable where retention policy permits.

A source update does not automatically change the identity of previously acquired content. A derived artifact must not be silently relabelled as having been produced from a newer source version.

Withdrawal or deletion of a source should propagate an affected/unavailable status to linked derivatives where applicable. Each owning system remains responsible for its own copies and lifecycle obligations. Provenance metadata retention after source deletion requires a separate, purpose- and obligation-specific policy; this contract selects no universal rule.

## 11. Completeness, conflicts, and uncertainty

A future implementation should be able to express at least:
- complete to the declared level;
- partial, with known gaps;
- unknown or unavailable;
- conflicting attribution or lineage claims;
- integrity check not performed;
- integrity check failed or inconclusive;
- upstream source/version no longer accessible;
- processing outcome uncertain.

Conflicting provenance claims must be preserved and surfaced for reconciliation. Do not silently choose a source actor, timestamp, version, or transformation history based only on recency or convenience.

Completeness is always relative to a declared requirement or policy. The contract does not define one universal completeness threshold.

## 12. Access, privacy, and security

- Provenance metadata is not inherently public; access is governed by the same applicable project, tenant, subject, classification, and purpose restrictions as the related information.
- Enforce access restrictions before disclosing source locations, actor identifiers, lineage, or integrity references.
- Prefer opaque references and minimal metadata over copied payloads.
- Do not record passwords, API keys, access tokens, private keys, recovery codes, raw authentication evidence, or other secrets.
- Avoid unnecessary personal data about authors, submitters, reviewers, or operators.
- Do not expose restricted upstream sources through a derived artifact's provenance link.
- Treat externally supplied provenance metadata as untrusted until evaluated under the relevant identity, integrity, and source-governance processes.
- Protect provenance records against unauthorized alteration and preserve sufficient history under a separately approved integrity and retention design.
- Cross-project or cross-tenant references must not create an access path around the owning system's controls.

## 13. Failure handling

| Failure | Required behavior |
| --- | --- |
| Origin metadata missing | Mark unknown or incomplete; do not invent source details |
| Actor attribution cannot be validated | Preserve it as an unverified attribution; do not present it as authenticated identity |
| Source version ambiguous | Record ambiguity and avoid claiming exact-version lineage |
| Transformation record unavailable | Mark the lineage gap; do not claim a complete chain |
| Integrity reference absent | Mark not available/not checked; do not imply integrity verification |
| Integrity check fails | Mark affected artifact/lineage state and escalate under the owning system's policy |
| Conflicting provenance claims | Preserve both claims and request reconciliation where material |
| Upstream source withdrawn/deleted/unavailable | Mark affected references and let each owning system apply its lifecycle policy |
| Provenance service/reference unavailable | Report traceability as unavailable; do not fabricate a fallback record |
| Provenance recording fails during processing | Record the processing outcome as incomplete/uncertain; apply the operation-specific policy |
| Mandatory provenance precondition not met | Block or quarantine the affected operation/artifact under the applicable approved policy |
| Provenance access denied | Do not disclose metadata or infer permission from possession of a reference |

Fail-closed behavior applies when a separately approved policy makes provenance or a particular evidence level mandatory for the operation. This contract does not categorically block every use of information with incomplete provenance; future purpose- and risk-specific policy must define when gaps are tolerable, require warning, or require blocking.

## 14. Validation requirements

Before implementation authorization, define acceptance criteria and isolated tests for:
- stable artifact and source/version references;
- unknown, partial, conflicting, and unavailable provenance;
- distinction between source actor, acquiring actor, processing actor, and requester;
- unverified attribution versus authenticated identity references;
- direct and multi-parent transformation lineage;
- extraction, summarization, translation, normalization, copying, and lossy transformation labels;
- exact source-version linkage and mutable-source changes;
- integrity metadata semantics and failed/inconclusive checks;
- correction, supersession, withdrawal, deletion, and derivative reconciliation;
- cross-project, tenant, subject, and classification isolation;
- authorization references that cannot be reused as permits;
- trust, confidence, quality, and validation state separation;
- privacy minimization and secret leakage prevention;
- interrupted processing and provenance-recording failure;
- access-control enforcement for source locations and actor metadata.

Contract tests, feature tests, privacy/security validation, operational smoke tests, compatibility tests, and release approval are separate. A pass in one category does not imply another passed.

## 15. Deferred decisions

The following remain deliberately unresolved:
- owner approval of this contract;
- required provenance level by artifact class, operation, and risk;
- final actor/principal categories and identity-reference rules;
- interoperable identifier, schema, and versioning model;
- event, append-only, correction, and integrity mechanisms;
- source/version identity and content-fingerprint standards;
- timestamp precision, clock trust, and ordering semantics;
- lineage retention, deletion, and legal/contractual requirements;
- treatment of derived data, caches, indexes, backups, and cross-system copies;
- access model for sensitive provenance metadata;
- interface/API, serialization, protocol, service boundaries, storage, runtime, and deployment;
- operational ownership, monitoring, recovery, and acceptance thresholds.

Resolve these only after the agent's purpose, users, information classes, operating context, and applicable obligations are defined.

## 16. Related records

- [Project Status](PROJECT_STATUS.md)
- [Component Model](COMPONENT_MODEL.md)
- [Knowledge Management System](../systems/knowledge/README.md)
- [Memory Management System](../systems/memory/README.md)
- [Permissions and Authorization](../systems/permissions/README.md)
- [Identity Contract](../systems/permissions/IDENTITY_CONTRACT.md)
- [Authorization Contract](../systems/permissions/AUTHORIZATION_CONTRACT.md)
