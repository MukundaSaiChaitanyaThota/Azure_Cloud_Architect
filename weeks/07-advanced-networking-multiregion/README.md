# Week 07: Advanced Networking & Multi-region

## 🎯 Objectives

- Design global routing, replication and failover strategies for multi-region deployments while maintaining private connectivity and security.
- Evaluate active-active vs active-passive patterns and their operational cost.

## 📚 Learning Topics

- Private DNS resolver and cross-region name resolution patterns.
- Azure Firewall hub patterns and centralized inspection.
- Global routing: Azure Front Door, Traffic Manager, and prerequisites for health probes.
- Active-active vs active-passive architectures and their consistency implications.
- CosmosDB multi-region configurations and consistency models.

## 🛠 Hands-on Tasks (copyable steps)

1. Design private DNS solution

- Implement Private DNS zones and conditional forwarding using Azure DNS Private Resolver for cross-region name resolution.

2. Configure Azure Firewall hub

- Deploy Firewall in hub, centralize logging, and configure route tables for spoke egress through the firewall.

3. Set up Front Door with regional backends

- Configure Front Door backend pools pointing to regional Application Gateway endpoints and validate health probe routing.

4. CosmosDB replication test

- Configure CosmosDB with multiple regions and experiment with read replicas and failover priority changes.

5. DR testing

- Simulate regional outage and execute failover runbooks, measure RTO and RPO, and validate data integrity.

## 🏗 Deliverables

- Private DNS architecture and resolver configuration examples.
- Firewall hub Terraform examples and route table configs.
- Front Door config and failover test report.
- CosmosDB multi-region setup with replication and test scripts.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/multi-region-topology.png` — Front Door -> Regional AKS/Functions clusters -> CosmosDB multi-region with firewall hub.

## 📓 Notes & Reflection (TMS perspective)

Multi-region design is trade-off heavy. Week 07 forced me to think about consistency models and how much complexity we are willing to bear for availability. In many cases, active-passive is simpler and cheaper while active-active is justified only for high-traffic, low-latency needs.

— TMS
