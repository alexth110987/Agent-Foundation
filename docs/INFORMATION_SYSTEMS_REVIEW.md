# Memory and Knowledge Architecture — Joint Review

**Review status:** Joint review completed; both exact logical architecture versions formally approved by the owner on 2026-11-11 (D-006 and D-007).  
**Review scope:** Owner-approved logical architecture versions on `main`, identified by D-006 and D-007.  
**Implementation status:** Documentation only. Logical architecture approval does not authorize implementation, technology selection, runtime construction, or operational use.

## 1. Review objective

Assess whether the proposed Memory Management System (MMS) and Knowledge Management System (KMS) have clear, non-overlapping responsibilities and compatible lifecycle, provenance, trust, access, and validation boundaries while preserving the purpose-neutral Stage 1 scope.

## 2. Documents reviewed

- `systems/memory/MEMORY_ARCHITECTURE.md`
- `systems/memory/README.md`
- `systems/knowledge/KNOWLEDGE_ARCHITECTURE.md`
- `systems/knowledge/README.md`
- `docs/ARCHITECTURE.md`
- `docs/COMPONENT_MODEL.md`
- `docs/DECISIONS.md`
- `docs/PROJECT_STATUS.md`
- `AGENT.md`
- `AGENTS.md`

## 3. Overall assessment

**Result: Broadly consistent at the responsibility-boundary level, with cross-system contracts still to be specified.**

The designs make the essential distinction:

- MMS governs explicitly permitted information retained across tasks.
- KMS governs curated source material and derived reference content with provenance, versions, review/trust state, freshness, and retrieval scope.
- Project records remain governed by their repository/version-control process.
- Task context, workflow state, and operational logs remain separately owned.
- Neither memory nor knowledge can modify Core instructions, grant permissions, or create workflow authority.

No fundamental contradiction was identified that requires changing the approved project baseline. The following points should be resolved in a later design pass before implementation is considered.

## 4. Findings and recommendations

### R-01 — Define the knowledge-to-memory handoff

**Observation:** KMS states that retrieved knowledge is not automatically promoted to memory. MMS permits provenance references but does not define a precise relationship to a KMS source/version.

**Recommendation:** If a future approved use case needs a durable memory record based on knowledge, the memory record should store a minimal, authorized summary or reference and retain a stable pointer to the exact KMS source/version where available. The memory write gate must independently apply memory policy, authorization, minimization, and retention. KMS retrieval approval must not imply memory-write approval.

**Disposition:** Open design requirement; no automatic handoff.

### R-02 — Make lifecycle ownership explicit across systems

**Observation:** Both systems describe expiry, supersession, deletion, and audit history. KMS additionally owns source content, derived passages, and indexes; MMS owns durable memory records.

**Recommendation:** Each system should remain authoritative for its own primary records and derived data. A deletion or withdrawal request that affects both systems should be coordinated, but neither system should directly alter the other's authoritative records. Define the handling of copied excerpts, cached results, backups, and audit metadata before implementation. A reference to deleted or withdrawn knowledge must not be represented as an available source.

**Disposition:** Ownership principle is clear; cross-system deletion/reconciliation contract remains open.

### R-03 — Establish a minimal shared provenance reference

**Observation:** Both architectures maintain provenance, but their record models are system-specific.

**Recommendation:** Later interface design should define a minimal interoperable provenance reference (for example, source/system identifier, record or version identifier, origin reference, observed time, and integrity/version reference where available). MMS and KMS may retain richer system-specific metadata. Unknown provenance must remain explicitly unknown.

**Disposition:** Open interface contract; no schema selected.

### R-04 — Separate authorization policy from enforcement points

**Observation:** MMS and KMS each describe an Access Controller, while the wider architecture also defines a Permission Service.

**Recommendation:** Treat the central Permission Service as the authority for policy decisions, with MMS and KMS enforcing the returned, scoped authorization at their own data boundaries. Their local controls may enforce system-specific constraints, but must not independently broaden grants or infer permission from relevance. Deny on unavailable or indeterminate authorization.

**Disposition:** Recommended interpretation consistent with least privilege; detailed interface remains open.

### R-05 — Distinguish source trust from memory confidence

**Observation:** KMS assesses source trust and quality; MMS records confidence or verification state for retained information.

**Recommendation:** Keep these as different attributes. A trusted publisher does not guarantee a particular claim is correct, and a memory record's confidence must not be copied mechanically from a source trust label. Preserve the evidence and reasoning relevant to each assessment.

**Disposition:** Boundary clarification recommended; no common scoring model.

### R-06 — Keep conflict handling at the correct level

**Observation:** KMS addresses conflicts between sources; MMS addresses conflicting retained records.

**Recommendation:** KMS should report source-level disagreement with provenance. MMS should manage conflicts between memory records under memory policy, optionally referring to KMS evidence. Neither system should silently resolve a conflict merely by ranking or recency.

**Disposition:** Responsibilities are compatible; cross-reference behavior remains open.

### R-07 — Define derived-index consistency and withdrawal semantics

**Observation:** KMS requires version-linked index entries and re-indexing, but the precise consistency guarantee after withdrawal, quarantine, or failed index updates is deferred.

**Recommendation:** Before implementation, define whether retrieval is blocked immediately when source status changes, how stale index entries are invalidated, how caches are purged, and what degraded-state response is returned when index reconciliation fails. Authorization and source eligibility must be rechecked at result disclosure, not assumed from index membership.

**Disposition:** Required future acceptance criteria; no technology or consistency model selected.

## 5. Boundary matrix

| Concern | Primary owner | Cross-system rule |
| --- | --- | --- |
| Temporary task data | Task context | Do not persist by default |
| Durable retained records | MMS | May reference KMS evidence; must pass independent memory write controls |
| Source catalogue and source versions | KMS | Owns source identity, review, trust, and version status |
| Extracted passages and search indexes | KMS | Every derived item remains linked to its source version |
| Approved project decisions | Project records | Neither MMS nor KMS silently replaces project authority |
| Workflow definitions and run state | Workflow Management System | Memory/knowledge data cannot alter workflow authority |
| Authorization policy | Permission Service / Core governance | MMS and KMS enforce scoped decisions at their own boundaries |
| Operational audit events | Logging system | Minimize content; define retention and deletion exceptions separately |

## 6. Validation implications

A future validation plan should include cross-system cases in addition to each system's independent tests:

- A KMS retrieval result cannot cause an MMS write without a separate authorized write decision.
- A memory record referencing a KMS version preserves the exact reference and reports when that version is withdrawn or unavailable.
- A source withdrawal or deletion is reconciled with derived passages, indexes, caches, and any explicitly linked memory references under defined policy.
- MMS and KMS cannot read across project/subject boundaries through shared identifiers or semantic retrieval.
- A central permission denial or unavailable authorization service blocks access in both systems.
- Source trust and memory confidence remain distinct in stored and returned metadata.
- Source-level and memory-record conflicts remain visible and traceable.
- Index lag, failed purge, or incomplete deletion is reported as pending/degraded rather than complete.

These are proposed acceptance requirements, not executed tests. No runtime exists to test them.

## 7. Owner disposition

On 2026-11-11, the owner accepted all seven recommendations in principle and authorized preparation of revised architecture documents. The owner subsequently formally approved the exact revised MMS and KMS logical architecture versions recorded in D-006 and D-007. Approval does not authorize implementation or technology choices.

| Recommendation | Owner disposition | Remaining work |
| --- | --- | --- |
| R-01 — Knowledge-to-memory handoff | Accepted in principle | Define interface only after purpose/context approval; independent memory write gate remains mandatory |
| R-02 — Cross-system lifecycle ownership | Accepted in principle | Define reconciliation for copies, caches, replicas, backups, and audit metadata |
| R-03 — Shared provenance reference | Accepted in principle | Define minimal fields and representation; no schema selected |
| R-04 — Central authorization and local enforcement | Accepted in principle | Define Permission Service contract and local enforcement; fail closed |
| R-05 — Trust versus confidence | Accepted in principle | Keep separate attributes, evidence, and ownership; no scoring model |
| R-06 — Conflict boundaries | Accepted in principle | Define references/escalation; no automatic resolution |
| R-07 — Index consistency and withdrawal | Accepted in principle | Define consistency, invalidation, cache purge, and degraded-state criteria |

## 8. Review disposition

| Item | Review outcome |
| --- | --- |
| Information category separation | Consistent at design level |
| Core immutability and authority boundary | Consistent |
| Project/subject isolation | Addressed independently; shared enforcement contract pending |
| Provenance | Addressed independently; interoperable reference pending |
| Retention and deletion | Addressed independently; cross-system reconciliation pending |
| Trust and confidence | Distinct concepts; keep separate |
| Conflict handling | Compatible responsibilities; cross-system references pending |
| Validation | Substantial independent coverage; add cross-system acceptance cases |

**Both logical architectures are Approved at the exact versions recorded in D-006 and D-007.** Approval is limited to the logical design baseline. Implementation, feature testing, security/privacy validation, operational smoke testing, and release approval remain separate and unauthorized.

## 9. Next work

1. Define the remaining MMS/KMS cross-system interface details only after purpose and operating context are approved, including provenance references, owner-local lifecycle reconciliation, withdrawal/deletion handling, and derived-index consistency.
2. Preserve the five open D-012 questions and resolve them only through separate explicit owner decisions; do not infer answers from this review.
3. Keep implementation deferred until purpose, operating context, and any bounded implementation architecture are explicitly approved. D-006 and D-007 already record approval of the exact logical architecture baselines; no further approval request for those same versions is outstanding.

No operational workflow, data store, index, retrieval engine, integration, or domain-specific capability is authorized by this review.
