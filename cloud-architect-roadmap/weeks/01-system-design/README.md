Week 01: System Design

🎯 Objectives

- Define high-level system architecture patterns for hybrid microservices + serverless on Azure.
- Create component diagrams and define integration points, data stores, and security boundaries.
- Translate non-functional requirements (availability, scalability, security, cost) into architecture decisions.

📚 Learning Topics

- Hybrid architectures: when to use AKS vs Azure Functions.
- Design patterns: API Gateway, Backend-for-Frontend, CQRS, Saga for distributed transactions.
- Data partitioning and consistency models in CosmosDB and Redis caching patterns.
- Network segmentation, Private Link, and secure service communication.
- Patterns for resilience and failover across regions.

🛠 Hands-on Tasks (Step-by-step)

1. Requirements Gathering
   - List functional and non-functional requirements for a sample e-commerce microservice platform.

2. High-level Component Diagram
   - Draft a diagram that includes Azure Front Door, Application Gateway, AKS (microservices), Azure Functions (serverless backends), Service Bus for async integration, CosmosDB for global distributed data, and Redis for caching.

3. Define Data Ownership
   - Assign data ownership per bounded context and choose consistency models for CosmosDB containers.

4. Integration & Messaging
   - Design async flows with Service Bus topics/subscriptions and event-driven boundaries.

5. Security & Identity
   - Define the use of Managed Identities, Azure AD B2C for external users, and Private Endpoints for data stores.

6. Observability & SLOs
   - Define service level objectives, logging strategy with Log Analytics, and tracing with Application Insights.

🏗 Deliverables

- System Design README with diagrams and decision notes.
- Component interaction diagrams saved to `cloud-architect-roadmap/diagrams`.
- A list of ADRs to capture key design decisions.

🔍 Architecture Diagrams (placeholders)

- diagrams/system-design-highlevel.png — Front Door -> App Gateway -> AKS + Functions -> Service Bus -> CosmosDB/Redis.

📓 Notes & Reflection (TMS perspective)

Design is the bridge between requirements and implementation. For TMS, Week 01 was about choosing the right tool for each problem: AKS for long-running, resource-intensive services requiring control, and Functions for ephemeral, event-driven tasks. Emphasize clear contracts and async boundaries to reduce coupling and improve resiliency.
