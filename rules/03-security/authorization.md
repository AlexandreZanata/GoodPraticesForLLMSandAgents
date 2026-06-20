---
id: sec.authz
triggers:
  - authorization
  - authz
  - permission
  - rbac
  - abac
  - bola
  - access-control
alwaysApply: false
---
# Authorization

> Default-deny, server-side only, explicit BOLA prevention (OWASP 2025).

## Core rules

- **Default-deny:** no permission unless explicitly granted.
- **Server-side only:** never rely on UI hiding or client-side role checks for security.
- Every mutating and sensitive read operation checks authorization in **Application layer** before domain work.

## BOLA — Broken Object Level Authorization

> Verify the requester owns or may access the **specific object ID** — not merely that they are authenticated.

```text
WRONG: GET /orders/{id}  → return order if user is logged in
RIGHT: GET /orders/{id}  → return order if user owns order OR has role X on tenant
```

- Resolve resource → check actor relationship to that resource (owner, tenant member, role on resource).
- NEVER use sequential/predictable IDs alone as security (use UUIDs + authorization check).

## RBAC + ABAC

- Roles from domain glossary with business meaning.
- Attribute checks: tenant isolation, resource state, contextual attributes (e.g. assigned clinician).

## Agent action

For every endpoint with `{id}` in path, add explicit authorization test: wrong user → 403, not 404 leakage unless policy requires uniform 404.
