# Findora — Cost Safeguards

## 1. Objective

Findora must remain affordable to develop, test, operate, and maintain.
Paid services must be introduced deliberately rather than by default.

## 2. Local-First Development

- Develop and test locally before provisioning cloud infrastructure.
- Use synthetic resource and pricing data during early development.
- Use a deterministic mock AI provider by default.
- Do not require paid AI calls for unit or integration tests.
- Avoid unnecessary managed databases and always-on cloud resources during development.

## 3. AI Cost Controls

- Keep AI provider credentials on the server.
- Never expose API keys in frontend code.
- Set limits for request frequency, input size, output tokens, and execution time.
- Apply per-user and global usage limits.
- Bound retries and avoid retry storms.
- Cache only where correctness, privacy, and freshness permit.
- Record usage and estimated cost without logging prompts containing sensitive data.
- Provide a graceful fallback when the provider is unavailable or the budget is exhausted.
- Allow deterministic mock mode for development and automated testing.

## 4. Cloud Cost Controls

Before provisioning AWS services:

1. Estimate expected monthly charges.
2. Identify fixed and usage-based costs.
3. Review networking, logging, storage, database, and data-transfer charges.
4. Configure billing alerts and AWS Budgets where available.
5. Select only the regions and services required.
6. Apply resource tags for ownership and environment.
7. Document how to stop or remove development resources safely.

AWS Budgets and billing alerts are notifications, not guaranteed spending caps.

## 5. Environment Separation

- Local development must not require production credentials.
- Test data must be synthetic or appropriately sanitized.
- Production secrets must be stored outside source control.
- Development and production environments must use separate configuration and credentials.
- Destructive operations must never run automatically against production resources.

## 6. Resource Efficiency

- Use pagination for large results.
- Apply database indexes based on query patterns.
- Avoid unnecessary repeated model calls.
- Use bounded background jobs and connection pools.
- Set request, database, and provider timeouts.
- Monitor slow queries and expensive operations before optimizing.
- Introduce additional infrastructure only when measured requirements justify it.

## 7. Cost Visibility

The application should eventually record:
- AI request counts and token usage where available
- Estimated model-provider cost where pricing data is available
- Resource synchronization volume
- Storage usage and import sizes
- Background job failures and retry counts

Estimates must identify their assumptions and must not be presented as exact invoices.

## 8. Shutdown and Cleanup

Before ending a cloud development session:
- Stop resources that do not need to remain active.
- Check for unused load balancers, disks, snapshots, databases, and public IP addresses.
- Preserve required data and backups before deleting resources.
- Verify resource deletion rather than assuming it succeeded.

## 9. Release Gate

Cloud deployment requires a reviewed cost estimate, configured alerts,
least-privilege access, and documented cleanup procedures.

No production infrastructure should be provisioned until the deployment plan
and expected charges have been reviewed.
