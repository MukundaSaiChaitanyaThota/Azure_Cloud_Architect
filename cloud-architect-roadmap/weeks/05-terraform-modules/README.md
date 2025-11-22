Week 05: Terraform Modules

🎯 Objectives

- Build reusable, well-tested Terraform modules for common platform components (networking, identity, AKS, monitoring).
- Establish testing and CI for Terraform with linters and unit/integration tests.

📚 Learning Topics

- Module design patterns: inputs/outputs, naming, and versioning.
- State management: remote state, locking, and workspaces.
- Testing: terratest, kitchen-terraform, and unit testing strategies.
- Registry vs. private module strategy and semantic versioning.

🛠 Hands-on Tasks (Step-by-step)

1. Create a Module Scaffold
   - Build a networking module with configurable VNet, subnets, and NSGs.

2. Inputs/Outputs and Validation
   - Implement input validation using Terraform 0.15+ features and document all variables.

3. Remote State
   - Configure Terraform backend in Azure Storage with proper access controls and blob lock.

4. CI for Terraform
   - Create a GitHub Actions workflow that runs `terraform fmt`, `terraform validate`, and `tflint` on PRs.

5. Testing Modules
   - Write Terratest suites for the networking module and run them in CI.

🏗 Deliverables

- Reusable Terraform modules in `projects/terraform-modules` or `infrastructure/modules`.
- CI workflows for Terraform linting and validation.
- Test suites and example usage for each module.

🔍 Architecture Diagrams (placeholders)

- diagrams/terraform-module-structure.png — Example module layout and environment overlays.

📓 Notes & Reflection (TMS perspective)

Modules are the unit of reuse. Week 05 was about making infrastructure repeatable and testable — invest early in testing to avoid surprises when modules are consumed across environments.
