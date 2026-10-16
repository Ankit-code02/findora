# Findora — Security Threat Model

## 1. Purpose

This document identifies important security risks and the controls required
before Findora is released to real users.

The threat model must evolve as the application, integrations, and deployment change.
No design can guarantee zero vulnerabilities.

## 2. Assets to Protect

- User identity and authentication sessions
- User-owned project descriptions and saved collections
- Resource catalogue and pricing observations
- AI provider credentials and other application secrets
- Database contents and backups
- Administrative operations and audit records
- Cloud credentials, deployment configuration, and logs

## 3. Trust Boundaries

- Browser to backend API
- Backend to PostgreSQL
- Backend to AI provider
- Backend to external resource and pricing sources
- Application runtime to AWS services
- User and organization boundaries
- CI/CD pipeline to deployment environment

## 4. Threats and Required Controls

### Authentication Spoofing

Threat:
An attacker impersonates a user or submits an invalid identity token.

Controls:
- Use a standards-compliant OIDC provider.
- Validate token signature, issuer, audience, expiry, and required claims.
- Use secure redirect URI and session configuration.
- Reject invalid or expired authentication credentials.
- Never accept identity claims supplied only by the browser.

### Broken Authorization

Threat:
A user accesses administrative functions or another user's saved data.

Controls:
- Enforce authorization in the backend.
- Deny access by default.
- Check permissions on every protected operation.
- Scope queries to the authenticated user's authorized organization.
- Test horizontal and vertical privilege escalation.
- Never rely only on frontend route guards.

### Cross-Organization Data Exposure

Threat:
An organization accesses another organization's projects, saved resources, or settings.

Controls:
- Include organization ownership in relevant data models.
- Apply tenant scope to reads, updates, and deletes.
- Validate organization membership server-side.
- Add negative integration tests for cross-tenant access.
- Avoid trusting organization IDs from request bodies without authorization checks.

### Malicious Resource Content and Prompt Injection

Threat:
External descriptions or imported content contain instructions intended to manipulate the AI.

Controls:
- Treat external content as untrusted data.
- Separate trusted system instructions from retrieved content.
- Restrict AI tools and permissions.
- Validate structured output.
- Never execute model-generated SQL, shell commands, or infrastructure changes.
- Do not pass secrets or unrelated user data to the model.

### Server-Side Request Forgery

Threat:
A resource URL causes the backend to access internal services or sensitive endpoints.

Controls:
- Prefer known official URLs and trusted source integrations.
- Restrict outbound requests to approved protocols and destinations.
- Block loopback, private, link-local, and cloud metadata addresses.
- Revalidate destinations after DNS resolution and redirects.
- Apply network egress restrictions and request timeouts.
- Do not create an unrestricted URL-fetching proxy.

### Unsafe File Uploads

Threat:
A malicious or oversized CSV exhausts resources or exploits a parser.

Controls:
- Enforce file-size, row-count, and column-count limits.
- Validate file format and required fields.
- Parse files defensively and reject malformed records.
- Avoid executing uploaded content.
- Do not trust file extensions or browser-supplied MIME types.
- Store uploads outside executable application paths.

### Secret Exposure

Threat:
Credentials are committed to Git or written to logs.

Controls:
- Never commit real secrets or production environment files.
- Maintain a safe .env.example containing placeholders only.
- Use managed secrets in deployed environments.
- Add secret scanning to the development workflow.
- Redact sensitive values from logs and error messages.
- Rotate any credential that may have been exposed.

### Denial of Service and Resource Exhaustion

Threat:
Requests, file uploads, or AI calls exhaust CPU, memory, database connections, or spending limits.

Controls:
- Apply request limits and rate limits.
- Use bounded concurrency and queues.
- Set connection, request, and AI timeouts.
- Limit AI input and output tokens.
- Cap retries and prevent retry storms.
- Paginate large results and constrain database queries.
- Provide a safe fallback when the AI provider is unavailable.

### Database Injection and Data Tampering

Threat:
Malicious input changes queries or unauthorized operations alter records.

Controls:
- Use parameterized database operations.
- Validate request input and enforce database constraints.
- Apply least-privilege database roles.
- Use transactions for related updates.
- Test invalid and malicious inputs.
- Record security-sensitive changes in an attributable audit trail.

### Dependency and Build Pipeline Compromise

Threat:
A vulnerable or malicious dependency compromises the application.

Controls:
- Use dependency vulnerability scanning.
- Keep dependencies version-controlled and reviewed.
- Protect repository and deployment permissions.
- Scan secrets and source code in CI.
- Use minimal production containers.
- Pin or constrain critical build inputs and review dependency updates.

### Cloud Misconfiguration

Threat:
A public database, overly broad IAM policy, or exposed administrative service leaks data.

Controls:
- Prefer private database networking.
- Use least-privilege IAM roles.
- Avoid permanent cloud keys where temporary role credentials are available.
- Require encrypted connections.
- Restrict administrative access.
- Review cloud configuration before deployment.
- Enable appropriate logging, monitoring, and billing alerts.

### Audit Record Tampering

Threat:
A user modifies or deletes records of important actions.

Controls:
- Restrict audit-event write and read permissions.
- Record actor, action, timestamp, and relevant non-sensitive context.
- Prevent ordinary users from modifying audit history.
- Avoid recording credentials or unnecessary personal data.
- Consider protected centralized log storage for production.

## 5. Security Verification Requirements

Before production release, test at least:

- Invalid, expired, and incorrectly scoped identity tokens
- Unauthorized access to protected endpoints
- Cross-user and cross-organization data access
- Malformed requests and injection attempts
- Oversized and malformed file uploads
- Prompt injection through untrusted resource content
- AI timeout, invalid output, and rate-limit behavior
- SSRF attempts against local and private network destinations
- Secret scanning and dependency vulnerability checks
- Backup restoration and recovery procedures

## 6. Incident Response

If a credential or sensitive data is exposed:
1. Stop further exposure where possible.
2. Revoke or rotate the affected credential.
3. Review relevant access logs.
4. Assess affected systems and data.
5. Restore safe configuration.
6. Document the incident and corrective action.

## 7. Release Policy

A successful build alone is not proof of security.

Release requires passing automated tests, reviewing security-sensitive changes,
checking dependency findings, and documenting unresolved risks.

High-severity unresolved issues must block release unless a documented,
authorized exception and compensating controls exist.
