# AKS Reference Architecture — Production-Grade Microservices Platform

## Overview

This project contains a reference architecture, best-practices, and example artifacts for running production-grade microservices on Azure Kubernetes Service (AKS). It is intended as a starting point for platform teams to adopt a secure, observable, and cost-effective cluster topology and deployment workflow.

Key goals:
- Reliable deployments with GitOps and progressive delivery
- Strong security posture with private networking and managed identities
- Observability across services and platform components
- Cost control and autoscaling primitives

---

## Architecture Diagram (placeholder)

- `cloud-architect-roadmap/diagrams/aks-reference-architecture.png` — Front Door -> App Gateway (WAF) -> AKS (multiple node pools: system, user, spot) -> Private Endpoints to CosmosDB, Redis, Key Vault; ArgoCD and CI pipeline components shown.

Include monitoring (Prometheus/Grafana, Application Insights), logging flow to Log Analytics, and backup/Velero components.

---

## Design Overview

Components:
- Edge: Azure Front Door for global routing and DDoS protection; regional Application Gateway for WAF and TLS termination.
- Platform: AKS clusters (single or multi-cluster strategy), ArgoCD for GitOps, ACR for images, Terraform for infra.
- Data plane: Cosmos DB (multi-region), Azure Cache for Redis, Blob Storage.
- Observability: OpenTelemetry -> Application Insights / Prometheus -> Grafana; Log Analytics for centralized logs.

### Deployment topology
- Platform (management) cluster runs ArgoCD, observability stack, and platform operators.
- Workload clusters host customer microservices; clusters can be per team or per environment depending on tenancy and isolation needs.

---

## Design Trade-offs

1. Azure CNI vs Kubenet
- Azure CNI (preferred): simplifies Private Endpoint connectivity and network policies but consumes IP space from VNet. Requires careful IP planning.
- Kubenet: conserves IPs but complicates routing and integration with Azure platform services.

2. Single cluster vs Multi-cluster
- Single cluster: simpler to operate initially, cheaper, but risks noisy neighbor issues and blast radius.
- Multi-cluster: stronger isolation and easier upgrades per-tenant but increases operational overhead.

3. AGIC vs NGINX + Front Door
- AGIC + App Gateway: tighter Azure integration, WAF capabilities, simpler TLS offload with native features.
- NGINX Ingress + Front Door: more flexible and portable; requires managing WAF at Front Door or custom WAF rules.

4. GitOps (ArgoCD) model
- App of Apps and ApplicationSets provide scale; enforcement of declarative state reduces drift but requires strict repo discipline.

---

## Deployment Workflow

1. Development
- Developers push code to `apps/<service>`; CI builds images and runs unit/integration tests.

2. Image registry and promotions
- CI pushes images to ACR with immutable tags (commit SHA). Promotion tags (`canary`, `stable`) created by release workflows.

3. GitOps
- Application manifests/Helm charts live in a Git repo. ArgoCD (AppSet or App-of-Apps) monitors the repo and reconciles clusters.

4. Progressive delivery
- Use Argo Rollouts for canary deployments, automated promotion based on metrics and synthetic checks.

5. Secrets
- Use External Secrets Operator to sync secrets from Azure Key Vault into Kubernetes Secrets; avoid storing secrets in Git.

6. Observability and verification
- CI/CD pipelines run smoke tests and verify metrics/logs after deployment; ArgoCD health checks and alerts gate promotion.

---

## Security Considerations

Network:
- Hub-and-Spoke topology with AKS in spoke VNets. Private cluster API server and private endpoints for PaaS resources.
- Azure Firewall in hub for centralized egress control; UDRs force egress through firewall when required.

Identity:
- Managed Identities (user-assigned) for cluster control plane and workload identities for pods (AAD Workload Identity) to access Azure resources.
- Enable Azure AD integration for Kubernetes API server and enforce RBAC roles per namespace.

Runtime:
- Pod Security Standards (restricted) applied to production namespaces.
- Network Policies (Calico or Azure native) to restrict pod-to-pod communication.
- Image scanning on push to ACR and Defender for Containers enabled.

Supply chain:
- Use signed images when possible, admission controllers to enforce image provenance, and OPA/Gatekeeper policies for policy-as-code enforcement.

---

## Cost Optimization

- Node pools: separate system, user, and spot/low-priority node pools. Use spot for non-critical batch workloads.
- Autoscaling: Cluster Autoscaler and HPA to scale nodes and pods respectively. Use KEDA for event-driven scaling.
- Rightsizing: monitor resource requests/limits and tune VM SKUs based on utilization trends.
- Non-prod cost controls: schedule shutdown or reduced size for non-prod clusters and use sandbox tiers for dev.

---

## Scaling & Resilience

- Use availability zones and multiple node pools to tolerate AZ failures.
- Multi-region cluster strategy for global presence and disaster recovery; pair with Cosmos DB multi-region writes when needed.
- Implement health probes at Front Door and Application Gateway; automate failover plans and runbooks.

---

## Example Terraform & Helm Structure (recommended)

- `infrastructure/modules/aks` — module to create AKS cluster with configurable node pools, network plugin, and addons.
- `infrastructure/environments/{dev,staging,prod}` — environment overlays referencing modules.
- `charts/<service>` — Helm charts for each microservice.
- `gitops/apps/{env}/{app}` — ArgoCD app manifests and values files.

---

## Future Enhancements

- Adopt a lightweight service mesh (Linkerd) for observability and mTLS if team needs L7 controls.
- Implement image signing and Sigstore integration for stronger supply-chain guarantees.
- Add automated chaos experiments and continuous DR drills integrated into pipelines.

---

## Notes & TMS Reflection

This reference balances Azure-native convenience with operational rigor. My pragmatic rule: automate repeatable tasks early (network, secrets, CI/CD), instrument everything, and postpone adding complexity (mesh, multi-cluster) until the metrics show a need.

— TMS
