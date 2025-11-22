GitOps with ArgoCD

Overview

Implement a GitOps-driven deployment pipeline using ArgoCD and Helm to manage AKS workloads and environment configurations.

Architecture Diagram (placeholder)

- diagrams/gitops-argocd.png — GitHub repo (infrastructure + apps) -> ArgoCD in a management cluster -> target AKS clusters.

Tech Stack

- ArgoCD
- Helm charts and Helmfile (optional)
- GitHub Actions for CI
- Azure Container Registry
- Kubernetes RBAC and network policies

Terraform Structure (if applicable)

- modules/
  - argo/
  - aks/
  - helm-charts/

Deployment Workflow

- Developers open PRs to application manifests/Helm charts.
- GitHub Actions builds images and updates image tags in Git (or uses image automation).
- ArgoCD syncs changes into the target clusters and reports status back to GitHub.

Security Considerations

- Use read-only deploy keys for ArgoCD to pull manifests.
- Use RBAC to limit ArgoCD permissions and restrict namespaces.
- Seal secrets with Sealed Secrets or use External Secrets backed by Key Vault.

Cost considerations

- ArgoCD adds management overhead but reduces manual drift; run it in a small management cluster.
