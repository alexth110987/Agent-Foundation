# Development Guide

## Working model
Use the ChatGPT Project for planning and review. Use this repository for approved, durable project records. There is no assumed automatic synchronization.

## Before a change
1. Read `AGENT.md`, `docs/PROJECT_STATUS.md`, and relevant design or decision records.
2. Identify the exact files and scope affected.
3. Check current repository state rather than relying on memory or old conversation context.
4. Separate proposals from approved changes.
5. Identify permission, privacy, compatibility, and safety implications.

## Making a change
1. Keep the change focused and internally consistent.
2. Update related documentation and status records.
3. Do not add secrets or private configuration.
4. Do not introduce dependencies or executable functionality without an approved need.
5. Use clear commit messages describing the change.

## Reviewing a change
Check consistency with `AGENT.md`; consistency among architecture, decisions, and status; workflow-template completeness where workflows are added; absence of secrets and unintended personal data; clarity about documentation versus implemented functionality; and whether validation evidence supports each claimed result.

## Status discipline
- **Proposed:** suggested, not approved.
- **Approved:** accepted by the owner.
- **Implemented:** committed to the repository.
- **Verified:** checked against explicit criteria with evidence.
- **Blocked:** cannot proceed; state the blocker.
- **On hold:** intentionally paused.

Never report a proposal as implemented or an implementation as verified without evidence.
