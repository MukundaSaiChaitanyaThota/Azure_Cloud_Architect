Azure Networking Notes

Overview

Reference guide for designing secure, scalable Azure network topologies for hybrid microservices and serverless deployments.

Core Concepts

- VNets and subnets: logical isolation; use separate subnets for AKS nodes, platform services, and data.
- NSGs: subnet- or NIC-level controls; prefer deny-by-default rules.
- Route Tables: customize traffic flows; use UDRs to force traffic through Azure Firewall or NVA when needed.
- Azure Firewall: centralized policy enforcement for outbound/ingress; integrates with Threat Intelligence.
- Private Link & Private Endpoints: provide private connectivity for PaaS services like CosmosDB and Redis.
- VNet peering vs. Hub-and-Spoke: prefer hub for shared services (firewall, bastion, DNS).

Design Patterns

- Hub-and-Spoke
  - Hub: Firewall, Bastion, DNS, Shared services
  - Spokes: AKS, App, Data
  - Advantages: central control plane and policy enforcement

- Private Service Communication
  - Use Private Endpoints for CosmosDB, Redis, Key Vault.
  - Block public network access and use service endpoints where Private Link is not available.

- Ingress
  - Use Front Door for global routing and DDoS protection at the CDN edge.
  - Use Application Gateway (AGIC) per region for WAF and path-based routing into AKS.

Connectivity

- VPN Gateway vs ExpressRoute
  - ExpressRoute for high-throughput, low-latency on-prem connectivity; VPN for lighter workloads.
- Forced tunneling
  - Use UDRs to route egress through firewall or on-prem when necessary.

Security

- NSG logging & Network Watcher
  - Capture NSG flow logs in Log Analytics for audit and troubleshooting.
- DDoS Protection Standard
  - Enable for production front-ends.
- Private DNS and name resolution
  - Use private DNS zones and link VNets for consistent resolution of private endpoints.

Operational

- Monitor using Network Watcher, Firewall logs, and Azure Sentinel / Defender for networking anomalies.
- Document IP addressing plan to avoid overlapping with on-prem.
- Plan for cross-region peering and failover; consider gateway transit if needed.

TMS Notes

- Always prefer Private Link for PaaS to reduce public surface area.
- Hub-and-Spoke simplifies governance but increases complexity of deployment automation — codify via Terraform modules.
