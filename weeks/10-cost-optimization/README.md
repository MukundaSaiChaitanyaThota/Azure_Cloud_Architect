# Week 10: Cost Optimization

## 🎯 Objectives

- Apply FinOps practices to measure, control, and optimize costs across Azure services used by the platform.
- Identify cost drivers and implement automation and governance to manage spend.

## 📚 Learning Topics

- Azure Cost Management, budgets, and alerts.
- Rightsizing: AKS node pools, reserved instances, spot instances, and autoscaling.
- Serverless cost implications: Function consumption vs premium plans.
- Data cost optimization: CosmosDB RU tuning, TTLs, and caching strategies.

## 🛠 Hands-on Tasks (copyable steps)

1. Create a cost baseline and budget

    ```bash
    # create a budget via az cli example
    az consumption budget create --amount 1000 --category Cost --time-grain Monthly --name "dev-budget" --scope /subscriptions/<SUB>
    ```

2. Analyze AKS cost drivers

    - Use Cost Management to identify top spenders and evaluate node pool sizing and use of spot instances.

3. Tune CosmosDB costs

    - Review RU consumption, consider autoscale, and implement TTLs for ephemeral data.

4. Implement tagging and policies

    - Create Azure Policy to enforce tagging and add CI checks to block untagged deployments.

5. Automate cost savings

    - Create scripts or pipelines to shut down non-prod resources on a schedule and to scale down dev environments during off-hours.

## 🏗 Deliverables

- Cost baseline report and recommended actions.
- Automated scripts/workflows for resource scheduling and tagging enforcement.
- A set of cost guardrails implemented via Azure Policy and CI checks.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/cost-optimization.png` — mapping of cost drivers across services and recommended savings actions.

## 📓 Notes & Reflection (TMS perspective)

FinOps is continuous; the biggest gains come from measurement and automation. Week 10 is about building habits: tagging, scheduled automation, and regularly reviewing spend with stakeholders.

— TMS
