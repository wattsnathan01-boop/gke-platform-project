# GKE Platform Project

A production-grade Kubernetes platform on Google Cloud Platform (GCP), built entirely with Terraform (Infrastructure as Code).

## What it deploys

- **VPC** — custom private network with no auto-created subnets
- **Subnet** — regional subnet in `us-central1` with dedicated IP ranges for pods and services
- **GKE Cluster** — Google Kubernetes Engine cluster with multi-zone node pool
- **Node Pool** — `e2-small` nodes using standard persistent disk, spread across availability zones for redundancy

## Architecture

```
Internet
    │
    ▼
GCP Load Balancer (public IP)
    │
    ▼
GKE Cluster (us-central1)
    ├── Node (us-central1-a)
    ├── Node (us-central1-b)
    └── Node (us-central1-c)

All resources inside private VPC (10.0.1.0/24)
Pod network:     10.1.0.0/16
Service network: 10.2.0.0/16
```

## Prerequisites

- [Terraform](https://www.terraform.io/) >= 1.0
- [Google Cloud SDK](https://cloud.google.com/sdk)
- A GCP project with billing enabled
- The following APIs enabled:
  - `compute.googleapis.com`
  - `container.googleapis.com`

## Usage

1. Authenticate with GCP:
```bash
gcloud auth application-default login
```

2. Update the `project` field in `main.tf` with your GCP project ID.

3. Initialise Terraform:
```bash
terraform init
```

4. Preview the changes:
```bash
terraform plan
```

5. Deploy the infrastructure:
```bash
terraform apply
```

6. Connect kubectl to the cluster:
```bash
gcloud container clusters get-credentials gke-cluster --region=us-central1
```

7. Verify nodes are running:
```bash
kubectl get nodes
```

## Tear down

To avoid ongoing costs, destroy the infrastructure when not in use:
```bash
terraform destroy
```

## Tech Stack

- **Terraform** — Infrastructure as Code
- **Google Cloud Platform** — Cloud provider
- **Google Kubernetes Engine (GKE)** — Managed Kubernetes
- **kubectl** — Kubernetes CLI
