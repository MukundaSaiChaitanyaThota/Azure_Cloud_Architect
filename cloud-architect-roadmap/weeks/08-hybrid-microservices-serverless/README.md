Week 08: Hybrid Microservices + Serverless Integration

🎯 Objectives

- Implement integration patterns between AKS-hosted microservices and Azure Functions for event-driven processing.
- Ensure secure, reliable communication and observability across hybrid boundaries.

📚 Learning Topics

- Event-driven patterns: Service Bus, Event Grid, SignalR.
- Orchestration patterns using Durable Functions vs distributed choreography.
- Data consistency across microservices and serverless functions.
- Observability across hybrid stacks with distributed tracing.

🛠 Hands-on Tasks (Step-by-step)

1. Event-driven Integration
   - Build a sample flow where AKS publishes events to Service Bus and Functions process downstream tasks.

2. Durable Operations
   - Implement a Durable Function for long-running orchestration triggered by events.

3. Security & Messaging
   - Use Managed Identities for Service Bus access and enable encryption-in-transit.

4. Tracing & Correlation
   - Implement distributed tracing using Application Insights and OpenTelemetry to follow requests across AKS and Functions.

🏗 Deliverables

- Sample integration code and deployment manifests.
- Tracing examples and SLOs for end-to-end flows.

🔍 Architecture Diagrams (placeholders)

- diagrams/hybrid-integration.png — AKS -> Service Bus -> Functions -> CosmosDB/Redis with tracing annotations.

📓 Notes & Reflection (TMS perspective)

Week 08 reinforced the importance of clear contracts and robust error handling between microservices and serverless components. Observability and correlation IDs save debugging time.
