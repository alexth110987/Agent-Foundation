# Tool Registry

**Status:** Empty registry. No tools are approved, configured, validated, active, or connected.  
**Governance:** Proposed; see [Tool Governance](../docs/TOOL_GOVERNANCE.md).

The registry is an index, not an authorization mechanism. Technical availability, connector visibility, a candidate record, or a successful test does not confer permission to invoke a tool.

| Tool ID | Name | Provider / exact version | Purpose and scope | Lifecycle state | Owner | Approval / evidence |
| --- | --- | --- | --- | --- | --- | --- |
| — | No tools registered | — | — | — | — | — |

Before adding a candidate, follow registration and review requirements in docs/TOOL_GOVERNANCE.md. A candidate record must identify exact tool/API and version, owner, purpose, allowed/prohibited operations, data scope, read/write/delete and external side effects, identity and authorization boundaries, authentication and secret reference (never secret values), rate/cost/dependency limits, failure/retry/recovery behavior, logging and retention requirements, provenance/lifecycle obligations, licence/privacy/security review, validation evidence, approval, review date, expiry, and revocation conditions.

**Current restrictions:** No tools may be invoked or connected under this registry. No credential or secret may be stored in this public repository.
