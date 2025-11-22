AKS Reference Architecture

Overview

A reference architecture for hosting microservices on Azure Kubernetes Service (AKS) with secure connectivity, ingress, and integration with managed Azure services.

Architecture Diagram (placeholder)

- diagrams/aks-reference-architecture.png — Front Door -> Application Gateway -> AKS (multiple node pools) -> Private Link to CosmosDB and Redis.

Tech Stack

- AKS (Kubernetes)
- Azure Front Door
- Azure Application Gateway
- Azure Container Registry (ACR)
- CosmosDB (multi-region) for globally distributed data
- Azure Cache for Redis for caching
- Azure Monitor / Log Analytics / Application Insights
- Azure AD for identity and RBAC

Terraform Structure (if applicable)

- modules/
  - aks/
  - networking/
  - monitoring/
  - identity/
- environments/
  - dev/
  - prod/

Deployment Workflow

- CI builds container images and pushes to ACR.
- CD uses either Helm + ArgoCD or GitHub Actions to update AKS workloads.
- Ingress is managed via Application Gateway Ingress Controller (AGIC) and Front Door for global routing.

Security Considerations

- Use Managed Identities for AKS to access Key Vault and other services.
- Use Private Endpoints for CosmosDB and Redis.
- Network policies and Azure Firewall or NSGs to limit traffic.

Cost considerations

- Use autoscaling node pools and spot instances for non-critical workloads.
- Use CosmosDB autoscale RU for variable loads.
- Evaluate Front Door vs Application Gateway costs based on global traffic patterns.

Future Enhancements

- Implement multi-tenant patterns, service meshes (e.g., Istio or Linkerd), and advanced observability with distributed tracing.
