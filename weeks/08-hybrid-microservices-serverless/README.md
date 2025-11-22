# Week 08: Hybrid Microservices + Serverless

## 🎯 Objectives

- Architect reliable integrations between AKS-hosted microservices and Azure Functions for event-driven workloads.
- Implement resilient messaging, DLQ handling, and idempotent consumer patterns.

## 📚 Learning Topics

- Design boundaries: when to use AKS vs Functions.
- Messaging: Azure Service Bus (topics/subscriptions), Event Grid for event routing.
- Error handling: Dead Letter Queues, poison message handling, retries, and backoff strategies.
- Durable Functions for orchestrations and sagas.

## 🛠 Hands-on Tasks (copyable steps)

1. Build event-driven flows

- Implement an order processing workflow: API → order service (AKS) → Service Bus topic → Functions worker for payment/fulfillment.

2. Configure DLQs and retry policies

- Configure Service Bus retry policies and dead-lettering and implement a process to inspect and repair DLQ messages.

3. Durable Functions

- Create a Durable Function orchestrator for long-running processes with compensation logic.

4. KEDA autoscaling

- Configure KEDA to scale consumers in AKS based on queue length or custom metrics.

## 🏗 Deliverables

- Sample event-driven integration code and deployment manifests.
- DLQ inspection scripts and remediation playbook.
- Durable Functions orchestration sample and tests.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/hybrid-integration.png` — AKS -> Service Bus -> Functions -> Datastores with DLQ paths highlighted.

## 📓 Notes & Reflection (TMS perspective)

Event-driven architecture reduces coupling but increases operational surface. Week 08 was all about making the system observable and resilient: idempotency, clear retry semantics, and robust DLQ processes are essential.

— TMS
