# harness

**Epic: Spike - Evaluating Harness for Infrastructure and Application Pipelines**

**Objective:**
Evaluate the capabilities of Harness (harness.io) within our tech stack (GitLab, Terraform Cloud, and AWS) by implementing two CI/CD pipelines:
1. **Infrastructure Pipeline:** Provision and configure EKS clusters.
2. **Application Pipeline:** Build and deploy a Java microservice.

**Tech Stack:**
- **SCM:** GitLab (Monorepo for infrastructure, separate repo for application)
- **Infrastructure Management:** Terraform Cloud
- **Cloud Provider:** AWS
- **Containerization:** Docker
- **Orchestration:** Kubernetes (EKS)
- **GitOps:** ArgoCD

---

### **Scope of Work**
#### **1. Infrastructure Pipeline**
This pipeline provisions and configures two EKS clusters (Dev and Prod) in the same AWS account.

- **Provision Dev Cluster**
  - Deploy an EKS cluster using Terraform.
- **Configure Dev Cluster**
  - Install and configure ArgoCD (if possible).
  - Point ArgoCD to the infrastructure repository in GitLab (can be a public repo).
- **Smoke Test Configuration**
  - Execute a script that runs `kubectl get pods -A` to verify ArgoCD deployment.
  - If the test passes, proceed to the next step.
- **Provision Prod Cluster**
  - Deploy an EKS cluster for production.

#### **2. Application Pipeline**
This pipeline builds, packages, and deploys a Java microservice to the provisioned EKS clusters.

- **Build & Package**
  - Checkout the Java microservice repository.
  - Perform a Maven build, test, and package step.
  - Build a Docker container and push it to AWS ECR.
- **Update ArgoCD Repository**
  - Modify the Kubernetes manifests in the ArgoCD repository to reference the new container image.
- **Deploy to Dev Cluster**
  - ArgoCD applies the updated manifests to the Dev EKS cluster.
  - Validate deployment success.
- **Deploy to Prod Cluster (Manual Trigger)**
  - After validation in Dev, a manual trigger allows promotion to the Prod cluster.

---

### **Acceptance Criteria**
#### **General Acceptance Criteria**
- The Harness pipelines should be easy to reproduce in another AWS account by configuring necessary parameters (GitLab tokens, AWS credentials, Kubeconfigs, etc.).
- The initial Harness agent can be run from a local machine or Minikube.
- Both pipelines should execute successfully.

#### **Infrastructure Pipeline Acceptance Criteria**
- [ ] The pipeline provisions a Dev EKS cluster using Terraform Cloud.
- [ ] ArgoCD is installed on the Dev cluster and points to the correct GitLab repository.
- [ ] A smoke test (`kubectl get pods -A`) verifies the cluster is configured correctly.
- [ ] The pipeline provisions a Prod EKS cluster only if the smoke test passes.

#### **Application Pipeline Acceptance Criteria**
- [ ] The pipeline builds and packages the Java microservice using Maven.
- [ ] The Docker image is built and pushed to AWS ECR.
- [ ] Kubernetes manifests are updated in the ArgoCD repository.
- [ ] The Dev cluster successfully deploys the updated microservice via ArgoCD.
- [ ] A manual trigger deploys the microservice to the Prod cluster.

#### **Reproducibility & Portability Acceptance Criteria**
- [ ] All configurations (GitLab tokens, AWS credentials, kubeconfigs) can be parameterized for deployment in another AWS account.
- [ ] The Harness agent can be set up either locally or on Minikube with minimal effort.
- [ ] The entire setup should be repeatable without major manual interventions.
