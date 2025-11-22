# 02-azure-networking

# Week 02: Azure Networking

## 🎯 Objectives

- Master Azure networking building blocks and design secure, scalable topologies for hybrid microservices + serverless.
- Implement Private Link, Hub-and-Spoke, and secure ingress patterns.
- Define network observability and networking policies for production readiness.

## 📚 Learning Topics

- VNets, subnets, NSGs, route tables, and Azure CNI vs kubenet for AKS.
- Hub-and-Spoke topology, Azure Firewall, Bastion, and DNS design.
- Private Link & Private Endpoints for PaaS services (CosmosDB, Key Vault, Redis).
- Ingress strategies: Azure Front Door, Application Gateway (WAF), and AGIC.
- Connectivity to on-prem: VPN Gateway vs ExpressRoute and forced tunneling concepts.
- Network observability: Network Watcher, NSG flow logs, and Firewall diagnostics.

## 🛠 Hands-on Tasks (copyable steps)

1. Create a Hub-and-Spoke design document

- Draw a diagram that includes: hub VNet with Firewall, Bastion, DNS; spoke VNets for AKS, apps, and data; private endpoints to key PaaS services.

2. Create VNets and subnets (example)

```bash
az group create -n rg-network-demo -l eastus
az network vnet create -g rg-network-demo -n hub-vnet --address-prefix 10.0.0.0/16
az network vnet create -g rg-network-demo -n spoke-app --address-prefix 10.1.0.0/16
```

3. Deploy Private Endpoints for CosmosDB & Redis

```bash
# replace placeholders accordingly
az network private-endpoint create --name pe-cosmos --resource-group rg-network-demo --vnet-name spoke-app --subnet subnet-data --private-connection-resource-id <COSMOS_RESOURCE_ID> --group-ids Sql
```

4. Deploy Azure Firewall in the hub and configure Route Table

```bash
az network firewall create -n tms-firewall -g rg-network-demo --location eastus
# create route table and UDRs to force egress through the firewall
```

5. Configure secure ingress with Front Door + App Gateway

- Deploy Front Door frontend and add regional Application Gateway backends with health probes.
- Use WAF policies on Application Gateway for regional protection.

6. Enable Network Watcher and NSG flow logs

```bash
az network watcher configure -g rg-network-demo -l eastus --enabled true
az monitor log-analytics workspace create -g rg-network-demo -n la-network
# enable NSG flow logs to Log Analytics
```

7. Validate connectivity and private access

- From an AKS pod, verify you can reach CosmosDB via private endpoint and that public endpoint access is blocked.

## 🏗 Deliverables

- Hub-and-Spoke network diagram saved to `cloud-architect-roadmap/diagrams/hub-spoke-network.png`.
- Terraform sample module for hub-and-spoke networking (scaffold) in `projects/terraform-modules` or `infrastructure/modules`.
- Private Endpoint implementation notes and verification steps documented in `docs/notes/azure-networking-notes.md`.
- Network observability queries and example dashboards for NSG/Firewall logs.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/hub-spoke-network.png` — Hub (Firewall, Bastion) -> Spokes (AKS, App, Data) with Private Endpoints to CosmosDB/Redis.
- `cloud-architect-roadmap/diagrams/frontdoor-appgw-architecture.png` — Front Door -> App Gateway -> AKS/Functions per region.

## 📓 Notes & Reflection (TMS perspective)

Networking is the backbone of secure, reliable architectures. For me (TMS), Week 02 reinforced that getting private connectivity and clear segmentation right early saves countless incidents later. I prefer codifying network patterns in Terraform modules so the hub-and-spoke can be reproduced consistently. Observability for networking is often overlooked — enable NSG flow logs and Firewall diagnostics from day one.

— TMS
