Week 06: Security & Zero Trust

🎯 Objectives

- Apply Zero Trust principles across identity, network, and data layers.
- Harden AKS and serverless workloads and implement least-privilege access.

📚 Learning Topics

- Azure AD, Conditional Access, Privileged Identity Management (PIM).
- Managed Identities for Azure resources.
- Network controls: Private Endpoints, Service Endpoints, and Azure Firewall.
- Runtime security: Microsoft Defender for Cloud, policy enforcement, and Kubernetes security tools.

🛠 Hands-on Tasks (Step-by-step)

1. Identity & Access
   - Configure Azure AD Conditional Access and enable PIM for sensitive roles.

2. Secrets & Key Management
   - Enforce Key Vault for secrets; integrate with AKS and Functions via Managed Identities.

3. Network Isolation
   - Ensure PaaS services use Private Endpoints and traffic flows are restricted by NSGs and Firewall rules.

4. Runtime Protection
   - Enable Microsoft Defender for Cloud and configure policies for Kubernetes and serverless workloads.

5. Policy as Code
   - Implement Azure Policy definitions and assign them to the management group for enforcement.

🏗 Deliverables

- Security baseline module for Terraform.
- Runbooks for incident response and breach simulations.
- Policies and map of controls across the platform.

🔍 Architecture Diagrams (placeholders)

- diagrams/security-zero-trust.png — Identity plane (Azure AD) -> Workload plane (AKS/Functions) -> Data plane (Key Vault, CosmosDB) with Private Links.

📓 Notes & Reflection (TMS perspective)

Security is non-negotiable. Week 06 emphasized shifting left with policy as code and ensuring identities and secrets are tightly controlled to reduce attack surface.
