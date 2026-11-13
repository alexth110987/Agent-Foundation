# Repository Instructions

**Authority:** This file defines repository-maintenance conventions only. It is subordinate to `AGENT.md` and must never override the project identity, scope, safeguards, approved decisions, or change-control rules in that file.

## Required reading before repository work
Read the live files on `main` in this order:
1. `AGENT.md`
2. `AGENTS.md`
3. `docs/PROJECT_STATUS.md`

If any required file cannot be read, stop and report the exact failure before making repository changes.

## Repository workflow
- Treat `main` as the sole canonical branch and source of official approved project records.
- Use GitHub as the official project-management and durable record-keeping environment. Do not create a parallel project status, decision register, or knowledge store in ChatGPT.
- Prefer one central ChatGPT conversation for project work where practical. This does not remove context limits or guarantee continuity; when a handover is necessary, refer to the authoritative GitHub records and re-read the required live-main files.
- Chat discussion, summaries, drafts, and memory are not authoritative project records unless an approved result is recorded in the repository.
- Inspect the relevant files and their current state before editing.
- Keep changes narrowly scoped to the task and preserve established decisions, exclusions, holds, and evidence.
- Do not infer approval from discussion, research, or a proposed design.
- Do not add secrets, credentials, private keys, tokens, or other sensitive access material.
- Do not introduce executable capabilities, integrations, domain-specific workflows, or runtime dependencies without explicit owner approval.
- For repository changes, prepare the complete change before committing and make one atomic commit for the task.
- Use the current branch head as the expected parent when updating `main`; do not overwrite concurrent changes.
- After committing, re-fetch every changed file from `main` and verify its content and the resulting commit.
- Inspect relevant GitHub Actions runs and status checks. Report absent checks or runs as absent; never describe them as passing.
- Record material documentation changes in `docs/CHANGELOG.md` and keep `README.md` and `docs/PROJECT_STATUS.md` consistent when their content is affected.

## Reporting
Report the exact commit, changed files, verification performed, and any missing or failed checks. Distinguish documentation changes from implemented or verified runtime functionality. Do not claim completion until post-commit verification succeeds.
