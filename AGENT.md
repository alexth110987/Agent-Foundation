# Agent Foundation — Master Instructions

**Authority:** This is the highest-authority project instruction document in this repository.

## Project identity
Agent Foundation is an independent project to establish a reliable, modular, extensible foundation for a future AI agent. Its operational purpose is intentionally undefined. Do not infer one from examples, folder names, or other projects.

## Source of truth and workspace boundaries
- This repository's `main` branch is the canonical source for approved project files and recorded decisions.
- The dedicated ChatGPT Project **Agent Foundation** is the planning and collaboration workspace.
- Do not assume automatic synchronization between ChatGPT and GitHub.
- Record approved decisions and material changes in this repository.
- Keep this project independent from all other projects. Do not import their instructions, files, memories, workflows, assumptions, or decisions unless the owner explicitly authorizes a specific import.
- When sources conflict, follow this file first, then approved decision records and current project status. Raise unresolved conflicts instead of silently choosing.

## Current scope
Foundation design only: governance, neutral core architecture, workflow specifications, memory and knowledge boundaries, permissions and tool governance, validation, testing, logging, and error handling.

Do not build an operational agent, select a domain, connect external services, enable tools, or add domain-specific workflows until explicitly approved.

## Status vocabulary
- **Proposed:** under consideration; not approved.
- **Approved:** explicitly accepted by the owner.
- **Implemented:** present in the repository or intended execution environment.
- **Verified:** checked against stated acceptance criteria, with evidence recorded.
- **Deferred / On hold:** intentionally not active.
- **Rejected:** not selected; retain the reason when useful.

A proposal is not approval. Research is not approval. Implementation is not verification.

## Authority and change control
Do not silently change approved decisions, scope, exclusions, safeguards, or status. For consequential changes, explain the impact and obtain explicit authorization before acting.

## Safety and permissions baseline
- Default to least privilege.
- Treat tools and external integrations as unavailable until deliberately configured and authorized.
- Never store passwords, API keys, access tokens, private keys, recovery codes, or other secrets in this public repository.
- Require explicit authorization before external communication, purchases, publication, data deletion, destructive modification, or other consequential actions.
- Prefer scoped, reversible actions and preserve backups where appropriate.
- Never claim an action succeeded without checking its result.

## Workflow quality
Every workflow must use the shared template and define its trigger, inputs, preconditions, permissions, steps, validation, outputs, and failure handling. A workflow document is not evidence that it is executable, tested, or approved for use.

## Documentation and reporting
Keep the README, architecture, decision log, changelog, and project status consistent. Record material changes with date, rationale, and verification evidence. Report accurately what is proposed, approved, implemented, verified, pending, blocked, or failed.

## Development sequence
1. Establish the foundation.
2. Agree the future purpose and operating context.
3. Approve an architecture for that purpose.
4. Define workflows and permission boundaries.
5. Implement only approved capabilities.
6. Validate against explicit criteria.
7. Consider release only after separate approval.

Until the owner changes the scope, remain in **Stage 1 — Core foundation**.
