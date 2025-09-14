# FastAPI Kubernetes Deployment Project

This project demonstrates a complete CI/CD pipeline using **CircleCI**, **Docker**, **Terraform**, **Helm**, and **Kubernetes (EKS)**.  
It builds, tests, and deploys a simple **FastAPI** application to a Kubernetes cluster.

---

## **Project Structure**

terraform-project/
├─ app/
│ ├─ Dockerfile
│ ├─ requirements.txt
│ └─ main.py
├─ helm/
│ └─ myapp/
│ ├─ Chart.yaml
│ ├─ values.yaml
│ └─ templates/
│ └─ deployment.yaml
├─ terraform/
│ ├─ main.tf
│ └─ variables.tf
└─ .circleci/
└─ config.yml


---

## **Features**

- **CI/CD with CircleCI**
  - Orbs for Docker, Helm, Kubernetes, and AWS CLI
  - Caching for dependencies and Docker layers
  - Test splitting for faster pipelines

- **Containerization with Docker**
  - Builds a FastAPI app image
  - Pushes images to DockerHub

- **Infrastructure as Code with Terraform**
  - Deploys an EKS cluster with node groups
  - Outputs cluster endpoint and kubeconfig

- **Kubernetes Deployment with Helm**
  - Helm chart for deploying the FastAPI app
  - Configurable image repository and tag

---

## **Setup Instructions**

1. Configure Terraform Variables
Edit `terraform/variables.tf` or provide a `terraform.tfvars` file with:

aws_region = "us-east-1"
cluster_name = "myapp-cluster"
vpc_id = "<YOUR_VPC_ID>"
subnet_ids = ["<SUBNET_ID1>", "<SUBNET_ID2>"]

2.Terraform Apply
cd terraform
terraform init
terraform apply -auto-approve

3. Build and Push Docker Image
docker build -t myorg/myapp:latest app/
docker push myorg/myapp:latest

4. Helm Deployment
helm upgrade --install myapp helm/myapp \
  --set image.repository=myorg/myapp \
  --set image.tag=latest \
  --namespace default

5. Access the App
kubectl port-forward svc/myapp 8080:80

Notes: Visit: http://localhost:8080

---

## CircleCI Pipeline
- Install dependencies: Caches Python packages
- Test: Runs tests with parallelism and split-by-timing
- Docker Build & Push: Builds and pushes Docker image
- Deploy: Uses Terraform and Helm to deploy to EKS


## Requirements
- Docker
- AWS CLI configured with permissions
- Terraform >= 1.5
- Helm >= 3.12
- CircleCI account
