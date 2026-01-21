# Pattern 006 - Tenant Isolation Boundary (SaaS)

## Context

This pattern applies to multi-tenant SaaS systems where multiple customers share
application infrastructure, services, or data stores.

Tenant isolation is a foundational security and trust requirement, often
assumed but not always explicitly designed or enforced.

---

## Problem

In many SaaS systems, tenant separation is enforced implicitly through
application logic. Over time, this leads to:

- Inconsistent tenant checks across services
- Broken object-level authorization and data leakage
- Difficulty reasoning about blast radius during incidents
- High risk during feature expansion or refactoring

The core problem is not multi-tenancy itself, but **lack of an explicit boundary
that defines how and where tenant isolation is enforced**.

---

## Forces / Constraints

- Cost efficiency encourages shared infrastructure
- Performance and scalability requirements vary by tenant
- Teams evolve services independently
- Data models and access patterns grow more complex over time
- Customers expect strong isolation guarantees regardless of implementation

The design must make **tenant isolation a system property**, not a convention.

---

## Pattern

Establish a clear **Tenant Isolation Boundary** that defines where tenant context
is established, enforced, and verified.

At this boundary:
- Tenant identity is explicitly resolved and normalized
- All downstream access is scoped to a single tenant context
- Cross-tenant access is prohibited by default
- Any intentional cross-tenant operation is explicit and auditable

The boundary may exist at the API layer, service layer, data access layer, or a
combination thereof, but its responsibilities must be unambiguous.

---

## Key Decisions

- How is tenant identity established and propagated?
- Where is tenant enforcement guaranteed?
- How are shared vs tenant-scoped resources handled?
- How is tenant context validated during service-to-service calls?
- What isolation guarantees are communicated externally?

These decisions directly impact customer trust and regulatory exposure.

---

## Implementation Notes (High-Level)

- Treat tenant context as mandatory, not optional
- Enforce tenant scoping as close to data access as possible
- Prefer deny-by-default for cross-tenant operations
- Use explicit schemas or partitions where appropriate
- Log and monitor cross-tenant access attempts
- Design administrative or support access paths deliberately

Implementation details may vary, but isolation guarantees must be explicit.

---

## Failure Modes

- Tenant checks implemented inconsistently across services
- Background jobs or async workflows bypass tenant enforcement
- Shared caches or indices leak tenant data
- Administrative paths erode isolation guarantees
- Tenant context lost or overwritten during request propagation

Most failures emerge from **implicit trust and hidden coupling**.

---

## Verification

- Test tenant isolation across representative access paths
- Validate denial of unauthorized cross-tenant access
- Review data access layers for implicit tenant assumptions
- Simulate failure scenarios and assess blast radius
- Audit logs for cross-tenant activity

Verification should confirm **containment**, not just correctness.

---

## When NOT to use

- Single-tenant systems with no shared infrastructure
- Architectures where full physical isolation is guaranteed
- Environments where tenancy is externalized entirely

Even then, documenting tenant assumptions remains valuable.

---
