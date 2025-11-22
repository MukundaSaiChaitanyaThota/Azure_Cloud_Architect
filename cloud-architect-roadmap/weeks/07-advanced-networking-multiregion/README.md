Week 07: Advanced Networking & Multi-region

🎯 Objectives

- Design advanced network topologies for multi-region deployments, inter-region connectivity, and optimized failover.
- Implement global traffic management and secure replication strategies.

📚 Learning Topics

- Global load balancing with Front Door and Traffic Manager.
- Inter-region VNet peering and paired-region strategies.
- Data replication strategies for CosmosDB and storage accounts.
- Cross-region private connectivity and routing considerations.

🛠 Hands-on Tasks (Step-by-step)

1. Multi-region Topology
   - Design paired-region topologies and plan for disaster recovery.

2. Global Traffic Management
   - Configure Front Door with regional backends and health probes.

3. Data Replication
   - Configure CosmosDB multi-region writes or read-replicas and test failover.

4. Private Connectivity
   - Implement cross-region private connectivity using VNet peering and private endpoints where applicable.

🏗 Deliverables

- Multi-region network diagrams and failover runbooks.
- Terraform examples for region bootstrap and replication.

🔍 Architecture Diagrams (placeholders)

- diagrams/multi-region-topology.png — Front Door -> Regional AKS/Functions clusters -> CosmosDB multi-region.

📓 Notes & Reflection (TMS perspective)

Week 07 highlighted that multi-region design is trade-off heavy: cost vs RTO/RPO, consistency vs availability. Understand application requirements first before choosing replication strategies.
