Week 09: Observability & Disaster Recovery

🎯 Objectives

- Build a comprehensive observability strategy and DR plan across hybrid services.
- Implement monitoring, alerting, and automated runbooks for common failures.

📚 Learning Topics

- Metrics, logs, and traces: OpenTelemetry, Application Insights, Log Analytics.
- Alerting and incident management patterns.
- Backup strategies: snapshots, backups for CosmosDB, and Velero for Kubernetes.
- Runbooks and automated incident response.

🛠 Hands-on Tasks (Step-by-step)

1. Instrumentation
   - Instrument a microservice and a Function with OpenTelemetry and connect to Application Insights.

2. Centralized Logging
   - Configure Log Analytics to collect AKS and Function logs and create useful queries.

3. Alerting & Runbooks
   - Create alert rules with action groups and automated runbooks for common incidents.

4. Backup & Restore
   - Schedule backups for CosmosDB and test Velero restores for AKS workloads.

🏗 Deliverables

- Observability playbook and runbooks for incident scenarios.
- Sample dashboards and alert rules.

🔍 Architecture Diagrams (placeholders)

- diagrams/observability-flow.png — Telemetry pipelines from AKS/Functions -> Log Analytics -> Dashboards/Alerts.

📓 Notes & Reflection (TMS perspective)

Good observability is proactive. Week 09 focused on making systems self-diagnosing as much as possible and validating DR plans regularly.
