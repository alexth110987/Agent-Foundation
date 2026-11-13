# Repository Instructions

**Authority:** Subordinate to `docs/MASTER_PROMPT.md`. This file governs repository-maintenance procedure only.

## Required live reading

Before every task, workflow, run, file operation, or GitHub operation, read from `main` in this exact order:
1. `docs/MASTER_PROMPT.md`
2. `AGENTS.md`
3. `docs/PROJECT_STATUS.md`

If any file cannot be read, stop and report the exact failure. The initial creation of these three bootstrap files is the owner's explicitly authorized exception.

## Repository workflow

- Inspect the repository and relevant live files before editing.
- Preserve all approved decisions, exclusions, holds, compatibility rules, safeguards, and evidence.
- Do not infer approval from discussion, research, or tool availability.
- Do not add secrets, credentials, tokens, private keys, or unnecessary personal data.
- Run privacy/security checks before public changes.
- Prepare each task as one complete, atomic commit per repository.
- Do not overwrite concurrent changes; use the current branch head as the expected parent.
- After committing, re-fetch every changed file and verify exact content and commit identity.
- Inspect relevant GitHub Actions runs and checks. Report missing or absent checks honestly.
- Keep documentation, implementation, feature tests, compatibility tests, smoke tests, and release approval distinct.
- Do not modify production systems or make unapproved changes to other repositories.

## Reporting

Report changed paths, commit SHA, post-commit verification, tests and their exact scope, workflow results, and any pending, failed, or blocked items. Never claim implementation or verification that did not occur.
