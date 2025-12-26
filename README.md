# End-to-End DevOps CI/CD with Terraform, Jenkins & AWS EKS

## 📌 Project Overview

This project demonstrates a **production-ready DevOps workflow** that provisions cloud infrastructure using **Terraform** and deploys a **Node.js application** to **AWS EKS** through a fully automated **CI/CD pipeline** powered by **Jenkins**.

The goal of this project is to showcase real-world DevOps practices, including **Infrastructure as Code (IaC)**, **containerization**, **continuous integration**, **continuous delivery**, and **Kubernetes-based application deployment** on AWS.

---

## 🏗️ Architecture Overview

**High-level flow:**

1. Infrastructure is provisioned on AWS using **Terraform**
2. Application code is built and tested via **Jenkins CI pipeline**
3. Docker image is built and pushed to **Amazon ECR**
4. Jenkins deploys the application to **Amazon EKS** using Kubernetes manifests
5. Kubernetes performs **rolling updates**, health checks, and manages replicas

---

## ⚙️ Infrastructure Provisioning (Terraform)

The following AWS resources are provisioned using **Terraform**:

* VPC
* Public and private subnets
* Internet Gateway & route tables
* IAM roles and policies (EKS cluster & worker nodes)
* Amazon EKS cluster
* Managed node groups

**Key benefits:**

* Fully reproducible infrastructure
* Version-controlled cloud resources
* Modular and scalable design

---

## 🚀 CI/CD Pipeline (Jenkins)

The Jenkins pipeline automates the complete application delivery lifecycle and integrates **SonarCloud code quality analysis**, **Docker image management**, **Amazon ECR**, **Kubernetes deployment**, and **cluster monitoring with Prometheus**.

### Pipeline Stages

1. **Checkout Code** – Pulls the source code from GitHub
2. **Static Code Analysis (SonarCloud)** – Runs Sonar Scanner in a Docker container to analyze code quality, bugs, vulnerabilities, and code smells using SonarCloud quality gates
3. **Authenticate to Amazon ECR** – Logs in to Amazon ECR using AWS credentials
4. **Build Docker Image** – Builds a versioned Docker image for the Node.js application
5. **Push Image to ECR** – Pushes the tagged image to Amazon ECR
6. **Deploy to Kubernetes (EKS)** – Updates the Kubernetes Deployment image and applies manifests
7. **Monitoring Setup** – Installs or upgrades **Prometheus (kube-prometheus-stack)** using Helm
8. **Rollout Verification** – Verifies successful rolling deployment using `kubectl rollout status`

The pipeline enforces **code quality checks before deployment** and ensures consistent, automated, and production-ready releases.

---

## ☸️ Kubernetes Deployment (EKS)

The application is deployed to AWS EKS using Kubernetes manifests that define:

* Deployment with multiple replicas
* Rolling update strategy
* Readiness and liveness probes (`/health` endpoint)
* Resource requests and limits

### Monitoring & Observability

* **Prometheus** is installed via **Helm** using the `kube-prometheus-stack`
* Provides cluster-level and application-level metrics
* Enables proactive monitoring of pod health, CPU, memory, and resource utilization

Kubernetes ensures:

* High availability
* Self-healing pods
* Zero-downtime deployments

---

## 🧰 Technology Stack

* **Cloud Provider:** AWS
* **Infrastructure as Code:** Terraform
* **CI/CD:** Jenkins
* **Code Quality & Security:** SonarCloud (Sonar Scanner)
* **Containerization:** Docker
* **Container Registry:** Amazon ECR
* **Orchestration:** Kubernetes (AWS EKS)
* **Monitoring & Observability:** Prometheus (Helm-based deployment)
* **Application:** Node.js
* **Version Control:** Git & GitHub

---

## 📂 Project Structure

```
├── terraform/              # Terraform IaC for AWS resources (VPC, subnets, EKS, IAM)
│   ├── vpc.tf
│   ├── eks.tf
│   ├── iam.tf
│   └── outputs.tf
│
├── k8s/                    # Kubernetes manifests
│   ├── deployment.yaml
│   └── service.yaml
│
├── Jenkinsfile             # Jenkins pipeline (SonarCloud, Docker, ECR, EKS, Prometheus)
├── Dockerfile              # Node.js application containerization
├── app/                    # Application source code
└── README.md
```

---

## ✅ Key Features

* Infrastructure provisioning using **Terraform (IaC)**
* Automated CI/CD with **Jenkins**
* Docker image build and push to **Amazon ECR**
* Deployment to **AWS EKS** with rolling updates
* Kubernetes health checks and self-healing
* Production-oriented DevOps best practices

---

## 🎯 Use Cases

* DevOps Engineer portfolio project
* Cloud Engineer hands-on practice
* CI/CD and Kubernetes learning reference
* Infrastructure-as-Code implementation example

---

## 🔮 Future Improvements

* Add visualization dashboards with **Grafana**
* Extend security scanning with **Trivy** and **Snyk**
* Introduce **Helm** or **GitOps (Argo CD)** for deployments
* Add automated rollback on deployment failure

---

## 👤 Author

**Kenneth Agbata**
DevOps / Cloud Engineer

---

## 📜 License

This project is for educational and portfolio purposes.
