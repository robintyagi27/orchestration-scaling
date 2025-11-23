# MERN Stack Kubernetes Deployment with HELM and Jenkins CI/CD

## Overview

This project demonstrates the deployment of a **MERN (MongoDB, Express.js, React.js, Node.js)** application using Kubernetes, HELM charts, and Jenkins for CI/CD automation.

The application is split into two repositories:

- **Frontend (React)**: [learnerReportCS\_frontend](https://github.com/ranyabrkumar/Containerization_Container_Orchestration/tree/main/frontend)
- **Backend (Node.js + Express)**: [learnerReportCS\_backend](https://github.com/ranyabrkumar/Containerization_Container_Orchestration/tree/main/backend)

---

## Objectives

- **Kubernetes Deployments**: Create manifests for both frontend and backend.
- **HELM Chart**: Provide a unified deployment mechanism with configuration flexibility.
- **Jenkins Pipeline**: Automate build, push, and deployment processes.

---

## Prerequisites

- Kubernetes Cluster (Minikube, EKS, GKE, AKS, etc.)
- `kubectl` configured to access the cluster
- Docker (for image builds)
- Helm v3
- Jenkins server with:
  - Docker access
  - Kubernetes CLI tools
  - Credentials for Docker registry & kubeconfig

---

## Project Structure

```
.
├── k8s/                   # Raw Kubernetes manifests
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   
├── helm/
│   └── learnerreport/     # HELM chart for unified deployment
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
├── pipeline.jfl        # Groovy pipeline for CI/CD
└── README.md
```

---

## Deployment Workflow

### 1. Docker Image Build

Jenkins builds images for both frontend and backend:

```bash
docker build -t <registry>/learner-frontend:<tag> ./frontend
docker build -t <registry>/learner-backend:<tag> ./backend
docker push <registry>/learner-frontend:<tag>
docker push <registry>/learner-backend:<tag>
```

### 2. Deploy with Helm

Install/upgrade the application in Kubernetes:

```bash
helm create learnerreport
helm install backend ./learnerreport --values ./learnerreport/values_be.yaml
helm install frontend ./learnerreport --values ./learnerreport/values_fe.yaml
helm install mongo ./learnerreport --values ./learnerreport/values_db.yaml

```

---

## Jenkins Pipeline Overview

Stages in `Jenkinsfile`:

1.**Cleanup Workspace** — Clone FE & BE repositories.
2. **Checkout** — Clone git repositories.
3. **Build Backend Docker Image** — build and push docker image for backend.
4. **Build Frontend Docker Image** — build and push docker image for frontend.
5. **Set kubeconfig** — set kube config.
6. **Deploy using Helm** - Use Helm to update the deployment.
<img width="1103" height="510" alt="helm_deployment_Using_Jenkins" src="https://github.com/user-attachments/assets/35684a30-01b0-41bb-b5f7-346e4cbcb769" />

---

## Verification

- **Frontend**: Access frontend`.
- **Backend**: Test health endpoint (`/api/health`).
- **Logs**: `kubectl logs <pod-name>`
---
  **pod details**
![SVC_details](https://github.com/user-attachments/assets/1b46d3bf-27be-40f5-8860-efe8c4072e24)
![Pods_details](https://github.com/user-attachments/assets/cac1dbf2-91cb-4185-8132-f905b8abac01)
![pod_logs](https://github.com/user-attachments/assets/e6de541d-c943-47cf-b076-cc7e76d951d0)

---
**HELM Chart**
![helm_chart_deployment_v2](https://github.com/user-attachments/assets/5058726a-4ea0-4318-9ce3-d8f8a748a5fb)
![helm_chart_deployment](https://github.com/user-attachments/assets/d54ed3b4-78b9-4013-9606-7f9bc2a7ccaa)

---
**Frontend deployed by K8s**
<img width="2194" height="1323" alt="frontend_k8s" src="https://github.com/user-attachments/assets/4046b8e1-6668-4df6-a646-6a66678532b4" />

---
**Frontend deployed by Helm**

![frontend_helm_manual](https://github.com/user-attachments/assets/04f45271-48d6-44e8-876f-d5c940958b7d)

---

