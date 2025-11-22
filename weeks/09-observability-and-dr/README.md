# 09-observability-and-dr

# Week 09: Observability & Disaster Recovery

## 🎯 Objectives

- Build a unified observability strategy (metrics, logs, traces) and a documented DR plan with measurable RTO/RPO.
- Implement necessary tooling to run chaos experiments and validate resilience.

## 📚 Learning Topics

- Monitoring stack: Prometheus, Grafana, Application Insights, Loki for logs.
- Distributed tracing with OpenTelemetry and correlating logs/traces/metrics.
- Chaos engineering basics and fault injection tools.
- Backup and restore for AKS (Velero) and data stores (CosmosDB PITR / continuous backup).

## 🛠 Hands-on Tasks (copyable steps)

1. Instrument a service with OTEL

- Add OpenTelemetry SDK to app and configure exporter to Application Insights and a collector that forwards to Prometheus.

2. Centralized logging

- Deploy Loki (or use Azure Monitor) and configure Fluent Bit to forward logs from cluster.

3. Prometheus & Grafana

- Deploy Prometheus Operator and Grafana, create dashboards for cluster and application metrics.

4. Chaos testing

- Use tools like Litmus or Chaos Mesh to inject failures (pod kill, network delay) and validate runbooks.

5. DR playbook

- Document backup schedule and perform a restore test for a namespace and a CosmosDB container.

## 🏗 Deliverables

- OTEL instrumentation examples and collector config.
- Dashboards and alerting rules (example in Grafana/Prometheus and Application Insights).
- Chaos test results and DR drill report.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/observability-flow.png` — telemetry flow from apps to collectors and storage, and the DR backup topology.

## 📓 Notes & Reflection (TMS perspective)

Instrumentation is the cost of being able to operate. Week 09 felt like turning on headlights: you can only fix what you can see. Regular DR drills and chaos testing build confidence that runbooks work under pressure.

— TMS
