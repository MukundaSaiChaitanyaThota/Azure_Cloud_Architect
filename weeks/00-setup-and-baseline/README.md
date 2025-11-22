# 00-setup-and-baseline

# Week 00: Setup & Baseline

## 🎯 Objectives

- Create a repeatable local and cloud workspace for the 3-month Azure Cloud Architect journey.
- Establish repository and documentation scaffolding, collaboration tools, and baseline IaC state.
- Validate minimal runnable platform artifacts (AKS + Function App) and centralized observability.
- Capture a personal skills baseline and a learning plan to measure progress.

## 📚 Learning Topics

- Azure account organization, subscriptions, and RBAC basics.
- Azure CLI and authentication models (service principals, managed identities).
- Terraform remote state with Azure Storage and module organization.
- Git workflows, GitHub basics, and GitOps orientation.
- VS Code setup, useful extensions, and diagramming tooling (draw.io, Figma, Mermaid).
- Basic AKS and Azure Functions bootstrapping patterns and observability with Log Analytics.

## 🛠 Hands-on Tasks (copyable steps)

1. Clone repository and inspect structure

```bash
git clone <your-repo-url> azure-cloud-architect
cd azure-cloud-architect
ls -la
```

2. Duplicate repo structure locally for experiments (no overwrite)

```bash
# create a sandbox copy of the repo structure without git history
rsync -a --exclude='.git' . ../azure-cloud-architect-sandbox
cd ../azure-cloud-architect-sandbox
```

3. Create a Notion dashboard (or equivalent) for tracking progress

- Create a workspace called "Azure Cloud Architect — TMS".
- Add pages: Weekly Roadmap, ADRs, Diagrams, Learning Notes, Tasks.
- Link the repo (paste README links) and create a checklist for the first 12 weeks.

4. Create an architecture diagram workspace

- Create a directory in the repo for diagrams and a local workspace in your diagram tool:

```bash
mkdir -p cloud-architect-roadmap/diagrams
# open diagram tool of choice (Figma/draw.io) and save first diagram as:
# cloud-architect-roadmap/diagrams/baseline-vnet-aks-function.png
```

5. Create an automation service principal for CI/CD (store credentials securely)

```bash
az login
az ad sp create-for-rbac --name "tms-cloud-architect-sp" \
  --role Contributor \
  --scopes /subscriptions/<SUBSCRIPTION_ID> \
  --sdk-auth > sp-credentials.json
# Move credentials to a secure place (do not commit)
mv sp-credentials.json ~/.secure/azure/
```

6. Bootstrap Terraform backend (resource group + storage account)

```bash
# create resource group and storage account (example)
az group create -n rg-terraform-backend -l eastus
az storage account create -n tmsbackend$RANDOM -g rg-terraform-backend --sku Standard_LRS
# create blob container for state
az storage container create --name tfstate --account-name <STORAGE_ACCOUNT_NAME>
```

Add backend config to your Terraform config and run:

```bash
terraform init
```

7. Minimal AKS + Function smoke test

- Use a small Terraform module or Azure CLI to deploy a very small AKS cluster and a Function App connected to a Log Analytics workspace to validate connectivity and telemetry.

8. Self-assessment baseline (copy this checklist into your Notion page)

- Azure fundamentals (subscriptions, RBAC): [ ]
- Terraform basics (init, plan, apply): [ ]
- Kubernetes basics (kubectl, pods, services): [ ]
- Basic networking (VNets, NSGs, Private Endpoint): [ ]
- Observability (Log Analytics / Application Insights): [ ]
- CI/CD familiarity (GitHub Actions, ArgoCD): [ ]

Score yourself 1–5 for each and capture strengths & gaps.

## 🏗 Deliverables

- Repository scaffold and sandbox copy for experiments.
- Notion dashboard with the weekly roadmap and checklists.
- `cloud-architect-roadmap/diagrams/baseline-vnet-aks-function.png` placeholder saved locally.
- Service principal credentials stored securely for CI (not in repo).
- Terraform backend (storage account + container) configured and `terraform init` validated.
- Minimal AKS cluster and Function App deployed and sending telemetry to Log Analytics (smoke test).
- Baseline self-assessment saved in Notion or `docs/notes/self-assessment.md` in repo.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/baseline-vnet-aks-function.png` — Front Door -> App Gateway -> AKS + Function App -> Private Endpoints (CosmosDB, Redis)
- `cloud-architect-roadmap/diagrams/system-design-highlevel.png` — High level components and integration points

(Use SVG or PNG; save working files in `cloud-architect-roadmap/diagrams/`.)

## 📓 Notes & Reflection (TMS perspective)

Week 00 is the foundation — not the flashy part, but the most important. For me (TMS), this week is about removing friction: repeatable setups, secure automation credentials, clear documentation, and a place to capture decisions. I treat the initial smoke deployments as experiments to validate assumptions (networking, identity, and telemetry) rather than production-ready systems. Writing a short self-assessment helps me stay honest about where I need to focus during the months ahead.

Tips

- Do not commit credentials; use Key Vault and GitHub Secrets.
- Keep diagrams simple at first; iterate and refine as you implement.
- Automate the backend bootstrap so any teammate can reproduce the environment.

— TMS
