# 04-gitops-argocd

# Week 04: GitOps Architecture with ArgoCD + Helm

## 🎯 Objectives

- Define a GitOps architecture that supports multi-environment deployments, safe promotions, and secure secrets management using ArgoCD and Helm.
- Implement repository patterns and ArgoCD constructs (AppSets, App of Apps) for scale.

## 📚 Learning Topics

- Repo patterns: apps repo vs infra repo, mono-repo vs multi-repo trade-offs.
- ArgoCD concepts: Applications, AppSets, App of Apps pattern, and multi-cluster management.
- Helm best practices: chart structure, values files, and umbrella charts.
- Secrets: External Secrets Operator, Sealed Secrets, and Key Vault integration.
- Promotion strategies: image promotion, GitOps promotions via branch/PR, canary and blue/green with Argo Rollouts.
- RBAC and security: restrict ArgoCD's permissions, use read-only deploy keys, and limit namespace scopes.

## 🛠 Hands-on Tasks (copyable steps)

1. Choose repository layout

- Example layout (multi-repo):
  - `infrastructure/` (Terraform, bootstrap)
  - `platform/` (ArgoCD apps, operators)
  - `apps/` (application Helm charts or kustomize overlays)

- Example layout (mono-repo):
  - `clusters/` (cluster manifests)
  - `apps/` (applications in env overlays)
  - `charts/` (helm charts)

2. Install ArgoCD and configure AppSets

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
# Install ArgoCD AppSet controller
kubectl apply -f https://raw.githubusercontent.com/argoproj-labs/applicationset/main/manifests/install.yaml
```

3. Create App of Apps pattern

- Create a root ArgoCD Application that points to a `clusters/` directory which contains Application CRs for each environment.

4. Secrets handling

- Install External Secrets Operator and configure SecretStore pointing to Azure Key Vault.

```bash
helm repo add external-secrets https://charts.external-secrets.io
helm install external-secrets external-secrets/external-secrets -n external-secrets --create-namespace
```

- Create a SecretStore manifest that references Key Vault and an ExternalSecret manifest per secret.

5. Promotion strategies

- Canary with Argo Rollouts: add Rollout CRDs and configure metrics-based automated promotion.
- Blue/green via two parallel environments and DNS switch using Front Door or Traffic Manager.
- Image promotion: CI updates image tags in `apps/` repo and triggers ArgoCD sync (or use Image Updater).

6. RBAC & security

- Create minimal ArgoCD service account with least privilege and configure SSO/OIDC for human access.
- Use `argocd-rbac-cm` to map groups/roles and limit operations per namespace.

## 🏗 Deliverables

- Sample repo layout (mono or multi) with example charts and overlays.
- ArgoCD Application manifests for `dev`, `staging`, and `prod` with AppSet examples.
- External Secrets Operator configuration for Key Vault integration and example secrets.
- Example Argo Rollouts config for canary deployments and automated promotions.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/gitops-argocd-flow.png` — GitHub Actions -> ACR -> apps repo -> ArgoCD (AppSet) -> AKS clusters

## 📓 Notes & Reflection (TMS perspective)

GitOps at scale requires discipline around repository layout and promotion. I prefer a clear separation of concerns: infra and platform in one repo (bootstrapped by Terraform) and applications in another. Secrets are the hardest part to get right — External Secrets Operator backed by Key Vault is my preferred pattern for Azure.

— TMS
