Week 00: Setup and Baseline

🎯 Objectives

- Establish the workspace, tooling, and baseline knowledge required for a 3-month Azure Cloud Architect journey.
- Install and configure core CLI tools, IDE extensions, and local environment configuration for secure Azure development.
- Create an initial project scaffold for the roadmap and projects.

📚 Learning Topics

- Azure account and subscription hygiene: management groups, subscriptions, and role-based access control (RBAC).
- Azure CLI and Azure PowerShell basics.
- Infrastructure as Code (IaC) fundamentals: Terraform basics and recommended organization for modules.
- Git workflow for collaboration and GitHub repository best practices.
- Local tooling: Visual Studio Code settings, extensions for Terraform, YAML, ARM/Bicep, and Kubernetes.
- Security baseline: Azure AD, service principals,-managed identities, and secrets storage using Key Vault.

🛠 Hands-on Tasks (Step-by-step)

1. Azure Accounts & Subscriptions
   - Sign in to the Azure portal and confirm you have at least one subscription dedicated to experimentation and another for production-like workloads.
   - Set up naming conventions and tags for resource governance (example tags: owner=TMS, project=cloud-architect, environment=dev).

2. CLI & Tooling Installation
   - Install Azure CLI (az) and verify with `az account show`.
   - Install Terraform (>=1.0) and verify with `terraform -v`.
   - Install kubectl and aks-preview (if planning to use AKS features).
   - Configure VS Code with recommended extensions: Azure Account, Azure CLI, Terraform, HashiCorp Vault (if used), Kubernetes, YAML, and GitLens.

3. GitHub Repository Setup
   - Fork or clone the `Azure_Cloud_Architect` repository locally.
   - Configure branch protection on `main` and create a `develop` branch for iterative work.
   - Set up Git hooks (optional) for pre-commit linting and formatting. Use tools like pre-commit and husky for multi-language repos.

4. Authentication & Secrets
   - Create an Azure AD service principal for automation: `az ad sp create-for-rbac --name "tms-cloud-architect-sp" --role Contributor` and securely store its credentials in GitHub Secrets for CI/CD.
   - Create an Azure Key Vault and add a secret for storing non-sensitive IaC state access keys (do not store secrets in git).

5. Terraform Backend & State
   - Create a resource group and storage account to host Terraform state with container and blob lock enabled.
   - Initialize a Terraform workspace locally and verify remote state configuration.

6. Baseline Network & Naming
   - Draft a baseline VNet design (single VNet with subnets for mgmt, apps, data, and AKS nodes). Use addressing that avoids collisions with common on-prem ranges.

7. Minimal AKS & Function App
   - Deploy a minimal AKS cluster with a node pool for Linux workloads and enable RBAC.
   - Create an Azure Function App (Linux, Consumption Plan) and configure a Managed Identity.

8. Observability
   - Create a Log Analytics workspace and connect both AKS and Function App for centralized logs and metrics.

🏗 Deliverables

- Local dev environment with Azure CLI, Terraform, kubectl, and VS Code configured.
- Service principal created for automation and credentials stored in GitHub Secrets.
- Terraform backend storage account and initial state configured.
- Minimal AKS and Function App deployed and connected to a Log Analytics workspace.
- A baseline VNet design document saved in `cloud-architect-roadmap/diagrams`.

🔍 Architecture Diagrams (placeholders)

- diagrams/baseline-vnet-aks-function.png — Diagram showing Front Door -> App Gateway -> AKS + Function App in a VNet with Private Link to CosmosDB and Redis.

Note: The image file is a placeholder; draw the diagram using your preferred tool and save it to `cloud-architect-roadmap/diagrams`.

📓 Notes & Reflection (TMS perspective)

Week 00 is about setting the stage. For me (TMS), this week felt like putting the foundations in place — securing accounts, establishing repeatable processes, and ensuring I have the right tools and guardrails. The goal is to avoid surprises later when we scale microservices and automation. Starting with small, repeatable deployments for AKS and Functions under Terraform helps validate the CI/CD and state management patterns early.

Tips I noted while doing setup:
- Keep secrets out of code; use Key Vault and GitHub Secrets.
- Use separate subscriptions for experimentation and sensitive workloads.
- Use descriptive tags and naming from day one; it pays dividends for FinOps and governance.
- Prefer managed services like Azure Front Door and Private Link for secure and performant connectivity.

Status: Completed baseline tasks and ready to proceed to Week 01 — System Design.
