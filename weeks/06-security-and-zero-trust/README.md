# Week 06: Security & Zero Trust

## 🎯 Objectives

- Apply Zero Trust principles across identity, network, and data layers for the platform.
- Harden AKS and serverless workloads with least-privilege access and runtime protections.

## 📚 Learning Topics

- Azure AD, Conditional Access, Privileged Identity Management (PIM), and role-based access control.
- Managed identities for resource access, Key Vault patterns, and secrets lifecycle.
- Network isolation and Private Endpoints, Azure Firewall, NSGs, and WAF policies.
- Runtime security: Microsoft Defender for Cloud, vulnerability scanning, and Kubernetes security tooling.
- Policy as code: Azure Policy, Gatekeeper (OPA), and enforcement pipelines.

## 🛠 Hands-on Tasks (copyable steps)

1. Configure Azure AD and PIM

- Review tenant roles and set up PIM for subscription-level privileged roles.

2. Enforce Key Vault for secrets

- Create an Azure Key Vault and configure RBAC/policies to ensure apps use Key Vault instead of local secrets.

3. Use Managed Identities

- Enable system-assigned or user-assigned managed identities for AKS and Function Apps and grant least-privilege access to resources.

4. Harden network and ingress

- Implement Private Endpoints, set NSGs with deny-by-default rules and deploy WAF policies on Application Gateway.

5. Implement policy as code

- Create and assign Azure Policy definitions to enforce tag requirements, restricted VM sizes, and Key Vault usage.

6. Runtime protections

- Enable Microsoft Defender for Cloud and integrate vulnerability scanning for container images and registry.

## 🏗 Deliverables

- Security baseline Terraform module (identity, Key Vault, basic policy definitions).
- Runbooks for responding to security incidents and remediation playbooks.
- A documented policy set assigned at the management group or subscription level.
- Checklist for hardening AKS and Functions.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/security-zero-trust.png` — Identity plane -> Workload plane -> Data plane with private endpoints and firewalls.

## 📓 Notes & Reflection (TMS perspective)

Security is a continuous process. Week 06 is where policies, identities, and runtime protections should be automated and audited. I focus on removing secret sprawl, enforcing managed identities, and ensuring least-privilege access across the platform.

— TMS
