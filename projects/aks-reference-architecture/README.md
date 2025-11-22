# Project: Production-Grade AKS Platform (Microservices)

## Overview

A reference implementation and guidance for running production-grade microservices on Azure Kubernetes Service (AKS). This project documents recommended topology, CI/CD practices, operational controls, and trade-offs for a hybrid microservices + serverless platform. The goal is to provide a repeatable, secure, and observable foundation that teams can adopt and extend.

## Architecture Diagram (placeholder)

- `cloud-architect-roadmap/diagrams/aks-reference-architecture.png` — Front Door -> Application Gateway -> AKS (multiple node pools) -> Private Link to CosmosDB and Redis. Include monitoring, logging and CI/CD flow in the diagram.


## Tech Stack

- AKS (managed Kubernetes)
- Azure Front Door (global routing, WAF)
- Azure Application Gateway (regional ingress, AGIC)
- Azure Container Registry (ACR)
- Azure Cosmos DB (multi-region, Core SQL API)
- Azure Cache for Redis (hot data cache)
- Azure Monitor / Log Analytics / Application Insights
- ArgoCD (GitOps) and Helm charts
- Terraform for infrastructure provisioning
- KEDA for event-driven autoscaling (optional)
- Velero for backups and restores of cluster resources

## Deployment Workflow

1. Code & Build
- Developers push app changes to `apps/<service>` repository or monorepo.
- GitHub Actions build container images, run tests, push images to ACR with immutable tags.

2. Image Promotion
- Use image promotion or immutability pattern: tags include commit SHA and a promoted `stable` tag for release.

3. GitOps
- Application manifests/Helm charts live in `infrastructure/apps` or a GitOps repo.
- ArgoCD monitors the Git repo and reconciles desired state into target clusters.

4. Progressive Delivery
- Use Argo Rollouts for canary or blue/green deployments with automated promotion rules based on SLOs (error rate, latency).

5. Secrets
- Use External Secrets Operator to sync secrets from Azure Key Vault or Sealed Secrets when appropriate. Never store raw secrets in Git.

6. Observability
- Ensure every service emits OpenTelemetry traces and metrics; wire to Application Insights and Prometheus/Grafana dashboards.

## Security Considerations

- Network
  - Use Hub-and-Spoke network topology; place AKS in a spoke with private cluster mode enabled (API server private endpoint).
  - Use Private Endpoints for CosmosDB, Redis and Key Vault; disable public network access where possible.
  - Enforce NSGs and Azure Firewall rules in the hub.

- Identity & Access
  - Use Managed Identities (system or user-assigned) for cluster components and workloads instead of long-lived service principals.
  - Enforce Azure AD integration for Kubernetes API server and RBAC for namespace-level access control.

- Runtime
  - Apply Pod Security Standards (restricted for production namespaces) and network policies (Calico/Azure CNI) to restrict pod-to-pod traffic.
  - Scan container images on push and integrate vulnerability alerts via Microsoft Defender for Container Registries.

- Supply Chain
  - Sign images where possible and enforce admission policies to only allow images from trusted registries/tags.

## Cost Optimization

- Node Pools & Autoscaling
  - Use multiple node pools: system, workloads, and spot (preemptible) node pools for non-critical batch jobs to save cost.
  - Configure Cluster Autoscaler aggressively on non-prod and conservatively on prod depending on required headroom.

- Right-sizing
  - Continuously analyze pod and node utilization; tune requests/limits and choose appropriate VM SKUs.

- Caching & Data
  - Use Azure Cache for Redis for hot data to reduce RU charges on CosmosDB.
  - Use CosmosDB autoscale for unpredictable workloads, and provisioned throughput for stable workloads to optimize costs.

- Environments
  - Schedule non-prod clusters to scale down or shut down during off-hours and use sandbox copies with reduced capacity for development.

## Future Enhancements

- Multi-cluster topology with a dedicated management cluster for platform components (ArgoCD, logging, monitoring) and tenant-specific clusters for workloads.
- Service mesh adoption (e.g., Linkerd for lighter footprint or Istio for richer features) for advanced traffic shaping, mTLS, and telemetry.
- Implement full supply-chain security (Sigstore, image provenance) and OPA/Gatekeeper policies for policy-as-code enforcement.
- Automated cost optimization recommendations and rightsizing via scheduled reports and CI checks.


---

Notes (TMS perspective):

This reference balances operational simplicity and real-world production needs. My approach is pragmatic: automate what causes pain (deployments, rollbacks, backups), instrument everything, and keep security and cost considerations in your CI/CD pipeline. Start with a small, hardened cluster and iterate — add complexity like multi-cluster or service mesh only when you have measurable need.
