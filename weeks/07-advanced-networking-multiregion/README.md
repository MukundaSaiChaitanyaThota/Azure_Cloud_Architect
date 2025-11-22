# Week 07: Advanced Networking & Multi-region

## 🎯 Objectives

- Design and validate multi-region topologies, replication strategies, and global traffic management.
- Balance trade-offs for RTO/RPO, consistency, and cost in multi-region deployments.

## 📚 Learning Topics

- Global load balancing: Front Door, Traffic Manager.
- Cross-region replication: CosmosDB multi-region, geo-redundant storage and failover procedures.
- Inter-region connectivity, VNet peering, and gateway transit.
- Cost and operational implications of active-active vs active-passive topologies.

## 🛠 Hands-on Tasks (copyable steps)

1. Plan paired-region deployment

- Pick primary and secondary regions and document network and resource mapping.

2. Configure Front Door with regional backends

```bash
# example: create Front Door and add backend pools for each region with health probes
```

3. Configure CosmosDB multi-region replication

```bash
az cosmosdb create --name mycosmos --resource-group rg -- locations regionName=EastUS failoverPriority=0
az cosmosdb failover-priority-change --resource-group rg --name mycosmos --failover-policies "[{'regionName':'WestUS','failoverPriority':1}]"
```

4. Test failover scenarios

- Simulate regional outage by disabling Backends/health probes and validate Front Door failover.
- Run read/write tests to validate replication and consistency behavior.

5. Document DR and failback runbooks

- Create step-by-step runbooks for failover activation, DNS updates, and data validation.

## 🏗 Deliverables

- Multi-region topology diagram `cloud-architect-roadmap/diagrams/multi-region-topology.png`.
- Terraform examples for region bootstrap and replication for CosmosDB and infrastructure.
- Failover and failback runbooks and test results.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/multi-region-topology.png` — Front Door -> Regional AKS/Functions clusters -> CosmosDB multi-region.

## 📓 Notes & Reflection (TMS perspective)

Multi-region architecture introduces cost and complexity. Week 07 is about measuring what you need: high availability vs low latency, and choosing replication models that meet business needs. Practice failover with drills — runbooks that are never tested are unreliable.

— TMS
