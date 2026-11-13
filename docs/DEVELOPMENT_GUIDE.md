# Development Guide

## Working model
Use this GitHub repository on `main` as the sole official project-management environment and canonical source of truth for all approved project records, decisions, status, and durable evidence. ChatGPT is an assisting interface for discussion, research, drafting, review, and authorized repository work; it is not a parallel project workspace or authoritative record. Prefer one central ChatGPT conversation where practical, but do not rely on chat history for continuity. If a handover is needed, point to the relevant GitHub records and re-read the required live-main files. There is no assumed automatic synchronization.

## Before a change
1. Read the live files on `main` in this exact order: `AGENT.md`, `AGENTS.md`, then `docs/PROJECT_STATUS.md`. If any required file is inaccessible, stop and report the exact failure before making repository changes.
2. Read relevant design or decision records.
3. Identify the exact files and scope affected.
4. Check current repository state rather than relying on memory or old conversation context.
5. Separate proposals from approved changes.
6. Identify permission, privacy, compatibility, and safety implications.

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
