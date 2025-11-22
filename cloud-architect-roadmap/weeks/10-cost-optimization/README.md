Week 10: Cost Optimization

🎯 Objectives

- Learn FinOps principles and apply cost optimization techniques across Azure services.
- Identify cost drivers and implement governance to keep spend predictable.

📚 Learning Topics

- Azure Cost Management and budgeting.
- Rightsizing compute: AKS node pools, reserved instances, and spot instances.
- Serverless pricing considerations for Functions and scaling.
- Storage and database cost optimization: CosmosDB throughput, caching strategies.

🛠 Hands-on Tasks (Step-by-step)

1. Cost Baseline
   - Use Azure Cost Management to establish a baseline and set budgets/alerts.

2. Rightsizing
   - Analyze AKS node utilization and adjust node pool sizes and autoscaler settings.

3. Serverless Cost Patterns
   - Evaluate function cold starts vs. execution costs and consider Premium plan for heavy usage.

4. Storage Optimization
   - Tune CosmosDB throughput and implement TTLs and caching to reduce RUs.

🏗 Deliverables

- Cost optimization report and runbook.
- Budgets, alerts, and recommendations for each environment.

🔍 Architecture Diagrams (placeholders)

- diagrams/cost-optimization.png — Cost drivers map across compute, data, and networking.

📓 Notes & Reflection (TMS perspective)

FinOps is continuous. Week 10 showed that small optimizations compound; tagging, budgets, and automated scaling are low-hanging fruit.
