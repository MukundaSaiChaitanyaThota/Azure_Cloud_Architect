FinOps & Cost Architecture Notes

Overview

FinOps guidance for managing and optimizing costs in Azure across hybrid microservices and serverless platforms.

Principles

- Measure and attribute costs: use tags, resource groups, and subscriptions to track spend.
- Rightsize continuously: monitor utilization and adjust SKUs or scale settings.
- Automate cost controls: budgets, alerts, and automated remediation for overspend.

Areas of Focus

- Compute
  - AKS: use node autoscaling, spot instances for non-critical workloads, and appropriate VM SKUs.
  - Functions: choose Consumption vs Premium based on scale and VNET requirements.
- Data
  - CosmosDB: tune RU/s and use autoscale; partition design to avoid hot partitions.
  - Storage: lifecycle policies, cool/archive tiers for older data.
- Networking
  - Monitor Front Door and egress costs; use caching and regionalization to reduce egress.

Strategies

- Tagging and chargeback
  - Enforce tagging via policy; map tags to cost centers and projects.
- Reserved/Spot and Savings Plans
  - Use reserved instances or savings plans for predictable workloads and spot for fault-tolerant tasks.
- Cost-aware architecture
  - Push compute to serverless for sporadic workloads and to AKS for steady-state high-throughput.

Tooling & Automation

- Azure Cost Management + Budgets
  - Set budgets at subscription/resource group level and configure alerts & action groups.
- Reporting
  - Build dashboards for showback/chargeback with cost trends, per-service breakdown.
- CI checks
  - Add cost guardrails in CI (e.g., scan ARM/Terraform for large SKU choices, missing tags).

Operational Practices

- Daily/weekly cost reviews for active projects.
- Monthly optimization sprints to apply reserved instance purchases and rightsizing.
- Use anomaly detection to catch unexpected spikes.

TMS Notes

- Track RU consumption per container in CosmosDB; small misconfigurations cause large bills.
- Prefer autoscale for CosmosDB and functions where load varies significantly.
