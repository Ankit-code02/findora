# ADR 001: Start with a Modular Monolith

- Status: Accepted
- Date: 2026-10-16

## Context

Findora needs a maintainable backend for resource discovery, comparisons,
recommendations, AI integration, pricing information, and user workspaces.

Starting with multiple independently deployed services would add operational
complexity before we have evidence that separate services are necessary.

## Decision

Use a modular monolith built with Java 21 and Spring Boot 3.x.

Organize the code into clearly defined business modules. Keep module
responsibilities explicit and avoid unnecessary dependencies between modules.

## Consequences

Positive:
- Simpler local development and deployment.
- Easier debugging and integration testing.
- Fewer infrastructure and operational costs.
- Clear module boundaries that allow future extraction if justified.

Trade-offs:
- The application shares a deployment lifecycle.
- Module boundaries must be enforced through code organization and tests.
- A single deployment may eventually require scaling as a whole.

## Reconsideration Criteria

Reconsider this decision only when measured workload, independent scaling,
organizational needs, or reliability requirements justify a different architecture.
