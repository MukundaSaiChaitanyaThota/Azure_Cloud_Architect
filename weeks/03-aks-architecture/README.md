# Week 03: AKS Architecture

## 🎯 Objectives

- Design and operate production-grade AKS clusters optimized for security, scalability, and cost-efficiency.
- Understand node pool strategies, networking integration, runtime security, and observability for Kubernetes workloads.

## 📚 Learning Topics

- AKS cluster models: single vs multiple clusters, AKS managed vs self-managed.
- Node pools and Taints/Tolerations, spot instances, and node auto-scaling.
- Networking choices: Azure CNI vs Kubenet, private clusters, AGIC vs NGINX ingress.
- Pod security: RBAC, Pod Security Standards, network policies.
- Storage and persistent volumes: Managed Disks, Azure Files, CSI drivers.
- Observability stack: Prometheus, Grafana, Azure Monitor containers, and tracing.

## 🛠 Hands-on Tasks (copyable steps)

1. Bootstrap an AKS cluster with Terraform or az CLI

    ```bash
    # example using az cli (replace placeholders)
    az group create -n rg-aks-demo -l eastus
    az aks create -n aks-demo -g rg-aks-demo --node-count 2 --enable-addons monitoring --generate-ssh-keys --enable-managed-identity
    ```

2. Add an additional node pool for workloads

    ```bash
    az aks nodepool add --resource-group rg-aks-demo --cluster-name aks-demo --name workers --node-count 2 --node-vm-size Standard_DS2_v2
    ```

3. Configure Azure AD integration and RBAC

    ```bash
    # example reference; follow AKS docs to configure AKS server Azure AD integration and RBAC
    ```

4. Deploy an ingress controller (AGIC or NGINX)

    ```bash
    # AGIC example with Helm
    helm repo add application-gateway-kubernetes-ingress https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    helm install ingress-azure application-gateway-kubernetes-ingress/ingress-azure -n kube-system
    ```

5. Enable network policies and Pod Security Standards

    - Enable Calico or Azure-native network policies and configure pod security admission (enforce restricted policies for critical namespaces).

6. CI/CD for Kubernetes

    - Create a simple GitHub Actions pipeline that builds container images and pushes to ACR, then ArgoCD or kubectl deploys to AKS.

7. Backup & DR for AKS

    - Install Velero and perform snapshot/restore tests for critical workloads and PVs.

## 🏗 Deliverables

- AKS Terraform module scaffold in `projects/aks-reference-architecture` or `infrastructure/modules/aks`.
- Sample Helm chart and ArgoCD app manifest for one microservice.
- Runbook for cluster upgrades, node pool maintenance, and incident response.
- Backup/restore playbook using Velero.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/aks-cluster-architecture.png` — AKS cluster with multiple node pools, AGIC, private cluster API, Private Link to CosmosDB/Redis.

## 📓 Notes & Reflection (TMS perspective)

AKS provides powerful primitives but operational complexity can grow quickly. Week 03 was about balancing control and manageability: use managed capabilities where possible but enforce security and observability. Start small, automate upgrades and backups, and practice restores.

— TMS
