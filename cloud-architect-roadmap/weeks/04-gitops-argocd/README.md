Week 04: GitOps & ArgoCD

🎯 Objectives

- Implement GitOps principles using ArgoCD to manage Kubernetes application deployments and environment drift.
- Create a reproducible workflow integrating CI (image build) and CD (ArgoCD sync).

📚 Learning Topics

- GitOps concepts and benefits.
- ArgoCD architecture and operational patterns.
- Helm chart design and progressive delivery strategies.
- Secrets management in GitOps (External Secrets, Sealed Secrets, Key Vault provider).

🛠 Hands-on Tasks (Step-by-step)

1. ArgoCD Installation
   - Deploy ArgoCD to a management AKS cluster and secure access via OIDC or Azure AD.

2. Repository Structure
   - Design a mono-repo or multi-repo layout that separates infrastructure, platform, and application manifests.

3. CI Integration
   - Configure GitHub Actions to build images, push to ACR, and update Helm values or image tags in the Git repo.

4. Progressive Delivery
   - Implement canary or blue/green deployments using Argo Rollouts or Helm hooks.

5. Secrets
   - Integrate External Secrets Operator with Azure Key Vault or use Sealed Secrets for encrypted manifests.

🏗 Deliverables

- ArgoCD app manifests and example Helm charts.
- CI workflow snippets for image builds and Git updates.
- Runbook for GitOps troubleshooting and recoveries.

🔍 Architecture Diagrams (placeholders)

- diagrams/gitops-argocd-flow.png — GitHub Actions -> ACR -> Git repo -> ArgoCD -> AKS.

📓 Notes & Reflection (TMS perspective)

GitOps enforces declarative, auditable deployments. Week 04 was about trusting Git as the source of truth and automating safe releases while ensuring secrets are handled securely.
