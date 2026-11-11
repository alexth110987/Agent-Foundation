# Permissions and Authorization

**Status:** Logical architecture proposed — pending owner review.  
**Implementation:** Not implemented. No permission service, policy engine, identity provider, or enforcement adapter exists.

## Purpose

The Permission System defines how a future agent evaluates and enforces authorization for bounded actions. It applies least privilege, deny by default, central policy decisions, and local enforcement at protected component boundaries.

See [Permission System — Logical Architecture](PERMISSION_ARCHITECTURE.md) for proposed components, authority boundaries, request/decision contracts, approval lifecycle, enforcement, revocation, failure handling, security considerations, validation requirements, and deferred decisions.

## Baseline safeguards

- Core rules and approved owner decisions remain authoritative.
- Identity is not permission; authentication and authorization are separate.
- Workflows and tools may request permission but cannot grant or expand it.
- The Permission Service is the proposed central policy decision authority.
- Each protected component and tool adapter must enforce scoped decisions locally.
- Local enforcement may be stricter but cannot broaden a central grant.
- Missing, unavailable, stale, malformed, expired, revoked, or indeterminate authorization fails closed.
- Explicit approval is bounded to the stated action, resource, purpose, and validity.
- Authorization is not proof that an action was executed or succeeded.
- Never store credentials or secrets in this repository.

## Deferred decisions

No identity provider, principal model, policy language, policy store, service topology, approval provider, decision-token format, cache/revocation interval, runtime, or deployment approach has been selected.

This is a design proposal only. It does not authorize implementation, integrations, tool access, or operational use.
