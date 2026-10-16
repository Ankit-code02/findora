# Findora — Architecture

## 1. Architecture Style

Findora will begin as a modular monolith with clear module boundaries.
We will not introduce microservices unless measured requirements justify them.

## 2. Technology Stack

- Java 21
- Spring Boot 3.x
- Maven
- Spring Security
- Spring Data JPA
- PostgreSQL
- Flyway
- React
- TypeScript
- Vite
- Docker
- GitHub Actions
- AWS for the later deployment phase

## 3. Main Modules

### Identity and Access
OIDC authentication, user identity, organization membership, and authorization.

### Resource Catalogue
Resources, categories, tags, official URLs, pricing references, and verification status.

### Search and Discovery
Search queries, filters, pagination, and relevant resource retrieval.

### Comparison Engine
Consistent comparison criteria, explainable scoring, and suitability assessments.

### Project Advisor
Project requirements, clarifying questions, technology candidates, and implementation plans.

### AI Gateway
Provider-independent model access, structured responses, validation, usage limits, and safe fallbacks.

### Pricing Intelligence
Pricing sources, verification dates, assumptions, free-tier restrictions, and cost scenarios.

### Personal Workspace
Saved resources, collections, project plans, and comparisons.

### Audit and Operations
Security events, application health, operational metrics, and administrative activity.

## 4. Data Flow

1. A user submits a search, comparison, or project description.
2. The backend validates the request.
3. The backend retrieves relevant resources from the catalogue.
4. Deterministic rules evaluate compatibility, constraints, and available pricing data.
5. The AI gateway may explain the evidence and suggest alternatives.
6. The backend validates the result and records applicable provenance.
7. The frontend displays recommendations, sources, assumptions, and limitations.

## 5. AI Design

- Use a provider-independent interface.
- Provide a deterministic mock implementation for local development and testing.
- Keep financial calculations and scoring rules in application code.
- Validate model output against explicit schemas.
- Never execute arbitrary model-generated SQL, shell commands, or infrastructure actions.
- Limit model input, output, retries, timeouts, and usage.
- Treat retrieved content as untrusted data.

## 6. Data Storage

PostgreSQL will store structured resources, categories, source records, pricing observations,
comparison criteria, and user-owned workspace data.

Flyway will manage versioned database migrations.

Search will initially use PostgreSQL capabilities. A dedicated search engine will be
considered only if measured search requirements justify its operational cost.

## 7. Deployment Direction

Local development will be the first target.

AWS deployment will be introduced after local functionality, tests, security controls,
and cost estimates are reviewed.

Potential AWS services include:
- ECS Fargate for application containers
- RDS for PostgreSQL
- S3 for suitable object storage
- CloudFront for frontend delivery
- Secrets Manager for application secrets
- CloudWatch for logs and metrics

The final service selection and network architecture require a separate cost and security review.

## 8. Reliability

- Validate requests at system boundaries.
- Apply timeouts and bounded retries.
- Make imports and scheduled processing idempotent.
- Use pagination for potentially large result sets.
- Apply database constraints and appropriate indexes.
- Provide safe error responses and health checks.
- Test backup restoration before relying on backups.

## 9. Architecture Evolution

Every significant architectural change should have an Architecture Decision Record in docs/adr.
Changes must explain the problem, alternatives, trade-offs, security impact, and operational cost.
