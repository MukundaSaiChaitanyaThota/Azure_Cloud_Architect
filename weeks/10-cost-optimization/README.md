# Week 10: Cost Optimization (FinOps)

## 🎯 Objectives

- Implement processes to measure, manage, and optimize cloud spend across environments using FinOps principles.
- Instrument and automate cost-saving measures.

## 📚 Learning Topics

- Spot instances, reserved instances, and savings plans.
- Cost allocation: tagging, chargeback/showback, and mapping costs to teams/projects.
- Autoscaling strategies and scheduled scaling for non-prod environments.
- Measuring cost per environment and per service.

## 🛠 Hands-on Tasks (copyable steps)

1. Create cost baseline

- Use Azure Cost Management to generate a baseline report and identify top spenders.

2. Implement tagging & Azure Policy

- Enforce tags via Azure Policy and add CI checks for missing tags in IaC.

3. Schedule non-prod shutdowns

- Create automation (Azure Automation or GitHub Actions) to stop non-prod clusters during off-hours.

4. Analyze workload suitability for spot VMs

- Move batch and non-critical workloads to spot node pools and validate preemption handling.

5. Build cost dashboards

- Create dashboards showing daily/weekly/monthly spend, and cost per service/namespace.

## 🏗 Deliverables

- Cost baseline report and recommendations.
- Automated scheduling scripts and policy enforcement for tagging.
- Dashboards for cost per environment and alerts for anomalies.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/cost-optimization.png` — cost map highlighting compute, data, networking spend.

## 📓 Notes & Reflection (TMS perspective)

Cost optimization is iterative: measure → act → measure again. Week 10 focused on low-effort automation (scheduling, tags) that yields immediate wins while planning for structural changes (reserved instances) in the medium term.

— TMS
