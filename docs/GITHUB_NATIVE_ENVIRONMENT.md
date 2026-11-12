# GitHub-Native Project Environment

**Status:** Proposed project direction and logical environment design — pending owner review.
**Implementation status:** Documentation only. No privacy scanner, upload gate, timestamp service, GitHub Actions workflow, repository ruleset, backup process, or automation is implemented or authorized by this proposal.

## 1. Purpose

Establish GitHub as the central, well-governed working environment for the Agent Foundation project. The objective is to keep project information in its correct location, preserve decisions and history, prevent avoidable data loss or information mixing, and make material project activity traceable.

This proposal concerns the project workspace and its records. It does **not** select GitHub as a permanent runtime for a future AI agent, define an operational purpose, or authorize an executable agent.

## 2. Environment principles

1. The repository on `main` remains the canonical source for approved project records.
2. Keep architecture, decisions, workflows, validation evidence, logs, and project status in their designated locations.
3. Use Git history and exact commit/blob references for change traceability; do not treat a commit timestamp alone as proof of approval or truth.
4. Use automated checks as safeguards, not as substitutes for owner approval or human review.
5. Keep secrets and unnecessary personal data out of the repository, commit history, issues, pull requests, workflow logs, artifacts, and generated reports.
6. Fail closed when a required privacy/integrity check is unavailable, inconclusive, or reports a credible finding.
7. Keep backups and recovery separate from ordinary Git history; a remote repository is not, by itself, a complete backup strategy.
8. Keep this design purpose-neutral and limited to project administration.

## 3. Logical environment components

| Component | Responsibility | Proposed location / mechanism |
| --- | --- | --- |
| Canonical project store | Approved documents, decisions, architecture and records | GitHub repository, `main` |
| Information structure | Stable locations and ownership for each record type | Existing repository directories and documented file map |
| Change control | Review, approval, atomic change history and traceability | Pull requests, commits, decision records and repository rules |
| Privacy and secret screening | Inspect candidate content before it is uploaded; provide a second line of detection | Pre-upload gate plus GitHub Actions checks |
| Timestamp and event record | Record material project events with source and time semantics | UTC event metadata linked to commit SHA and workflow/event IDs |
| Quality checks | Validate links, required metadata, structure and policy constraints | GitHub Actions, once separately approved and implemented |
| Recovery | Restore from independent, controlled backups and verified Git history | Backup/recovery plan to be designed; no provider selected |

These are logical responsibilities, not implemented components or a selected technology stack.

## 4. Privacy and secret prevention gate

### 4.1 Protected data classes

The future gate must detect and prevent accidental publication of, at minimum:

- Direct identifiers such as personal names in sensitive contexts, home addresses, personal email addresses, phone numbers, and government or account identifiers.
- Special-category or otherwise sensitive personal data where applicable.
- Authentication material and secrets, including passwords, API keys, access tokens, private keys, recovery codes, session secrets, and credentials.
- Sensitive personal or operational payloads embedded in documents, configuration, source files, issue/PR text, commit messages, logs, generated reports, or supported attachments.

The exact data-classification policy and jurisdiction-specific obligations remain to be defined for the project's context.

### 4.2 Required control sequence

1. Inspect candidate files, diffs, metadata, and supported attachment contents **before any content is pushed or uploaded to GitHub**.
2. If the scanner detects a credible finding, cannot inspect a relevant file type, or is unavailable, block the upload by default.
3. Do not print or persist the matched personal data or secret in scanner output. Report only the file, rule/category, location where safe, and a non-reversible finding reference.
4. Remove the data at source and rescan. Do not merely redact the alert while leaving the sensitive value in the candidate content.
5. Run a separate GitHub Actions scan on pull requests and proposed changes as defense in depth, with required checks before merge.
6. Re-scan relevant repository content periodically and after scanner/rule updates; treat findings in already-published history as an incident requiring controlled remediation, not as a normal file edit.
7. Keep scanner versions, rule-set versions, scan scope, result, timestamp, and commit reference as evidence without copying sensitive values into logs.

No exception may authorize committing passwords, credentials, private keys, tokens, or other authentication secrets. Personal-data exceptions, if any are ever considered, require a separately approved legal/privacy basis and must not be inferred by the scanner or workflow.

### 4.3 Critical GitHub timing limitation

A GitHub Actions workflow normally starts only **after** a commit or pull-request content has reached GitHub. It therefore cannot guarantee that personal data never enters a public repository or public branch.

If the requirement is that prohibited data must never be uploaded to GitHub, the primary blocking gate must run in a trusted pre-upload path before the GitHub write occurs. Local Git hooks alone are bypassable and cannot be treated as an enforced boundary. The future design must therefore define and enforce an approved write path and prevent unscanned direct writes; repository rules and Actions checks are additional controls, not a substitute for pre-upload screening.

The current repository is public and its `main` branch currently has no enabled branch protection or required status checks. This proposal does not change repository visibility, permissions, or rulesets.

### 4.4 Detection limitations and failure behavior

No pattern scanner can prove that all personal data has been found. False negatives, obfuscated values, images, archives, unsupported formats, and contextual identifiers are possible. The future acceptance criteria must define supported file types, extraction/OCR coverage if needed, test corpora, false-positive handling, bypass resistance, and independent privacy validation.

A clean scan means only that no finding was detected within the declared scope and scanner version. It is not a guarantee that the content contains no personal data.

GitHub account attribution (for example, usernames and commit author metadata) may itself identify a person. It must be treated as a separately declared metadata class and addressed explicitly in the privacy policy; do not silently assume it is exempt or rewrite historical commits as part of this proposal.

## 5. Timestamp and event-recording system

### 5.1 Canonical representation

- Store timestamps in UTC using an explicit ISO 8601 / RFC 3339 offset, normally `YYYY-MM-DDTHH:MM:SSZ`; include fractional seconds when the source provides them and they are material.
- Convert to local time only for display, retaining the canonical UTC value.
- Record the time source and its trust level. Do not fabricate missing timestamps or imply precision the source does not provide.
- Bind every material record to stable references such as document ID/path, exact version or Git blob/commit SHA, event ID, and relevant workflow run ID.

### 5.2 Distinct time semantics

Preserve the distinctions already used by the Provenance and Cross-System Interface designs:

| Time field | Meaning |
| --- | --- |
| Event time | When the activity is asserted to have occurred |
| Effective time | When the resulting state is intended to apply |
| Observed time | When a component observed or received the event |
| Recorded time | When the event was durably recorded |
| Verification time | When a check was performed and its result established |

Not every record requires every field. Unavailable, estimated, conflicting, and untrusted values must be explicitly identified.

### 5.3 Timestamp authority

- Client-supplied Git author/committer dates are metadata claims and can be set or altered by the client; they are not, by themselves, authoritative proof of when GitHub received or processed an event.
- Prefer service-generated GitHub event or Actions run metadata for the recorded/observed time of repository automation, and preserve the associated run/event ID and commit SHA.
- Record owner approval time from the decision event and link it to the exact approved artifact/version; a later commit timestamp does not create or replace approval.
- Where a reliable service-generated timestamp is unavailable, record the source and mark the time as unverified or unknown.

### 5.4 Minimum event record

A future event record should contain, as applicable:

- Stable event ID and event type.
- Event, observed, recorded, and verification timestamps with source.
- Actor/component reference minimized to what is necessary.
- Target document/artifact path and exact version/blob/commit SHA.
- Related decision, review, validation, or workflow-run reference.
- Outcome (for example, proposed, approved, blocked, failed, or verified) and limitations.
- Integrity/evidence reference and schema/version.

Do not place personal data, credentials, full document payloads, or unnecessary prompt/conversation content in the event record. Timestamp and actor metadata can themselves be personal data and require minimization, access, and retention rules.

## 6. Organization, traceability, and recovery

- Preserve the existing separation between `core/`, `workflows/`, `systems/`, `tools/`, and `docs/`.
- Keep approved decisions in the decision register and link them to exact approved blobs; proposals and discussion do not become approval.
- Keep work items, open questions, blockers, and completion evidence separately identifiable.
- Use pull-request review and atomic commits for material changes once repository controls are approved.
- Define an independent backup schedule, backup scope, access restrictions, retention, restore tests, and recovery ownership before relying on GitHub as the sole working store.
- Do not add private user data or secrets to issues, pull requests, Actions logs, caches, artifacts, or backups merely because they are outside the main source tree.

## 7. Required future validation

Before any implementation or operational use, define and separately validate:

- Pre-upload blocking behavior and bypass resistance across every approved write path.
- Detection coverage for personal data and secrets across supported file types, metadata, diffs, attachments, and generated content.
- Fail-closed behavior when scanning is unavailable, unsupported, or inconclusive.
- False-positive review without exposing the suspected value.
- GitHub Actions defense-in-depth and required-check behavior.
- Timestamp source, UTC normalization, precision, clock trust, missing/conflicting values, and event ordering.
- Exact linkage from events to commit/blob, decision, workflow run, and validation evidence.
- Backup integrity, restore procedures, and recovery evidence.
- Privacy, access, retention, deletion, and incident-remediation behavior.

Smoke tests, feature tests, security/privacy validation, backup/restore validation, and release approval remain separate under the approved Validation Strategy.

## 8. Open decisions

The following remain unresolved and must not be silently selected:

- Whether the public repository remains public or whether visibility changes are needed.
- The exact approved write path and enforcement mechanism that prevents unscanned direct uploads.
- Scanner technology, supported file types, detection rules, OCR/archive coverage, update policy, and independent validation.
- Treatment and minimization of GitHub account attribution metadata.
- Privacy/legal data classification, applicable jurisdiction, retention, deletion, and incident response.
- Exact event schema, timestamp precision, trusted time sources, and retention.
- Repository branch/ruleset protections, reviewer requirements, and required status checks.
- Backup provider/location, schedule, retention, access model, and restore objectives.
- GitHub Actions workflow inventory, permissions, and execution limits.

## 9. Status and authorization boundary

This document records a proposed GitHub-native project-environment direction. It does not approve the detailed architecture, change repository visibility or permissions, enable branch protection, add workflows, install scanners, create a timestamp service, configure backups, or authorize implementation or production use.

The project remains Stage 1. The operational purpose and future agent runtime remain undefined. All implementation, integration, test execution, validation execution, production activity, and release gates remain separate and unchanged.
