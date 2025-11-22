# Week 05: Terraform Modules

## 🎯 Objectives

- Design and implement reusable Terraform modules for the platform: networking, identity, AKS, monitoring.
- Establish CI checks, testing, and versioning practices for modules.

## 📚 Learning Topics

- Module boundaries, inputs/outputs, naming and versioning strategies.
- Remote state backends, state locking, and workspace strategies.
- Testing modules with Terratest and unit testing approaches.
- Publishing modules (private registry vs repository-based consumption).

## 🛠 Hands-on Tasks (copyable steps)

1. Create a module scaffold

```bash
mkdir -p infrastructure/modules/networking
cd infrastructure/modules/networking
# add main.tf, variables.tf, outputs.tf and README.md
```

2. Define inputs/outputs and validation

- Use type constraints and default values for variables and document them.

3. Configure remote backend example

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-backend"
    storage_account_name = "<storage_account>"
    container_name       = "tfstate"
    key                  = "networking.tfstate"
  }
}
```

4. Add CI checks (GitHub Actions)

- Create a workflow to run `terraform fmt`, `terraform validate`, and tflint on PRs.

5. Add Terratest examples

- Write simple Go-based tests that validate the module's provisioning in a test subscription and clean up resources after test.

## 🏗 Deliverables

- Terraform module scaffold for networking and AKS in `infrastructure/modules/`.
- CI workflow for formatting and validation.
- Example Terratest or unit test for the networking module.
- Documentation and example usage for each module.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/terraform-module-structure.png` — module layout and environment overlays

## 📓 Notes & Reflection (TMS perspective)

Modules are the unit of reuse. Week 05 is about making infrastructure consumable and testable. Investment in validation and testing reduces surprises when modules are reused across environments and teams.

— TMS
