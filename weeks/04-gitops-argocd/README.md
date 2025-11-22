# 04-gitops-argocd

# Week 04: GitOps & ArgoCD

## 🎯 Objectives

- Adopt GitOps principles for Kubernetes deployments and platform configuration.
- Implement a reproducible pipeline that connects CI-built artifacts to declarative manifests under Git control and synchronizes via ArgoCD.

## 📚 Learning Topics

- GitOps fundamentals and benefits (auditability, declarative state, drift correction).
- ArgoCD architecture, app-of-apps patterns, and multi-cluster management.
- Helm chart best practices, values files, and immutable tags.
- Secrets management in GitOps (External Secrets, Sealed Secrets, Key Vault integration).
- Progressive delivery: canary, blue/green, and Argo Rollouts.

## 🛠 Hands-on Tasks (copyable steps)

1. Install ArgoCD in a management cluster

    ```bash
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

2. Create repository layout

    - Choose mono-repo or multi-repo patterns and create folders: `infrastructure/`, `platform/`, `apps/`, `helm-charts/`.

3. Configure ArgoCD app-of-apps

    - Create an ArgoCD Application manifest that points to the `apps/` directory to manage application deployments across clusters.

4. CI integration example

    - Create a GitHub Actions workflow that builds container images, pushes to ACR, and updates Helm values or image tags in the Git repo (or use image automation with ArgoCD Image Updater).

5. Secrets handling

    - Integrate External Secrets Operator with Azure Key Vault: install the operator and configure SecretStore CR that references Key Vault secrets.

6. Progressive delivery

    - Add Argo Rollouts to a sample app and configure a basic canary strategy with automated promotion based on success metrics.

## 🏗 Deliverables

- ArgoCD manifests for app-of-apps pattern and one sample Application.
- Sample GitHub Actions workflow for image build → repo update.
- Example Helm chart with values and Argo Rollout config for progressive delivery.
- Secrets integration example using External Secrets Operator with Key Vault.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/gitops-argocd-flow.png` — GitHub Actions -> ACR -> Git repo -> ArgoCD -> AKS

## 📓 Notes & Reflection (TMS perspective)

GitOps is a force-multiplier for operations: it makes deployments auditable and reduces manual intervention. Week 04 was about building the trust loop — CI produces artifacts, Git stores the desired state, and ArgoCD continuously reconciles actual state. Secrets and progressive delivery are the trickiest parts; invest time in a secure secrets workflow and robust rollout metrics.

— TMS
