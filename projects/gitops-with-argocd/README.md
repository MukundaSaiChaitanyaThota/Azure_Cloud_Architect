# GitOps with ArgoCD — Reference Project

## Overview

A hands-on reference for implementing GitOps using ArgoCD, ApplicationSets, and Helm. This project documents recommended repo layouts, multi-environment deployment patterns, and security/RBAC considerations.

---

## Architecture Diagram (placeholder)

- `cloud-architect-roadmap/diagrams/gitops-argocd.png` — GitHub (apps + infra) -> CI -> ACR -> GitOps repo -> ArgoCD (AppSet) -> AKS clusters

---

## Repo Layout Recommendations

- `infrastructure/` — Terraform modules and bootstrapping resources.
- `platform/` — platform-level manifests (ArgoCD, monitoring, operators).
- `apps/` — application Helm charts and environment overlays.
- `gitops/` — ArgoCD Application manifests (clusters/envs), AppSets definitions.

Mono-repo vs Multi-repo Trade-offs
- Mono-repo: Easier cross-ref and atomic changes across infra and apps but potential merge conflicts and permission granularity issues.
- Multi-repo: Clear separation of concerns, finer-grained permissions, but requires coordination across repos for changes that span infra and apps.

---

## ApplicationSets & Multi-env deployments

- Use ApplicationSets with generators (Git, List, Cluster) to create ArgoCD Applications across clusters or environments.
- App-of-Apps pattern: Root application references environment-specific application manifests for better separation.

---

## Secrets & Security

- Use External Secrets Operator with Azure Key Vault as secret store.
- Use sealed secrets for small teams without Key Vault access.
- Limit ArgoCD's service account scope: prefer namespace-scoped ArgoCD instances or use RBAC to limit cluster-level permissions.

---

## Promotion Strategies

- Canary releases using Argo Rollouts + ArgoCD for progressive delivery based on metrics.
- Blue/green via separate environments and traffic cutover at Front Door or DNS level.
- Git branch-based promotion: merge to `staging` then `main` triggers promotions by ArgoCD sync policies.

---

## RBAC & Auditing

- Map GitHub teams to Azure AD groups and configure ArgoCD SSO.
- Use `argocd-rbac-cm` to configure action-level permissions and limit `sync` to operators.
- Enable audit logs for ArgoCD and integrate with Log Analytics for retention and alerting.

---

## Deliverables

- Sample repo layout and a `gitops/` folder with AppSet examples for `dev`, `staging`, and `prod`.
- External Secrets Operator configuration for Key Vault and sample secret manifests.
- Argo Rollouts examples for canary deployment.

---

— TMS
