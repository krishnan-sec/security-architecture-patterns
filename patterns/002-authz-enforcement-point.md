# Pattern 002 - Authorization Enforcement Point (AEP)

## Context

This pattern applies to systems where **authorization decisions must remain
consistent, explainable, and evolvable** across multiple services, APIs, and
client types. It is particularly relevant in SaaS, multi-tenant, microservice,
and platform architectures.

As systems scale, authorization logic tends to fragment across codebases,
leading to subtle inconsistencies and increasing security risk.

---

## Problem

Authorization is often implemented opportunistically—embedded within individual
services, controllers, or data access layers. Over time, this results in:

- Inconsistent enforcement of access rules across services
- Duplication of authorization logic with divergent behavior
- Difficulty reasoning about “who can do what” at a system level
- Increased likelihood of broken object-level authorization (BOLA)
- High cost of change when policies evolve

The core issue is not missing checks, but **the absence of a clearly defined
enforcement point with ownership over authorization guarantees**.

---

## Forces / Constraints

- Teams need autonomy to ship features quickly
- Authorization rules evolve with business requirements
- Systems span multiple services, languages, and deployment models
- Centralized enforcement risks becoming a bottleneck
- Authorization decisions must be auditable and explainable

A viable design must balance **centralized policy intent** with **distributed
system execution**.

---

## Pattern

Introduce an explicit **Authorization Enforcement Point (AEP)** that is
responsible for enforcing authorization decisions consistently across the
system.

The AEP acts as the **gatekeeper between identity context and protected
resources**, ensuring that access decisions are applied uniformly regardless of
where requests originate.

Key responsibilities of an AEP include:

- Receiving a normalized identity and context (subject, tenant, action, resource)
- Enforcing authorization decisions derived from policy
- Rejecting requests that violate policy invariants
- Ensuring authorization outcomes are observable and traceable

The AEP may be implemented at different layers (API boundary, service layer,
sidecar, shared library), but **its responsibility must be explicit and
non-optional**.

---

## Key Decisions

- Where does authorization enforcement occur relative to request handling?
- Is policy evaluation centralized, distributed, or hybrid?
- What context is required to make correct authorization decisions?
- How are policy changes propagated safely?
- How are authorization decisions logged and audited?

These decisions define whether authorization remains a system property or
devolves into scattered checks.

---

## Implementation Notes (High-Level)

- Separate **policy definition**, **policy evaluation**, and **policy enforcement**
- Treat authorization decisions as data (inputs, outputs, rationale)
- Prefer deny-by-default semantics
- Ensure identity and resource context cannot be modified post-evaluation
- Design enforcement to fail closed under partial system failures
- Avoid embedding business logic inside authorization policies

The implementation may vary, but enforcement guarantees must be uniform.

---

## Failure Modes

- Authorization logic duplicated across services with subtle differences
- Enforcement bypassed for “internal” or “trusted” paths
- Policy complexity grows faster than reviewability
- Enforcement points drift as services evolve
- Authorization failures are logged without sufficient decision context

Most failures arise from **implicit trust and unclear ownership**.

---

## Verification

- Test authorization behavior across representative roles, actions, and resources
- Validate object-level authorization under boundary conditions
- Confirm deny-by-default behavior for unknown or malformed requests
- Audit authorization logs for decision traceability
- Periodically review enforcement coverage against system architecture

Verification should confirm **consistency and completeness**, not just correctness.

---

## When NOT to use

- Very small systems where authorization logic is trivial and unlikely to evolve
- Environments where policy cannot be externalized or reviewed meaningfully
- Systems where authorization must be enforced exclusively at the data storage
  layer (though upstream enforcement may still provide defense-in-depth)

Patterns should be adopted intentionally, not reflexively.

---
