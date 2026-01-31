## 🚀 Infrastructure as Code with Terraform — EKS Cluster + MySQL + CI/CD
This project provisions a fully automated Amazon EKS Kubernetes cluster using Terraform, deploys a MySQL database via Helm with persistent storage, and integrates a Jenkins CI/CD pipeline for infrastructure changes.
It supports multiple identical environments (dev, test, staging, prod) using reusable Terraform modules and remote state.

## 🧱 Architecture Overview
- AWS EKS cluster (managed node group + Fargate profile)
- MySQL deployed via Helm (Bitnami Legacy chart)
- Persistent storage using AWS EBS CSI Driver
- Remote Terraform state stored in S3
- Jenkins pipeline for automated IaC deployments
- Environment‑based provisioning (dev, test, staging, prod)

## 📦 Project Features
### 1. EKS Cluster Provisioning
- Terraform project fully decoupled from application code
- Creates:
- 3‑node managed node group
- 1 Fargate profile for Java application workloads
- Includes:
- VPC, subnets, routing
- IAM roles & policies
- EBS CSI Driver for persistent volumes

### 2. MySQL Deployment via Helm
- MySQL deployed with 3 replicas
- Persistent volumes for data durability
- Uses Bitnami Legacy chart (required due to repo changes)
- Custom values.yaml includes:
global:
  security:
    allowInsecureImages: True
image:
  registry: docker.io
  tag: latest
  repository: bitnamilegacy/mysql
- Deployment depends on EKS cluster creation:
depends_on = [module.eks.cluster_name]



### 3. Remote Terraform State
- S3 bucket configured as backend
- Centralized state for team collaboration
- Optional DynamoDB table for state locking
- Ensures consistent, conflict‑free IaC workflows

### 4. Multi‑Environment Support
Same Terraform code used to create:
- dev
- test
- staging
- production
Environments differ only by variables — ensuring identical cluster configurations.

### 5. CI/CD Pipeline for Infrastructure
A dedicated Jenkins pipeline automates Terraform operations:
- Checkout Terraform repo
- terraform fmt
- terraform validate
- terraform plan
- Manual approval (optional)
- terraform apply
This aligns infrastructure changes with GitOps best practices.
