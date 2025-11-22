System Design Notes

Overview

Concise reference for designing hybrid architectures (microservices + serverless) on Azure. Focus on mapping requirements to technology choices, defining bounded contexts, and translating non-functional requirements into architecture patterns.

Key Principles

- Right tool for the problem: AKS for long-running, stateful, and complex services; Azure Functions for ephemeral, event-driven tasks.
- Bounded contexts: assign data ownership per context and prefer async boundaries (Service Bus/Event Grid) between contexts.
- Separation of concerns: control plane (platform), data plane (datastores), application plane (AKS/Functions).
- Design for failure: expect partial failures, implement retries with exponential backoff and idempotency.
- Security by default: least-privilege, private endpoints, managed identities.

Non-functional Requirements (NFRs)

- Availability: define SLOs and SLI metrics (latency, error rate, uptime). Use Front Door + regional backends for global availability.
- Scalability: ensure stateless services are horizontally scalable; use AKS node pools + HPA and serverless autoscale for Functions.
- Consistency: choose CosmosDB consistency per container (Session/Strong when needed).
- Performance: place caches (Redis) near compute; consider read replicas for read-heavy workloads.
- Cost: model RU/s costs for CosmosDB; factor in Front Door and Gateway egress.

Design Patterns

- API Gateway / BFF: Front Door globally, Application Gateway per region for path-based routing and WAF.
- Backend-for-Frontend: tailor APIs per client (web, mobile) to reduce over-fetching.
- CQRS + Event Sourcing: use Service Bus for commands/events; CosmosDB for read stores.
- Saga pattern: for distributed transactions, implement compensation logic in orchestrator (Durable Functions) or choreographed events.

Data Design

- Data ownership: each microservice owns its own CosmosDB container or partition key.
- Partitioning: choose partition keys that avoid cross-partition transactions and hot partitions.
- Caching: use Azure Cache for Redis for session and hot data.
- Backups: enable continuous backup or point-in-time for critical containers.

Integration & Messaging

- Use Service Bus Topics for fan-out and ordered processing where needed.
- Use Event Grid for serverless event routing and lightweight notifications.
- Avoid tight coupling: version contracts, use feature flags for schema changes.

Security & Identity

- Use Azure AD for user identity; use managed identities for service-to-service auth.
- Limit public access with Private Link for CosmosDB, Redis, and Key Vault.
- Encrypt data at rest and in transit; enable diagnostic logs and monitor access.

Operational Considerations

- Observability: instrument services with tracing (OpenTelemetry), metrics, and logs to Log Analytics/Application Insights.
- CI/CD: use GitOps (ArgoCD) for Kubernetes manifests, GitHub Actions for image builds.
- Runbooks: prepare runbooks for failover, scale incidents, and data corruption.

Trade-offs Table (high level)

- AKS: control, complexity, cost for idle resources.
- Functions: cost efficiency, simpler ops for small workloads, cold-start considerations.
- CosmosDB: global distribution and low latency vs RU cost complexity.

TMS Notes

- Start with clear bounded contexts and contracts.
- Model NFRs early; implement simple prototypes to validate assumptions.
- Keep ADRs for key trade-offs and decisions.
