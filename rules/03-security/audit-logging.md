---
id: sec.audit
triggers:
  - audit
  - security-log
  - compliance
alwaysApply: false
---
# Audit Logging

> Security-relevant events logged with actor and action — never log sensitive payload.

## MUST audit

- Login success/failure, logout, MFA events
- Permission/role changes
- Data export, bulk delete, admin overrides
- Tenant configuration changes
- Access denied to sensitive resources (sampled if high volume)

## Log fields

```text
timestamp, correlationId, actorId, tenantId, action, resourceType, resourceId, outcome, clientIp
```

## MUST NOT log

- Passwords, tokens, session secrets
- Full PII payloads (SSN, card numbers) — log ID references only
- Request bodies containing secrets even on error

## Retention

- Define retention per regulation (LGPD/GDPR) — see `07-data-management/pii-and-data-retention.md`.
- Immutable append-only store where compliance requires.

## Agent action

When adding admin or auth feature, add audit event emission in same PR — not follow-up.
