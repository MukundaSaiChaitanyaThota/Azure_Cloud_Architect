Hybrid Microservices + Serverless

Overview

A hybrid architecture combining AKS-hosted microservices with Azure Functions for event-driven, short-lived tasks. This design prioritizes cost efficiency, scalability, and separation of concerns.

Architecture Diagram (placeholder)

- diagrams/hybrid-microservices-serverless.png — Front Door -> App Gateway -> AKS services + Serverless Functions reacting to Service Bus events; CosmosDB as primary datastore and Redis for caching.

Tech Stack

- AKS for core microservices
- Azure Functions (Consumption or Premium plan) for event-driven work
- Service Bus for decoupled messaging
- CosmosDB for global low-latency data
- Azure Redis Cache
- Event Grid for event routing

Terraform Structure (if applicable)

- modules/
  - functions/
  - aks/
  - messaging/
  - data/

Deployment Workflow

- CI builds container images and publishes to ACR.
- CD deploys microservices via Helm and serverless code via Azure Functions CI/CD (GitHub Actions).
- Use Feature Flags and Canary releases for safe rollouts.

Security Considerations

- Use Managed Identities for Functions and AKS pods.
- Use Private Endpoints for CosmosDB and Redis.
- Network segmentation to isolate sensitive data workloads.

Cost considerations

- Use Consumption plan for low-throughput Functions to save costs; switch to Premium for VNET integration or high-throughput.
- Consolidate smaller services into Functions where appropriate to reduce cluster footprint.

Future Enhancements

- Explore KEDA for event-driven autoscaling of AKS workloads and Functions.
