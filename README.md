# Findora

**Discover smarter. Choose better.**

Findora is an AI-assisted discovery and recommendation platform that helps people find useful websites, developer tools, learning resources, technology stacks, and affordable digital services.

Instead of relying on scattered recommendations and promotional social-media posts, users can explore curated resources, compare alternatives, understand pricing limitations, and receive personalized guidance for their projects.

## Planned Features

- **Resource Discovery:** Search websites and tools by category, purpose, and price model.
- **Project-to-Stack Advisor:** Describe a project and receive suitable technology recommendations.
- **Smart Comparisons:** Compare features, costs, limitations, and compatibility.
- **Deployment Planner:** Evaluate hosting, databases, storage, and deployment scenarios.
- **Personal Workspace:** Save resources, comparisons, and project plans.
- **Resource Verification:** Track source links, pricing references, and verification dates.

> Findora is currently in its foundation phase. Planned features are not yet implemented unless explicitly documented as complete.

## Technology Direction

- Java 21 and Spring Boot
- React and TypeScript
- PostgreSQL and Flyway
- Spring Security and OpenID Connect
- AI provider abstraction with a deterministic development mode
- Docker and GitHub Actions
- AWS deployment after local verification and cost review

## Engineering Principles

- Security by default
- Explainable recommendations
- Evidence-backed information
- Transparent pricing assumptions
- Automated testing
- Cost-conscious infrastructure
- Maintainable architecture

## Documentation

- [Product Specification](docs/product-spec.md)
- [Architecture](docs/architecture.md)
- [Security Threat Model](docs/threat-model.md)
- [Cost Safeguards](docs/cost-controls.md)
- [Architecture Decision Records](docs/adr/)

## Development Status

The repository currently contains the product and engineering foundation.
Application implementation, automated tests, AI integrations, and cloud deployment will be added in later phases.

## Security

Never commit API keys, passwords, access tokens, production environment files, or real user data.

Report security concerns privately rather than publishing exploitable details.

## License

No license has been selected yet. All rights are reserved by default until a license is added.
