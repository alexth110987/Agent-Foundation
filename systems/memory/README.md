# Memory System — Design Placeholder

**Status:** Not implemented. No memory technology has been selected.

## Design requirements
A future design should distinguish:
- short-lived task context;
- durable user-approved preferences or facts;
- project-specific decisions and records;
- curated reference knowledge;
- temporary data that should expire or be discarded.

## Safeguards
- Define what may be stored and why.
- Apply data minimization and purpose limitation.
- Keep project boundaries explicit.
- Record provenance, date, and confidence where appropriate.
- Define correction, retention, export, and deletion behavior.
- Never store credentials or secrets as ordinary memory.

No database, vector store, embedding model, or memory provider is selected.
