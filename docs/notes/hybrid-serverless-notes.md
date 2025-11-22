Hybrid Microservices + Serverless Notes

Overview

Guidance for integrating AKS microservices with Azure Functions and event-driven patterns to build cost-efficient, scalable systems.

When to use Serverless vs AKS

- Serverless (Functions): event-driven workloads, short-lived jobs, sporadic traffic, and integration tasks.
- AKS: complex business logic, long-running processes, fine-grained control over runtime and networking.

Eventing Patterns

- Service Bus
  - Use for ordered processing, durable pub/sub, and worker patterns.
  - Topics + Subscriptions for fan-out and filtering.
- Event Grid
  - Lightweight event routing with push delivery; great for serverless triggers and event-driven integrations.
- Durable Functions
  - Orchestration for long-running workflows and saga implementations.

Security & Connectivity

- Use Managed Identities for Functions and AKS to access Service Bus and Key Vault.
- Secure Service Bus with IP rules and private endpoints.
- Use VNET Integration (Functions Premium) when private connectivity is required.

Observability

- Correlation IDs: propagate across messages and logs to trace requests across AKS and Functions.
- Instrument with OpenTelemetry and export to Application Insights.

Operational Patterns

- Idempotency: design consumers to be idempotent due to at-least-once delivery semantics.
- Dead-lettering: configure DLQs for Service Bus to capture failed messages.
- Retries: exponential backoff and circuit breakers for transient faults.

Cost Considerations

- Serverless reduces baseline cost but be aware of high-frequency invocations and execution duration.
- Consolidate small tasks into Functions to reduce overhead; keep heavy workloads in AKS.

TMS Notes

- Use KEDA to scale AKS consumer workloads based on Service Bus length to balance cost and throughput.
- Design end-to-end SLOs and test failure scenarios that cross AKS and Functions boundaries.
