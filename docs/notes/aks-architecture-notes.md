AKS Architecture Notes

Overview

Operational and design notes for building production-grade AKS clusters integrated with Azure platform services.

Cluster Topology

- Multiple node pools: system, user/workloads, spot/low-priority, GPU.
- Availability zones: enable for control plane and node pools where supported.
- Node sizing: separate critical control-plane workloads from batch/ephemeral.

Security

- Azure AD integration for Kubernetes RBAC.
- Pod Security Standards (PSS)/PodSecurityPolicy alternatives.
- Use network policies (Calico/ Azure CNI) to control pod-to-pod traffic.
- Scan images using Container Registry vulnerability scanning and Microsoft Defender for Containers.

Networking

- Azure CNI vs Kubenet: choose Azure CNI for simpler VNet integration; Kubenet conserves IPs but requires more routing.
- AGIC (Application Gateway Ingress Controller) vs NGINX: AGIC integrates with Application Gateway for WAF and TLS offload; NGINX is flexible and lightweight.
- Use Private Cluster mode and private endpoints for API server access in sensitive environments.

Storage

- Use Managed Disks for persistent volumes; consider Azure Files for shared volumes.
- Avoid storing critical state in pods; rely on external datastores (CosmosDB, SQL, Storage Accounts).

Observability

- Integrate Azure Monitor (container insights) and Application Insights for telemetry.
- Use Prometheus + Grafana for Kubernetes-native metrics and custom alerts.

CI/CD & GitOps

- Use ArgoCD to manage manifests and Helm charts.
- Use GitHub Actions for image builds and push to ACR.
- Implement image promotion pipeline and immutable tags.

Autoscaling & Cost

- Cluster Autoscaler for node scaling; HPA for workload scaling; KEDA for event-driven autoscale.
- Use spot node pools for non-critical workloads to save cost.

Backup & DR

- Use Velero for backup/restore of cluster manifests and persistent volumes.
- Test cluster restore as part of DR exercises.

TMS Notes

- Start with a private cluster for production-like testing.
- Bake in security (RBAC, network policies) early — retrofitting is painful.
