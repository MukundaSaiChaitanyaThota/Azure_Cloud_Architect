# Week 05: Terraform Modules — Design & Pipelines

## 🎯 Objectives

- Design a modular Terraform layout for the platform and implement pipelines to validate and test modules.
- Decide on state management, locking, and promotion strategies for module versions.

## 📚 Learning Topics

- Module structure: root modules vs reusable modules, inputs/outputs, and semantic versioning.
- Remote state: Azure Storage backend, state locking, and workspace isolation.
- Pipelines: GitHub Actions for `fmt`, `validate`, `plan` and policy checks (tflint, checkov).
- Terragrunt vs native Terraform: trade-offs for DRY, environment overlays, and locking.
- Building reusable modules (VNet, AKS, Application Gateway) with proper documentation and examples.

## 🛠 Hands-on Tasks (copyable steps)

1. Create module folder structure

```
infrastructure/modules/
  networking/
  aks/
  monitoring/
  identity/

infrastructure/environments/
  dev/
  staging/
  prod/
```

2. Remote backend example (azurerm)

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-backend"
    storage_account_name = "<storage_account>"
    container_name       = "tfstate"
    key                  = "env-prod.tfstate"
  }
}
```

3. CI pipeline example (GitHub Actions)

- Workflow steps:
  - Run `terraform fmt -check`.
  - Run `terraform init` and `terraform validate`.
  - Run `tflint` and `checkov` security scans.
  - Run `terraform plan` and upload plan as an artifact for review.

4. Terragrunt vs Terraform

- Terragrunt simplifies environment overlays and DRY patterns but introduces an additional toolchain; native Terraform with well-designed modules and `terraform workspaces` can suffice for many teams.

5. Testing modules

- Implement Terratest (Go) or Kitchen-Terraform tests to validate module behavior in an ephemeral test subscription.

## 🏗 Deliverables

- Reusable modules for `networking`, `aks`, `appgateway` with clear variable docs and examples.
- CI workflows for module validation, plan generation and security checks.
- Example Terratest suites and instructions to run tests locally or in CI.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/terraform-module-structure.png` — module layout and environment overlays.

## 📓 Notes & Reflection (TMS perspective)

I prefer small, focused modules that do one thing well. Terragrunt is useful when you have many environments and repetitive wiring, but it adds complexity. Invest in CI early — automated plan checks and linting prevent many mistakes.

— TMS
