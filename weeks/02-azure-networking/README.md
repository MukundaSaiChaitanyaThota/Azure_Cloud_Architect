# 02-azure-networking

# Week 02: Azure Networking & Hub‑and‑Spoke Architecture

## 🎯 Objectives

- Design and implement a secure, scalable hub‑and‑spoke network topology for hybrid microservices + serverless workloads on Azure.
- Learn to use Private Endpoints, VNet peering, Azure Firewall, and Application Gateway to protect and route traffic.
- Bootstrap a Terraform-managed hub‑and‑spoke network and validate private AKS networking with an App Gateway ingress path.

## 📚 Learning Topics

- VNets & Subnets: address planning, subnet sizing, and IP management for node pools and pods.
- Routing & NAT: Route Tables (UDRs), forced tunneling patterns, and NAT Gateway for predictable egress.
- Private Endpoints & Private Link: securing PaaS access (CosmosDB, Key Vault, Redis) over the VNet.
- ExpressRoute & VPN Gateway: differences, use-cases, and hybrid connectivity patterns.
- DNS: Private DNS zones, conditional forwarding, and name resolution across peered VNets.
- VNet Peering vs Hub‑and‑Spoke: when to peer, transitive routing limitations, and gateway transit.
- Firewall & WAF: Azure Firewall for centralized policy; WAF (on Application Gateway/Front Door) for L7 protection.
- Network observability: Network Watcher, NSG flow logs, Firewall logs and integration with Log Analytics.

## 🛠 Hands-on Tasks (copyable steps)

> The steps below are example commands and Terraform guidance — adapt names, regions, and resource IDs for your subscription.

1) Plan IP addressing and subnets

- Choose address spaces that avoid overlap with on‑premises ranges. Example:
  - Hub VNet: 10.0.0.0/16
  - Spoke-app: 10.1.0.0/16 (AKS)
  - Spoke-data: 10.2.0.0/16 (datastores)
  - Spoke-platform: 10.3.0.0/16 (App Gateway, NAT)

2) Terraform scaffold for hub-and-spoke (quick start)

- Create `infrastructure/networking/hub` and `infrastructure/networking/spoke` modules. Minimal example backend init:

```hcl
# infrastructure/networking/main.tf (example)
module "hub" {
  source = "./modules/hub"
  vnet_address_space = ["10.0.0.0/16"]
  location = var.location
}

module "spoke_app" {
  source = "./modules/spoke"
  parent_vnet_id = module.hub.vnet_id
  vnet_address_space = ["10.1.0.0/16"]
}
```

- Initialize & plan

```bash
cd infrastructure/networking
terraform init
terraform plan -out tfplan
terraform apply tfplan
```

3) Deploy Azure Firewall in hub and configure UDRs

```bash
az network firewall create -g rg-network -n tms-firewall -l eastus
# Create route table, add default route for 0.0.0.0/0 to firewall private IP, associate with spoke subnet(s)
```

4) Create Private Endpoints for PaaS

```bash
# CosmosDB example
az network private-endpoint create --name pe-cosmos --resource-group rg-network --vnet-name spoke-data --subnet data-subnet --private-connection-resource-id /subscriptions/<sub>/resourceGroups/<rg>/providers/Microsoft.DocumentDB/databaseAccounts/<cosmos> --group-ids Sql
```

- Register private DNS zone and link to VNets:

```bash
az network private-dns zone create -g rg-network -n "privatelink.documents.azure.com"
az network private-dns link vnet create -g rg-network -n link-hub --zone-name "privatelink.documents.azure.com" --virtual-network hub-vnet --registration-enabled false
```

5) App Gateway + AGIC for private ingress to AKS

- Deploy Application Gateway into the hub/spoke-platform subnet with WAF enabled and ensure backend pools point to AGIC-managed endpoints.

```bash
# Install AGIC in AKS
helm repo add application-gateway-kubernetes-ingress https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
helm install ingress-azure application-gateway-kubernetes-ingress/ingress-azure -n kube-system --set appgw.name=<AGW_NAME> --set appgw.resourceGroup=<AGW_RG>
```

- Create Ingress manifest in your app with the class `azure/application-gateway` and test TLS termination via App Gateway.

6) Private AKS cluster networking validation

- Create a private AKS cluster (API server private) and ensure pods can reach PaaS via Private Endpoints.

```bash
az aks create -n aks-private -g rg-aks --enable-private-cluster --network-plugin azure --vnet-subnet-id /subscriptions/.../subnets/aks-subnet --enable-managed-identity --enable-addons monitoring
```

- Test from a pod

```bash
kubectl run -it --rm --image=alpine --overrides='{ "apiVersion": "v1", "kind": "Pod", "spec": { "containers": [{ "name": "curl", "image": "curlimages/curl", "command": ["sleep","3600"] }], "restartPolicy": "Never" }}' curl-test
kubectl exec -it pod/curl-test -- nslookup <cosmos-account>.privatelink.documents.azure.com
kubectl exec -it pod/curl-test -- curl -v https://<cosmos-private-ip>
```

7) Hybrid connectivity: verify ExpressRoute or VPN

- For ExpressRoute, coordinate with networking team to provision circuits and create private peering to your hub.
- For VPN Gateway quick test:

```bash
az network vnet-gateway create -g rg-network -n vpngw --public-ip-addresses <ip> --vnet hub-vnet --gateway-type Vpn --vpn-type RouteBased
```

8) Observability: enable Network Watcher & NSG flow logs

```bash
az network watcher configure -g rg-network -l eastus --enabled true
az network watcher flow-log configure -g rg-network --nsg /subscriptions/<sub>/resourceGroups/<rg>/providers/Microsoft.Network/networkSecurityGroups/<nsg> --enabled true --retention 30 --workspace /subscriptions/<sub>/resourceGroups/<rg>/providers/Microsoft.OperationalInsights/workspaces/<workspace>
```

## 🏗 Deliverables

- Terraform modules for `hub` and `spoke` networks with examples in `infrastructure/networking`.
- App Gateway + AGIC deployment manifest and sample Ingress in `projects/aks-reference-architecture`.
- Private AKS provisioning script or Terraform module with validation steps.
- Private DNS zone configuration and documentation for name resolution.
- Network observability queries (Log Analytics) and a sample dashboard for NSG/Firewall logs.

## 🔍 Diagrams (placeholders)

- `cloud-architect-roadmap/diagrams/hub-spoke-network.png` — Hub VNet with Firewall, Bastion, shared services; spokes for AKS, apps, and data with Private Endpoints.
- `cloud-architect-roadmap/diagrams/private-ingress-routing.png` — Flow: Front Door (optional) -> Frontend App Gateway (WAF) -> Backend App Gateway (regional) -> AGIC -> AKS pod; show UDRs & Firewall egress path.

(Export PNG/SVG into `cloud-architect-roadmap/diagrams/`.)

## 📓 Personal Reflection (TMS perspective)

Networking is the unsung hero of platform reliability. Week 02 forced me to think about address space and routability before any services were deployed. The hub‑and‑spoke model centralizes control and simplifies security posture, but it also introduces a dependency on the hub components — make sure automation is robust. Private endpoints dramatically reduce public exposure of PaaS services, but DNS and cross‑VNet resolution add complexity that should be scripted and tested.

Practical tips I used:
- Automate DNS zone creation and linking — manual steps are fragile.
- Start with a conservative subnet sizing strategy with documented growth plans.
- Test connectivity from a pod early — it surfaces DNS/NSG issues fast.

— TMS
