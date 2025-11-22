# Week 08: Hybrid Microservices + Serverless Integration

## 🎯 Objectives

- Implement integration patterns between AKS-hosted microservices and Azure Functions for event-driven processing.
- Ensure secure, observable, and resilient communication across hybrid boundaries.

## 📚 Learning Topics

- Event-driven architectures: Service Bus, Event Grid, Durable Functions.
- Orchestration vs choreography for long-running processes and sagas.
- Connectivity patterns: private endpoints, managed identities, and VNET integration for Functions.
- Observability and tracing across AKS and Functions with OpenTelemetry.

## 🛠 Hands-on Tasks (copyable steps)

1. Implement event-driven checkout flow

- AKS publishes an order created event to Service Bus Topic; Functions subscribe to process payment and trigger fulfillment.

2. Deploy Durable Functions for orchestration

- Implement a Durable Function orchestrator for payment and fulfillment retries and compensation logic.

3. Secure messaging and identity

- Grant managed identities to AKS pods and Function apps to send/receive messages from Service Bus and access Key Vault.

4. Use KEDA to autoscale consumers

- Install KEDA in AKS and configure scaling rules based on Service Bus queue length for efficient resource usage.

5. Implement tracing and correlation

- Propagate correlation IDs in messages and instrument services to capture traces across the end-to-end flow.

## 🏗 Deliverables

- Sample event-driven integration code and deployment manifests.
- Durable Function orchestration sample for long-running operations.
- KEDA autoscaling examples and deployment manifests.
- Tracing examples (OpenTelemetry) that show end-to-end correlation.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/hybrid-integration.png` — AKS -> Service Bus -> Functions -> CosmosDB/Redis with tracing annotations.

## 📓 Notes & Reflection (TMS perspective)

Hybrid patterns let you choose the right compute model for each workload. Week 08 is usually when integration complexity becomes visible — design for idempotency, retries, and observable correlation. KEDA is a cheap win to keep costs aligned with throughput.

— TMS
