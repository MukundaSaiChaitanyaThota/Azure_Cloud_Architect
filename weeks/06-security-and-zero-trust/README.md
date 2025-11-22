# Week 06: Security & Zero Trust

## 🎯 Objectives

- Implement Zero Trust controls across identity, network, and data planes for the platform.
- Automate policy enforcement, secrets management, and workload identity.

## 📚 Learning Topics

- IAM & RBAC: Azure AD apps, groups, role assignments, and subscription boundaries.
- Managed Identities (system & user-assigned) and workload identity for AKS.
- Key Vault: private endpoints, access policies vs RBAC, secret rotation patterns.
- Azure Policy & initiative definitions for guardrails (tagging, SKUs, resource types).
- Defender for Cloud (runtime protection), vulnerability scanning, and security alerts.

## 🛠 Hands-on Tasks (copyable steps)

1. Set up Managed Identities and AAD roles

- Create user-assigned managed identities and grant least-privilege roles to resources.

2. Configure Key Vault with Private Endpoint

```bash
az keyvault create -n tms-kv -g rg-security -l eastus
az network private-endpoint create -g rg-security -n pe-kv --vnet-name hub-vnet --subnet private-endpoints --private-connection-resource-id /subscriptions/<sub>/resourceGroups/<rg>/providers/Microsoft.KeyVault/vaults/tms-kv --group-ids vault
```

3. Enable workload identity for AKS

- Use Azure AD workload identity or the AAD Pod Identity project to provision identities for pods.

4. Implement Azure Policy

- Create initiative definitions for required tags, allowed SKUs, and Key Vault usage, and assign at the management group.

5. Enable Defender for Cloud and container scanning

- Enable Microsoft Defender for Cloud at subscription scope and configure recommendations and alerts.

## 🏗 Deliverables

- Managed identity templates and guidance for granting least-privilege access.
- Key Vault configuration with private endpoint examples.
- Azure Policy initiative sample and deployment instructions.
- Defender for Cloud enablement checklist and sample alert runbook.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/security-zero-trust.png` — identity plane -> workload plane -> data plane with Key Vault private endpoints and policy enforcement.

## 📓 Notes & Reflection (TMS perspective)

Zero Trust is less about a single product and more about assumptions: never trust by default. Week 06 was about building guardrails and automation so the platform enforces security at scale. Workload identity reduces secret handling and is a large security win.

— TMS
