# Security Architecture Patterns

This repository contains **design-level security architecture patterns** for modern
cloud and product systems (web and APIs). The goal is to share practical patterns
that improve security posture while enabling engineering velocity.

These patterns are written to be:
- **Architecture-first** (problem → trade-offs → design)
- **Technology-agnostic** where possible
- **Actionable** for product and platform engineering teams

## How to use this repository

Each pattern includes:
- Context and problem statement
- Architecture approach and decision points
- Trade-offs and failure modes
- Implementation notes (high-level)
- Verification and operational considerations

Patterns are designed to be adapted to your environment, not copy-pasted as-is.

## Patterns

- [001 Secure API Boundary](patterns/001-secure-api-boundary.md)
- [002 Authorization Enforcement Point](patterns/002-authz-enforcement-point.md)
- [003 Identity Trust Boundary](patterns/003-identity-trust-boundary.md)

## Notes

These documents are intended as **reference architecture guidance** and do not
include exploit payloads, target-specific details, or sensitive configuration.
