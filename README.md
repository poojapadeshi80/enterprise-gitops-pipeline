# poojapadeshi
#Pooja Suryavanshi - Devops Engineer
# Enterprise GitOps Pipeline: AWS EKS, Terraform, Jenkins, & Ansible

This repository contains a production-grade, end-to-end GitOps delivery pipeline designed to provision high-availability cloud infrastructure and automate application deployments. 

Built with enterprise standards reflecting 5+ years of DevOps engineering patterns, this project highlights robust infrastructure-as-code (IaC), configuration management, automated continuous integration, and Kubernetes orchestration.

---

## 🏗️ Architecture & Component Overview

* **Infrastructure as Code (IaC):** Modularized **Terraform** architecture utilizing verified AWS VPC and EKS modules to build a secure, scalable network layout across multiple Availability Zones (AZs).
* **Configuration Management:** **Ansible** playbooks executing secure post-provisioning steps, package dependencies, and operating system hardening across cluster nodes.
* **Continuous Integration (CI):** A declarative **Jenkinsfile** pipeline handling repository checkouts, static lint validation, structural Docker builds, and automated image publishing.
* **Artifact Registry:** Secure integration with Amazon Elastic Container Registry (**ECR**) for image versioning and vulnerability isolation.
* **Continuous Deployment (CD):** Zero-downtime Rolling Updates triggered natively inside **Kubernetes** using `kubectl` state definitions mapping directly to the live cluster.

---

## 📂 Repository Structure

```text
enterprise-gitops-pipeline/
├── terraform/                  # Terraform modules for AWS VPC and EKS 
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── ansible/                    # Configuration management routines
│   ├── playbooks/
│   │   └── configure-nodes.yml
│   └── inventory.ini
├── kubernetes/                 # K8s manifest files for microservices
│   ├── deployment.yaml
│   └── service.yaml
├── Jenkinsfile                 # Continuous integration pipeline automation
└── README.md
```

---

## 🚀 Step-by-Step Deployment Guide

### Prerequisites
* AWS CLI installed and configured with appropriate IAM administrative permissions.
* Terraform CLI (v1.5+) and Ansible installed on the orchestration runner.
* A running Jenkins master server with access to Docker and the AWS CLI utility tools.

### Step 1: Provision Infrastructure with Terraform
Navigate to the terraform directory, initialize the backend providers, and apply the infrastructure definition:
```bash
cd terraform
terraform init
terraform plan
terraform apply --auto-approve
```
*This block spins up a custom VPC, subnets, NAT gateways, Internet Gateways, IAM execution profiles, and an auto-scaling EKS cluster setup.*

### Step 2: Configure Worker Nodes via Ansible
Gather the target worker node IP addresses from your cloud console, map them within the `ansible/inventory.ini` configuration, and execute the initialization routine:
```bash
cd ../ansible
ansible-playbook -i inventory.ini playbooks/configure-nodes.yml
```

### Step 3: Configure the Jenkins Orchestration Pipeline
1. Connect your GitHub repository to your Jenkins instance using webhooks.
2. Create a new **Pipeline** job pointing directly to this repository.
3. Configure your AWS credentials (`AWS_ACCOUNT_ID`) inside the environment block.
4. Execute the pipeline execution trigger by selecting **Build Now**.

---

## 🛠️ Verification & Validation
Confirm successful infrastructure deployment and application container health by running:
```bash
# Update local kubeconfig to point to the new cluster
aws eks update-kubeconfig --region us-east-1 --name enterprise-kubernetes-cluster

# Check running pod health across availability targets
kubectl get pods -o wide

# Retrieve public facing LoadBalancer DNS resolution endpoint
kubectl get svc enterprise-app-service
```
