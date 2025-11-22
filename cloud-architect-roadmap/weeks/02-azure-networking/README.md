Week 02: Azure Networking

🎯 Objectives

- Build a deep understanding of Azure networking primitives and design secure, scalable network topologies for hybrid cloud solutions.
- Implement Private Link, VNet peering, Azure Firewall, and Front Door patterns.

📚 Learning Topics

- VNets, subnets, Network Security Groups (NSGs), and route tables.
- Azure Firewall, DDoS Protection, and Web Application Firewall (WAF).
- Private Link and Private Endpoints for PaaS services.
- VNet peering and Hub-and-Spoke topology.
- Azure Front Door and Application Gateway differences and integration.
- Service chaining and forced tunneling to on-premise.

🛠 Hands-on Tasks (Step-by-step)

1. Hub-and-Spoke Design
   - Design a hub VNet with shared services (Azure Firewall, Bastion, DNS) and spoke VNets for applications and data.

2. Private Endpoints & Private Link
   - Create private endpoints for CosmosDB and Azure Cache for Redis and validate traffic flows remain within the VNet.

3. Secure Ingress
   - Deploy Azure Front Door with WAF and integrate with Application Gateway in the hub for regional traffic management.

4. Network Policies & NSGs
   - Implement NSGs for subnet-level controls and test traffic connectivity between AKS node pools and data subnets.

5. Connectivity to On-prem
   - Set up VPN Gateway or ExpressRoute (conceptual) and validate routing for hybrid connectivity.

6. Observability
   - Enable Network Watcher and NSG flow logs; store logs in Log Analytics for analysis.

🏗 Deliverables

- Network design documents and diagrams stored in `cloud-architect-roadmap/diagrams`.
- Private endpoints implemented for key PaaS services (CosmosDB, Redis).
- Sample Terraform module for hub-and-spoke networking.

🔍 Architecture Diagrams (placeholders)

- diagrams/hub-spoke-network.png — Hub (Firewall, Bastion) -> Spokes (AKS, App, Data) with Private Endpoints to CosmosDB/Redis and Front Door in front.

📓 Notes & Reflection (TMS perspective)

Networking is where architecture maturity shows. For TMS, Week 02 emphasized minimizing public exposure using Private Link and clearly separating control and data planes. Hub-and-Spoke with Azure Firewall provides a consistent security posture and simplifies governance.
