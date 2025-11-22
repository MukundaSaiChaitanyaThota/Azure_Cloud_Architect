# Multi-Region Failover — Reference Project

## Overview

Guidance and examples for implementing multi-region failover strategies in Azure. This project covers active-passive and active-active models, data replication strategies, DNS and traffic routing using Azure Front Door, and runbooks to perform failover and failback.

---

## Architecture Diagram (placeholder)

- `cloud-architect-roadmap/diagrams/multi-region-failover.png` — Front Door -> Primary Region (AKS + Functions) -> Secondary Region readiness with replicated CosmosDB; show failover DNS and traffic patterns.

---

## Active-Passive vs Active-Active

- Active-Passive: Primary region handles all traffic; secondary region is warm or passive and promoted on failover. Simpler to implement and consistent for writes.
- Active-Active: Both regions serve traffic with replication (multi-master) where supported. Lower latency and higher availability but requires careful conflict resolution and higher cost.

---

## Data Replication Strategies

- CosmosDB: native multi-region writes or single write with geo-replication depending on consistency needs.
- Blob Storage: use RA-GRS or custom async replication for storage accounts.
- SQL/other RDBMS: consider zone-aware replication or Geo-restore depending on requirements.

---

## Traffic Management

- Use Azure Front Door for global routing and health-probe based failover.
- Configure health probes to detect regional health; prefer application-level probes for accurate readiness.
- DNS TTLs and failover automation: keep TTLs short for quicker failover and use automation to change priorities when necessary.

---

## Runbooks

- Failover runbook: steps to promote secondary region, reconfigure DNS/Front Door, validate data integrity and resume traffic.
- Failback runbook: steps to re-sync data, switch traffic back, and validate that RPO/RTO goals are met.

---

## Testing & Drills

- Regular DR drills: schedule and automate simulated outages, validate runbooks, and capture RTO/RPO metrics.
- Data consistency tests: ensure eventual consistency scenarios are acceptable and document compensating logic when needed.

---

## Deliverables

- Template Front Door configuration and sample health probes.
- CosmosDB replication setup scripts and failover testing scripts.
- Failover and failback runbooks and a checklist for DR drills.

---

— TMS
