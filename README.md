# 🚀 AWS EKS Backend Deployment Project

This project demonstrates how to containerize a Python Flask backend, push it to Amazon ECR, and deploy it to Amazon EKS using Kubernetes manifests.

It showcases a complete cloud-native workflow combining Docker, Kubernetes, and AWS infrastructure.

---

## 📚 Table of Contents

1. [Overview](#overview)
2. [Tech Stack](#tech-stack)
3. [Architecture](#architecture)
4. [Deployment Workflow](#deployment-workflow)
5. [Kubernetes Configuration](#kubernetes-configuration)
6. [Run Commands](#run-commands)
7. [Example Request](#example-request)
8. [Notes & Improvements](#notes--improvements)
9. [Key Learnings](#key-learnings)

---

## 🧠 Overview

The application is a Flask-based backend API that retrieves and returns data from an external API (Hacker News search API).

When a user sends a request:

- The backend receives a topic
- It queries an external API
- Filters and formats the response
- Returns structured JSON data

This backend was containerized and deployed into a Kubernetes cluster on AWS EKS.

---

## 🔧 Tech Stack

- **Python (Flask)** – Backend API  
- **Docker** – Containerization  
- **Amazon ECR** – Container registry  
- **Amazon EKS** – Kubernetes cluster  
- **kubectl** – Kubernetes management  
- **eksctl** – Cluster provisioning  
- **AWS CloudFormation** – Infrastructure orchestration  

---

## 🏗️ Architecture

- Flask app runs inside a Docker container  
- Image is built on EC2 and pushed to Amazon ECR  
- EKS cluster is created using `eksctl`  
- Kubernetes manifests define deployment and service  
- `kubectl` deploys the application to the cluster  

Flow:

User → Kubernetes Service → Pod (Flask App) → External API → JSON Response



<img width="598" height="344" alt="Captura de pantalla 2026-04-14 a la(s) 23 50 57" src="https://github.com/user-attachments/assets/321f8dcd-e152-4180-b98a-ceb22cebcebf" />

---

## ⚙️ Deployment Workflow

1. Launch EC2 instance  
2. Clone backend repository  
3. Build Docker image  
4. Push image to Amazon ECR  
5. Create EKS cluster using `eksctl`  
6. Apply Kubernetes manifests  
7. Deploy backend application  

---

## ☸️ Kubernetes Configuration

### Deployment

- 3 replicas of the backend container  
- Runs Flask app on port 8080  

### Service

- Type: `NodePort`  
- Exposes the application internally within the cluster  

---

## 🐳 Run Commands

### Build Docker Image

```bash
docker build -t nextwork-flask-backend .
```

### Push to ECR

```bash
aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.eu-central-1.amazonaws.com

docker tag nextwork-flask-backend:latest <account-id>.dkr.ecr.eu-central-1.amazonaws.com/nextwork-flask-backend:latest

docker push <account-id>.dkr.ecr.eu-central-1.amazonaws.com/nextwork-flask-backend:latest
```

### Deploy to EKS

```bash
kubectl apply -f manifests/
```

### Check Resources

```bash
kubectl get pods
kubectl get svc
```

---

## 🌐 Example Request

```bash
http://<service-ip>:<nodePort>/contents/aws
```

Returns:

```json
{
  "outcome": [
    {
      "id": "123",
      "title": "Example Title",
      "url": "https://example.com"
    }
  ]
}
```

---

## 🚧 Notes & Improvements

The application was successfully deployed and runs inside the Kubernetes cluster.

For full production readiness, the following improvements can be made:

- Change Service type from `NodePort` to `LoadBalancer` for public access  
- Use full Amazon ECR image URI instead of local image reference  
- Add health checks (liveness/readiness probes)  
- Implement autoscaling (HPA)  
- Add monitoring and logging (Prometheus/Grafana)  

---

## 💡 Key Learnings

- How to containerize applications using Docker  
- How to push images to Amazon ECR  
- How to provision Kubernetes clusters using `eksctl`  
- How to deploy applications to EKS using Kubernetes manifests  
- Understanding Kubernetes services and networking  
- Real-world troubleshooting of deployment and exposure issues  

---

## 📌 Why This Project Matters

Modern backend systems are not just about code, they require scalable, observable, and containerized infrastructure.

This project demonstrates:

- Cloud-native deployment workflows  
- Kubernetes based application management  
- Integration between AWS services and container orchestration  

---
## 🙏 Acknowledgment

This project is based on a backend starter provided by NextWork, used as part of a cloud engineering learning exercise.

---
