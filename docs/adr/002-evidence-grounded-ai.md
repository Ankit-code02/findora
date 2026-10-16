# ADR 002: Evidence-Grounded AI Recommendations

- Status: Accepted
- Date: 2026-10-16

## Context

Findora will recommend websites, developer tools, technology stacks,
and deployment services. Incorrect pricing or unsupported claims could
mislead users and damage trust.

## Decision

Keep deterministic evaluation and factual source data separate from AI-generated
explanations.

The backend will retrieve relevant catalogue data and evaluate compatibility,
constraints, and available pricing information. An AI provider may help explain
the evidence and suggest alternatives.

AI output must be validated and must not be treated as verified fact by default.

Use a provider-independent AI interface and a deterministic mock provider
for development and automated testing.

## Consequences

Positive:
- Recommendations are easier to test and explain.
- AI provider changes do not require rewriting business logic.
- Development and CI do not require paid model calls.
- Unsupported output can be rejected or replaced with a safe fallback.

Trade-offs:
- Catalogue quality and source freshness remain important.
- AI explanations require evaluation and monitoring.
- Real-time research may need separate, controlled integrations.

## Safety Requirements

- Do not expose provider credentials to the browser.
- Do not execute arbitrary model-generated SQL, shell commands, or infrastructure actions.
- Apply usage limits, timeouts, and bounded retries.
- Identify unverified facts and assumptions.
- Preserve relevant source references for recommendation claims.
