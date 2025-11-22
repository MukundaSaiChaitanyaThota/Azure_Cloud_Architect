# ADR-001: AKS Cluster Networking Model

## Context

AKS clusters require a networking model that balances operational simplicity, security, and scale. Key requirements for our platform include:

- Secure, private connectivity between AKS workloads and PaaS data services (CosmosDB, Key Vault, Redis) using Private Endpoints.
- Predictable IP addressing and routing to support hub-and-spoke topologies and Azure Firewall/NVA integration.
- Support for advanced networking features (network policies, pod-to-pod security, and transparent VNet integration) to enable isolation and least-privilege communication.
- Reasonable operational complexity for cluster lifecycle (scaling, upgrades) and predictable capacity planning for IP addresses.

## Decision

Adopt Azure CNI (Azure Container Networking Interface) in conjunction with private AKS clusters (API server private endpoint) and a hub-and-spoke VNet topology.

Key points of the decision:

- Use Azure CNI for primary clusters to allocate pod IPs from VNet subnets, enabling first-class VNet integration and simplified routing to PaaS private endpoints.
- Place AKS node pools in dedicated subnets with sized address spaces to avoid IP exhaustion; design VNet addressing with headroom for scaling and additional clusters.
- Enable Pod Network Policies (Calico or Azure-native policies) to enforce pod-to-pod isolation and use NSGs at the subnet boundary for coarse-grained controls.
- Use Private Endpoints for CosmosDB, Redis, Key Vault, and block public network access where possible.
- Run clusters as private clusters (disable public API server) and rely on jump hosts or private bastions for admin access, combined with Azure AD integration for RBAC.
- Where extreme IP conservation is required (very large multi-tenant footprints), consider separate clusters with Kubenet or alternate CNI strategies per team/project.

## Alternatives

1. Kubenet (basic Kubernetes networking)

- Pros: Conserves IP address space (pods use NAT to access VNet), simpler IP planning for very large clusters.
- Cons: More complex routing, limitations with advanced Azure networking features, additional complexity when integrating with Private Endpoints and Azure Firewall.

2. Azure CNI with overlay or secondary IPAM solutions (e.g., Kubenet + Azure CNI hybrid approaches)

- Pros: Can reduce IP consumption while retaining some VNet integration features.
- Cons: Increases operational complexity and may require custom tooling and maintenance.

3. Cilium/other CNIs with eBPF

- Pros: Advanced networking features (L7 policies, eBPF performance), improved security and observability.
- Cons: Additional operational surface and learning curve; integration with Azure native services may need extra work.

## Trade-offs

- Azure CNI provides the most straightforward, Azure-native experience: pod IPs are routable within the VNet which simplifies Private Endpoint connectivity and network policy enforcement. The downside is increased IP address consumption requiring careful VNet and subnet planning.
- Kubenet reduces IP consumption but introduces routing and operational complexity that often causes friction when integrating with PaaS services and enterprise network controls.
- Choosing Azure CNI favors security and simplicity at the expense of IP planning effort and potential need for larger address spaces or more clusters.
- Private clusters increase security posture (no public API) but require private admin workflows (bastion, VPN, or Jump hosts) and careful automation for cluster lifecycle tasks.

## Status

Accepted — Azure CNI + Private AKS + Hub-and-Spoke topology.


## Notes

- Revisit this ADR when evaluating multi-cluster strategies, IPv6 in Azure, or if significant changes in CNI capabilities occur.
- Document VNet addressing guidelines and provide Terraform module examples to enforce consistent subnet sizing and network resources.
