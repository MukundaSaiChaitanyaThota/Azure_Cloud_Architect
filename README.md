# Cloud Architect Roadmap (TMS)

> Transition from **DevOps Engineer** → **Azure Cloud Architect** in ~3 months,  
> with a focus on **Azure**, **AKS**, **Terraform**, **GitOps**, and **Hybrid (Microservices + Serverless)** architectures.

---

## 🎯 Goal

This repository documents my journey as **TMS** from a hands-on DevOps engineer to an **Azure Cloud Architect**.

It contains:

- A **week-by-week learning roadmap**
- **Real architecture projects** (AKS, GitOps, multi-region, serverless)
- **Design documents, diagrams, and decisions**
- A portfolio that hiring managers and architects can review

---

## 🧭 Roadmap Overview

The journey is split into phases:

1. **Week 0** – Setup & Baseline
2. **Weeks 1–3** – Foundations: System Design, Networking, AKS
3. **Weeks 4–6** – GitOps, Terraform, Security & Zero Trust
4. **Weeks 7–9** – Advanced Networking, Serverless, Observability & DR
5. **Weeks 10–12** – Cost, Portfolio, Architect Mindset & Interviews

The focus is **Azure-first**, but with solid, portable architecture principles.

---

## 📅 Weekly Progress

> Status legend:  
> ⬜ Not started · 🔄 In progress · ✅ Completed

| Week | Topic                                      | Status   | Notes                                  |
|------|--------------------------------------------|----------|----------------------------------------|
| 0    | Setup, Baseline & Repo Structure           | 🔄 In progress | Initial setup, goals, planning         |
| 1    | System Design Foundations                  | ⬜        | High-level architectures, trade-offs   |
| 2    | Azure Networking & Hub-Spoke Architecture  | ⬜        | VNets, subnets, routing, App Gateway   |
| 3    | AKS Architecture (Microservices Platform)  | ⬜        | Cluster design, ingress, TLS, storage  |
| 4    | GitOps with ArgoCD + Helm                  | ⬜        | GitOps repo layout, Argo strategies    |
| 5    | Terraform for Reusable Cloud Modules       | ⬜        | Modules, remote state, CI/CD for IaC   |
| 6    | Security, RBAC, Zero Trust on Azure        | ⬜        | Key Vault, Policies, Private Endpoints |
| 7    | Advanced Networking & Multi-Region Design  | ⬜        | Private DNS, Firewall, failover        |
| 8    | Hybrid Architectures (AKS + Serverless)    | ⬜        | Functions, Event Grid/Bus, CQRS, queues|
| 9    | Observability, DR, Resilience              | ⬜        | Logging, tracing, chaos, RPO/RTO       |
| 10   | Cost Optimization & FinOps                 | ⬜        | Spot, reserved, scaling, dashboards    |
| 11   | Portfolio Build & Case Studies             | ⬜        | Clean docs, diagrams, narratives       |
| 12   | Architect Mindset & Interview Prep         | ⬜        | Mock designs, Q&A, review & polish     |

I’ll update this table as I progress.

---

## 🏗 Key Projects (Portfolio)

Each project will live under [`/projects`](./projects) with its own `README.md` and diagrams.

- **1. Production-Grade AKS Platform (Microservices)**  
  - Private AKS cluster  
  - Ingress controller + App Gateway / Nginx  
  - Multiple node pools (including spot)  
  - GitOps deployment with ArgoCD + Helm  
  - RBAC, managed identities, Key Vault integration  
  - 📁 `projects/aks-reference-architecture/`

- **2. Multi-Region, Highly Available Architecture**  
  - Hub-spoke VNets  
  - Traffic Manager/Front Door or equivalent  
  - Geo-replicated data store  
  - Documented failover plan (RPO/RTO)  
  - 📁 `projects/multi-region-failover/`

- **3. Hybrid Microservices + Serverless Application**  
  - Core API on AKS  
  - Background processing using Azure Functions  
  - Messaging via Service Bus/Event Grid  
  - Event-driven workflows, DLQ, retries  
  - 📁 `projects/hybrid-microservices-serverless/`

- **4. GitOps Platform with ArgoCD**  
  - GitOps repo structure for environments (dev/stage/prod)  
  - ApplicationSets, Helm charts, promotions  
  - Policy and security considerations  
  - 📁 `projects/gitops-with-argocd/`

- **5. Observability & Resilience Layer**  
  - Logging, metrics, tracing setup  
  - Dashboards & alerts  
  - Chaos experiments & resilience report  
  - 📁 `projects/observability-and-dr/`

---

## 📂 Repo Structure

> Planned repo layout (will evolve as I progress):

```text
cloud-architect-roadmap/
│
├── README.md
│
├── weeks/
│   ├── 00-setup-and-baseline/
│   │   └── README.md
│   ├── 01-system-design/
│   │   └── README.md
│   ├── 02-azure-networking/
│   │   └── README.md
│   ├── 03-aks-architecture/
│   │   └── README.md
│   ├── 04-gitops-argocd/
│   │   └── README.md
│   ├── 05-terraform-modules/
│   │   └── README.md
│   ├── 06-security-and-zero-trust/
│   │   └── README.md
│   ├── 07-advanced-networking-multiregion/
│   │   └── README.md
│   ├── 08-hybrid-microservices-serverless/
│   │   └── README.md
│   ├── 09-observability-and-dr/
│   │   └── README.md
│   ├── 10-cost-optimization/
│   │   └── README.md
│   ├── 11-portfolio-and-case-studies/
│   │   └── README.md
│   └── 12-architect-mindset-and-interview-prep/
│       └── README.md
│
├── projects/
│   ├── aks-reference-architecture/
│   │   └── README.md
│   ├── multi-region-failover/
│   │   └── README.md
│   ├── hybrid-microservices-serverless/
│   │   └── README.md
│   ├── gitops-with-argocd/
│   │   └── README.md
│   └── observability-and-dr/
│       └── README.md
│
├── diagrams/
│   ├── hub-spoke-vnet.drawio
│   ├── hub-spoke-vnet.png
│   ├── aks-reference-architecture.drawio
│   ├── aks-reference-architecture.png
│   ├── hybrid-microservices-serverless.png
│   ├── multi-region-failover.png
│   └── observability-and-dr.png
│
└── docs/
    ├── architecture-decisions/
    │   ├── adr-001-cluster-networking.md
    │   ├── adr-002-gitops-strategy.md
    │   ├── adr-003-security-baseline.md
    │   └── adr-004-multi-region-strategy.md
    └── notes/
        ├── system-design-notes.md
        ├── azure-networking-notes.md
        └── aks-architecture-notes.md
