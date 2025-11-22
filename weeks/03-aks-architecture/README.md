# Week 03: AKS Architecture — Production Microservices Platform

## 🎯 Objectives

- Design a production-grade AKS platform that supports microservices with security, scalability, and operational hygiene.
- Make explicit choices around networking, node pool strategy, ingress, storage, and autoscaling.
- Provide repeatable deployment and operational patterns.

## 📚 Learning Topics

- Networking: Kubenet vs Azure CNI (trade-offs for IP management, routing, and Private Endpoints).
- Node pools: system vs user vs spot pools; taints/tolerations and upgrade strategies.
- Ingress options: NGINX, Application Gateway Ingress Controller (AGIC), Azure Front Door integration.
- Storage: persistent volumes, CSI drivers, performance and backup considerations; Key Vault for secrets.
- TLS & certificate management with cert-manager and issuing strategies.
- Autoscaling: HPA for pods, Cluster Autoscaler for nodes, KEDA for event-driven scaling.

## 🛠 Hands-on Tasks (copyable steps)

1. Choose networking model and create sample cluster

- Azure CNI (recommended) — run AKS with `--network-plugin azure` to get pod IPs in VNet.

    ```bash
    az aks create -n aks-prod -g rg-aks -l eastus --network-plugin azure --vnet-subnet-id /subscriptions/.../subnets/aks-subnet --enable-managed-identity --enable-addons monitoring
    ```

- Kubenet quick test (if IP conservation needed):

    ```bash
    az aks create -n aks-kubenet -g rg-aks -l eastus --network-plugin kubenet --enable-managed-identity
    ```

2. Create node pools

    ```bash
    # system pool (default)
    az aks nodepool add -g rg-aks --cluster-name aks-prod --name user --node-count 3 --node-vm-size Standard_DS2_v2
    # spot pool for batch or non-critical
    az aks nodepool add -g rg-aks --cluster-name aks-prod --name spot --enable-cluster-autoscaler --min-count 0 --max-count 5 --node-vm-size Standard_D2s_v3 --priority Spot
    ```

3. Ingress options

- Deploy NGINX Ingress Controller for standard L7 routing:

    ```bash
    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress
    ```

- Deploy AGIC for Azure Application Gateway integration (WAF + TLS termination):

    ```bash
    helm repo add application-gateway-kubernetes-ingress https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    helm install ingress-azure application-gateway-kubernetes-ingress/ingress-azure -n kube-system --set appgw.name=<appgw-name> --set appgw.resourceGroup=<rg>
    ```

4. Storage & Key Vault integration

- Use CSI drivers for Azure Disk and Azure File for PVs; configure StorageClasses for performance tiers.
- Mount secrets via External Secrets Operator or use Key Vault FlexVolume / CSI Secrets Store to inject secrets into pods.

5. TLS + cert-manager

    ```bash
    helm repo add jetstack https://charts.jetstack.io
    helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true
    ```
- Configure Issuer (ACME) or an internal CA and annotate Ingress resources to auto-issue certificates.

6. Autoscaling

- HPA example (CPU based):

    ```bash
    kubectl autoscale deployment my-app --cpu-percent=70 --min=2 --max=10
    ```

- Cluster Autoscaler: enable on the cluster or per node pool via `--enable-cluster-autoscaler` and `--min-count/--max-count`.
- KEDA: install KEDA to scale based on Service Bus queue length or custom metrics.

    ```bash
    helm repo add kedacore https://kedacore.github.io/charts
    helm install keda kedacore/keda --namespace keda --create-namespace
    ```

7. Backup & restore

- Install Velero for periodic backups and test restore workflows for namespaces and PVCs.

## 🏗 Deliverables

- AKS Terraform module scaffold with parameters for network plugin, node pool definitions, and autoscaler settings.
- Example Helm charts for ingress and a sample microservice including HPA and resource requests/limits.
- cert-manager Issuer/ClusterIssuer examples and guidance for certificate renewal.
- Backup playbook using Velero and restore verification steps.

## 🔍 Architecture Diagrams (placeholder)

- `cloud-architect-roadmap/diagrams/aks-cluster-architecture.png` — show node pools, ingress options (NGINX/AGIC), storage, Key Vault integration, and monitoring.

## 📓 Notes & Reflection (TMS perspective)

AKS gives flexibility but requires discipline: define node pools early, set sensible resource requests/limits, and automate certificate management and backups. I usually start with Azure CNI for simplicity integrating with Private Endpoints, then add spot pools as I have workload classification data. Instrumentation and autoscaling are non-negotiable for production reliability.

— TMS
