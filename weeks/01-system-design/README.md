# Week 01: System Design Foundations

## 🎯 Objectives

- Establish core system design principles for building scalable, highly available hybrid microservices + serverless systems on Azure.
- Identify patterns for caching, queuing, data partitioning, and failure isolation.
- Practice translating non-functional requirements (availability, latency, cost) into concrete architecture decisions and trade-offs.

## 📚 Learning Topics

- Scalability patterns: horizontal scaling, sharding/partitioning, CQRS.
- High availability: redundancy, multi-region strategies, health probes and graceful degradation.
- Caching strategies: cache-aside, write-through, TTLs, cache invalidation patterns using Azure Cache for Redis.
- Messaging and queues: when to use Service Bus vs Event Grid, ordering, delivery guarantees, and dead-lettering.
- Consistency models and data design for CosmosDB (partition keys, RU considerations).
- Observability and SLOs: defining SLIs/ SLOs and designing alerting and dashboards.

## 🛠 Hands-on Tasks (copyable steps)

1. Requirements and constraints

- Define functional requirements for a simple e-commerce system (catalog, cart, orders, payments, user profiles).
- Define NFRs: target p95 latency, availability (99.95%), expected throughput (requests/sec), and cost envelope.

2. Design a 3-tier e-commerce architecture (high level)

- Tier 1: Edge & API
  - Azure Front Door for global routing and WAF
  - Application Gateway per region for TLS termination and WAF
- Tier 2: Compute
  - AKS for core microservices (catalog, cart, order) and Azure Functions for asynchronous processing (payment webhook handlers, email notifications)
- Tier 3: Data
  - CosmosDB for operational data (catalog, orders), Azure Cache for Redis for hot reads/session cache, and Azure Blob Storage for assets
- Integration
  - Service Bus topics for order processing and retries; Event Grid for domain events

Sketch the diagram using your tool of choice and save to:

```text
cloud-architect-roadmap/diagrams/system-design-highlevel.png
```

3. Draw sequence and component diagrams

- Create a sequence diagram for checkout flow showing API → cart service → order service → payment processor → Service Bus → fulfillment worker.
- Create a C4-style component diagram that shows bounded contexts and data ownership.

4. Document trade-offs and decisions

- Create an ADR draft capturing the following decisions:
  - Use CosmosDB for globally distributed low-latency reads vs Azure SQL for relational consistency.
  - Use Service Bus for ordered, durable processing vs Event Grid for lightweight publish/subscribe.
  - Use Redis cache to reduce RU consumption on CosmosDB at the cost of cache invalidation complexity.

Save the ADR draft to `docs/architecture-decisions/ADR-<nnn>-system-design.md` and link it from this README.

5. Build a simple prototype (smoke test)

- Create a minimal API service (e.g., Node/Express or .NET minimal API) that exposes a product catalog endpoint.
- Containerize and push to ACR; deploy to AKS using a quick Helm chart or `kubectl apply`.
- Add a simple Function that subscribes to a Service Bus queue/topic to process order events.

Copyable commands (example for a quick local prototype):

```bash
# build and push image
docker build -t <ACR_NAME>.azurecr.io/catalog:0.1 .
docker push <ACR_NAME>.azurecr.io/catalog:0.1

# deploy sample manifest
kubectl apply -f k8s/catalog-deployment.yaml

# create Service Bus topic (az cli)
az servicebus topic create --resource-group <rg> --namespace-name <ns> --name orders
```

6. Define SLOs & Observability

- Define SLIs: request latency p95, error rate, successful checkout rate.
- Create sample Application Insights or OpenTelemetry instrumentation in your prototype and verify traces span across API → AKS → Functions.

## 🏗 Deliverables

- High-level system design diagram saved to `cloud-architect-roadmap/diagrams/system-design-highlevel.png`.
- Sequence diagrams for critical flows (checkout) saved in diagrams/.
- ADR draft documenting major trade-offs and decisions.
- Minimal prototype: a containerized catalog service deployed to AKS and a Function consuming order messages.
- A short report (markdown) listing the chosen SLOs, expected capacity, and how the design meets the requirements.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/system-design-highlevel.png` — Front Door -> App Gateway -> AKS + Functions -> Service Bus -> CosmosDB/Redis
- `cloud-architect-roadmap/diagrams/checkout-sequence.png` — Sequence diagram for checkout flow

(Use SVG or PNG; save working files and exports to `cloud-architect-roadmap/diagrams/`.)

## 📓 Notes & Reflection (TMS perspective)

System design is the exercise of managing trade-offs. For me (TMS), Week 01 is where ideas become constraints — scalability and availability goals force concrete choices about data stores, messaging, and caching. I aim to keep initial designs simple and test assumptions quickly with small prototypes. The 3-tier e-commerce example is a useful sandbox because it touches many architecture concerns: traffic shaping, stateful vs stateless components, and downstream integrations.

Reflections & tips

- Start by quantifying load and latency targets — they change design decisions more than technology preferences.
- Prefer async boundaries for resilience; synchronous calls across services increase blast radius.
- Cache early for read-heavy flows but instrument cache hit/miss rates and plan eviction/invalidation strategies.

— TMS
