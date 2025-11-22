Week 03: AKS Architecture

🎯 Objectives

- Design production-grade AKS clusters optimized for security, scalability, and operational simplicity.
- Understand node pools, pod security policies, ingress controllers, and CI/CD practices for Kubernetes workloads.

📚 Learning Topics

- AKS cluster topology: multiple node pools (system, user, GPU, spot).
- Kubernetes RBAC, Pod Security Standards, and network policies.
- Ingress options: Application Gateway (AGIC), NGINX, and Front Door for global routing.
- Storage options: Azure Disks, Files, and integration with CosmosDB.
- Service meshes and observability with Prometheus + Grafana and Application Insights.

🛠 Hands-on Tasks (Step-by-step)

1. AKS Cluster Bootstrapping
   - Deploy an AKS cluster using Terraform with at least two node pools: system and workloads.

2. Secure the Cluster
   - Enable Azure AD integration and RBAC. Implement Pod Security Standards and network policies.

3. Ingress and Traffic Management
   - Deploy AGIC for Application Gateway integration and test TLS termination with Front Door.

4. CI/CD for Kubernetes
   - Create a sample CI pipeline that builds an image, pushes to ACR, and updates a Helm release via ArgoCD.

5. Autoscaling and Cost Optimization
   - Configure Cluster Autoscaler and Horizontal Pod Autoscaler. Evaluate use of spot nodes for batch workloads.

6. Backup and DR for AKS
   - Configure Velero for cluster workload backups and test restore scenarios.

🏗 Deliverables

- AKS Terraform module with recommended defaults and security features.
- Sample Helm chart and ArgoCD app manifest.
- Runbook for cluster upgrades and node pool maintenance.

🔍 Architecture Diagrams (placeholders)

- diagrams/aks-cluster-architecture.png — AKS cluster with multiple node pools, AGIC, Private Link to CosmosDB and Redis, and monitoring integrations.

📓 Notes & Reflection (TMS perspective)

AKS brings the power of Kubernetes; the challenge is operational maturity. Week 03 was about baking in security and automation upfront — cluster RBAC, managed identities, and automated CI/CD reduce risk and operational burden.
