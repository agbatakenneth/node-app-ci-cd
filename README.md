🚀 End-to-End DevOps CI/CD: Node.js on AWS EKS
📌 Project Overview

This project demonstrates a production-ready DevOps workflow for deploying a Node.js application on AWS Kubernetes (EKS). It showcases full automation, code quality enforcement, containerization, CI/CD pipelines, and monitoring & observability — all implemented in a real-world enterprise style.

Instead of just deploying an app, this project tells a story of how infrastructure, code, and operations come together to deliver reliable software.
```
🧱 Architecture at a Glance
GitHub → Jenkins (EC2)
        ├── SonarCloud (Code Quality)
        ├── Docker Build & Tag
        ├── Push to AWS ECR
        └── Deploy to AWS EKS
              ├── Kubernetes Deployment
              ├── LoadBalancer Service
              ├── HPA (Auto Scaling)
              └── ServiceMonitor → Prometheus → Grafana
```

Think of this as a continuous feedback loop: Code → Build → Deploy → Monitor → Improve.

🔧 Tools & Technologies
Layer	Tool / Service
Cloud Infrastructure	AWS
IaC	Terraform
CI/CD	Jenkins
Containerization	Docker
Registry	AWS ECR
Orchestration	Kubernetes (EKS)
Monitoring	Prometheus
Visualization	Grafana
Code Quality	SonarCloud
Runtime	Node.js
📂 Project Structure (Story of the Code)
```
end-to-end-node-ci-cd/
├── app/           # Node.js service and Dockerfile
├── infra/         # Terraform scripts (VPC, EKS, IAM, ECR)
├── K8s/           # Kubernetes manifests & HPA/ServiceMonitor
├── Jenkinsfile    # CI/CD orchestration
├── script.sh      # Bootstrap Jenkins server
└── README.md
```

Everything is modular — infrastructure, app, and CI/CD are separated but fully connected.

🚀 Step-by-Step Implementation
1️⃣ Build the App

Simple Node.js service exposing /, /health, /metrics

Ready for Kubernetes probes and Prometheus metrics

Dependencies are baked into Docker image (no manual npm install)

2️⃣ Containerize

Node 18 runtime

Expose port 3000

Build & tag automated in Jenkins

3️⃣ Provision Infrastructure

Terraform provisions VPC, subnets, EKS cluster, node groups, IAM roles, and ECR repo

Update kubeconfig for Jenkins access

4️⃣ Setup Jenkins

Bootstrap server using script.sh

Add AWS & Kubernetes credentials

Configure plugins (Docker, NodeJS, SonarCloud, GitHub, etc.)

5️⃣ Configure Pipeline

Checkout → Code Quality Scan → Docker Build → Push to ECR → Deploy → Install Monitoring

SonarCloud scan enforces quality gates

Helm installs Prometheus & Grafana automatically

6️⃣ Deploy & Monitor

Kubernetes manifests applied dynamically with updated image tags

Rollout verified with kubectl rollout status

Prometheus scrapes metrics, Grafana visualizes them

7️⃣ Cleanup

Helm uninstall + kubectl delete + terraform destroy to avoid costs

💡 Why This Project Matters

Demonstrates enterprise-grade CI/CD

Shows secure Docker image management

Highlights IaC & Kubernetes best practices

Implements full observability (Prometheus + Grafana)

Enforces automated quality & deployment standards

If you like, I can take this further and create a fully “storytelling” README that:

Reads like a portfolio case study for recruiters

Highlights your decisions, challenges, and outcomes

Still includes all technical steps for reproducibility

Do you want me to do that next?

pls do it

Perfect — let’s transform your README into a portfolio-ready, storytelling style, keeping all the technical steps but presenting them like a real-world case study that recruiters and engineers will love. Here’s a full draft:

🚀 End-to-End DevOps CI/CD: Node.js on AWS EKS
📌 Project Overview

Imagine shipping a Node.js application to production with zero manual steps, full automation, and enterprise-grade standards.

This project is exactly that: a complete DevOps workflow on AWS Kubernetes (EKS), combining:

Infrastructure as Code (Terraform)

Continuous Integration & Delivery (Jenkins)

Containerization (Docker & AWS ECR)

Kubernetes Deployment

Monitoring & Observability (Prometheus + Grafana)

Code Quality Enforcement (SonarCloud)

It demonstrates how a developer and DevOps engineer can collaborate seamlessly to ship reliable software while maintaining full control over infrastructure, security, and observability.
```
🧱 Architecture Overview
GitHub → Jenkins (EC2)
        ├── SonarCloud (Code Quality)
        ├── Docker Build & Tag
        ├── Push to AWS ECR
        └── Deploy to AWS EKS
              ├── Kubernetes Deployment
              ├── LoadBalancer Service
              ├── HPA (Auto Scaling)
              └── ServiceMonitor → Prometheus → Grafana
```

This workflow ensures continuous feedback, zero-downtime deployments, and automatic scaling based on application demand.

🛠 Tools & Technologies
Layer	Tool / Service
Cloud Infrastructure	AWS
IaC	Terraform
CI/CD	Jenkins
Containerization	Docker
Container Registry	AWS ECR
Orchestration	Kubernetes (EKS)
Monitoring	Prometheus
Visualization	Grafana
Code Quality	SonarCloud
Runtime	Node.js
📂 Repository Structure
end-to-end-node-ci-cd/
├── app/           # Node.js application & Dockerfile
├── infra/         # Terraform scripts (VPC, EKS, IAM, ECR)
├── K8s/           # Kubernetes manifests & HPA/ServiceMonitor
├── Jenkinsfile    # CI/CD orchestration
├── script.sh      # Jenkins bootstrap script
└── README.md


Everything is modular: infrastructure, application, and CI/CD pipelines are separated but fully integrated.
---
🌟 Implementation Journey
1️⃣ Building the Application

Node.js HTTP service exposing:

/ → main endpoint

/health → readiness & liveness for Kubernetes

/metrics → Prometheus metrics

All dependencies are installed inside Docker — no manual npm commands needed.
---
2️⃣ Containerization

Dockerfile uses Node 18 runtime

Exposes port 3000

Image creation and tagging are fully automated in Jenkins

Taging uses Jenkins BUILD_NUMBER for version control
---
3️⃣ Infrastructure Provisioning (Terraform)

Terraform provisions:

VPC, subnets, and networking

EKS cluster (devops-eks)

Node groups

IAM roles and policies

ECR repository

Apply Terraform scripts:

cd infra
terraform init
terraform apply


Configure Kubernetes access:

aws eks update-kubeconfig --region us-east-1 --name devops-eks


This kubeconfig is uploaded to Jenkins as a secure file credential.
---
4️⃣ Jenkins Setup

Jenkins runs on an EC2 instance

script.sh bootstraps the server with required packages

Credentials added securely:

AWS access

Kubernetes kubeconfig

SonarCloud token
---
5️⃣ GitHub Integration

Pipeline triggered on push via webhook

Repository is fully connected to Jenkins SCM pipeline
---
6️⃣ Jenkins Pipeline Overview

Pipeline stages:

Checkout Code → Pull latest changes from GitHub

SonarCloud Scan → Static analysis, automated quality gates

Docker Build → Build & tag image

Push to ECR → Authenticate and push image

Deploy to EKS → Apply Kubernetes manifests

Install Monitoring → Helm deploy of Prometheus + Grafana

Tools configured in Jenkins: JDK 17, NodeJS 18
Plugins used: Docker Pipeline, NodeJS, Kubernetes CLI, AWS Credentials, GitHub, SonarQube Scanner
---
7️⃣ **Static Code Analysis (SonarCloud)**

- Dockerized SonarCloud scanner ensures quality gates

- Pipeline fails if code does not meet standards

Code issues visible in SonarCloud dashboard
---
8️⃣ **Docker Build & Push**

- Jenkins builds image from app/Dockerfile

- Tags using BUILD_NUMBER

- Pushes image to AWS ECR

Example:
```
663395718372.dkr.ecr.us-east-1.amazonaws.com/node-repo
```

No manual intervention required.
---
9️⃣ **Kubernetes Deployment & Monitoring**


**Deployment Flow:**

- Jenkins updates image tag dynamically

- Applies manifests from K8s/

- Verifies rollout status
```
kubectl rollout status deployment/node-app
```

**Resources deployed:**

- Deployment

- LoadBalancer Service

- Horizontal Pod Autoscaler

- ServiceMonitor

**Monitoring & Observability:**

- Prometheus scrapes metrics automatically

- Grafana dashboards visualize pod and application metrics

Access Grafana locally:
```
kubectl port-forward svc/prometheus-grafana -n monitoring 3001:80
```

Login via Kubernetes secrets for admin credentials.
---

🔟 **Cleanup**

To avoid costs after testing:
```
helm uninstall prometheus -n monitoring
kubectl delete -f K8s/
cd infra
terraform destroy
```
