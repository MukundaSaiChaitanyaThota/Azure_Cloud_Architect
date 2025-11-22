# 09-observability-and-dr

# Week 09: Observability & Disaster Recovery

## 🎯 Objectives

- Build a comprehensive observability strategy and a tested disaster recovery plan for hybrid services.
- Ensure telemetry, alerting, and runbooks are in place to detect and recover from incidents.

## 📚 Learning Topics

- Instrumentation: metrics, logs, traces (OpenTelemetry, Application Insights, Prometheus).
- Centralized logging and queries with Log Analytics.
- Alerting and incident management: action groups, runbooks, automated remediation.
- Backup strategies: Velero for Kubernetes, continuous backup for CosmosDB, and Storage replication.
- DR testing and drills.

## 🛠 Hands-on Tasks (copyable steps)

1. Instrument a sample service with OpenTelemetry

- Add OpenTelemetry SDK to a sample microservice and export to Application Insights or a collector.

2. Configure Log Analytics and dashboards

```bash
az monitor log-analytics workspace create -g rg-monitor -n la-obs
```

- Create sample queries and dashboards for service latency and error rates.

3. Alerts and runbooks

- Create alert rules (e.g., frontend 5xx rate > 1%) and attach action groups that trigger runbooks (automation runbooks or Logic Apps).

4. Backups & DR

- Configure Velero for AKS and test restore of a namespace and persistent volumes.
- Enable continuous backup/PITR for CosmosDB and test container-level restore.

5. DR drills

- Schedule and run a DR drill: simulate region outage, validate failover to secondary region, and measure RTO/RPO.

## 🏗 Deliverables

- Observability playbook and example dashboards.
- Alerting rules and runbooks for common incidents.
- Backup/restore playbook and DR drill report.
- Sample traces showing end-to-end flows across AKS and Functions.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/observability-flow.png` — Telemetry pipelines from AKS/Functions -> Log Analytics -> Dashboards/Alerts.

## 📓 Notes & Reflection (TMS perspective)

Observability is the muscle that lets you operate at scale. Week 09 is focused on making systems self-diagnosing and validating recovery procedures. Run real drills — they surface configuration and documentation gaps.

— TMS
