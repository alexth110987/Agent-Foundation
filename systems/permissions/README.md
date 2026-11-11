# Permissions and Authorization

**Status:** Permission architecture approved (D-008); Identity Contract and Authorization Contract proposed — pending owner review.  
**Implementation:** Not implemented. No permission service, policy engine, identity provider, approval service, or enforcement adapter exists.

## Purpose

The Permission System defines how a future agent evaluates and enforces authorization for bounded actions. It applies least privilege, deny by default, central policy decisions, and local enforcement at protected component boundaries.

## Architecture and contracts

- [Permission System — Logical Architecture](PERMISSION_ARCHITECTURE.md) — approved logical architecture (D-008); implementation not authorized.
- [Identity Contract](IDENTITY_CONTRACT.md) — proposed; pending owner review.
- [Authorization Contract](AUTHORIZATION_CONTRACT.md) — proposed; pending owner review.

The contracts refine identity and authorization boundaries but do not amend D-008 or authorize implementation.

## Baseline safeguards

- Core rules and approved owner decisions remain authoritative.
- Identity is not permission; authentication and authorization are separate.
- Workflows and tools may request permission but cannot grant or expand it.
- The Permission Service is the central policy decision authority in the approved logical architecture.
- Each protected component and tool adapter must enforce scoped decisions locally.
- Local enforcement may be stricter but cannot broaden a central grant.
- Missing, unavailable, stale, malformed, expired, revoked, or indeterminate authorization fails closed.
- Explicit approval is bounded to the stated action, resource, purpose, and validity; approval alone is not a permit.
- Authorization is not proof that an action was executed or succeeded.
- Never store credentials or secrets in this repository.

## Deferred decisions

No identity provider, final principal taxonomy, authentication protocol, policy language, policy store, API/protocol, decision format, approval provider, service topology, cache/revocation interval, runtime, or deployment approach has been selected.

No permission service, policy engine, identity provider, approval service, decision-token mechanism, or enforcement adapter exists. These documents are design records only; they do not authorize implementation, integrations, tool access, or operational use.
