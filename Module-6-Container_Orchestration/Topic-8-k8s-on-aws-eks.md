# ☸️ Running Kubernetes in Production: Why We Use AWS EKS

When we moved from local Kubernetes setups to production, one thing became clear:

Managing Kubernetes ourselves is not the real goal.  
Running applications reliably at scale is.

---

## 📌 The Problem with Self-Managed Kubernetes

Operating Kubernetes in production involves:

- Managing control plane components  
- Handling upgrades and patches  
- Ensuring high availability  
- Managing networking and security  

This quickly becomes operational overhead.

---

## ⚙️ The Shift to Managed Kubernetes

To reduce this complexity, we use **Amazon EKS**.

EKS is a managed Kubernetes service where:

- AWS manages the control plane  
- We focus on running workloads  

---

# 📍 EKS Architecture

A production EKS setup is divided into two main parts:

---

## 🔹 1. Control Plane (Managed by AWS)

Includes:

- API Server  
- etcd  
- Scheduler  
- Controller Manager  

### Responsibilities

- Cluster management  
- Scheduling  
- Desired state management  
- Cluster coordination  

This layer is highly available and fully managed by AWS.

---

## 🔹 2. Worker Nodes (Managed by Us)

Usually EC2 instances or managed node groups.

### Responsibilities

- Running application pods  
- Handling business workloads  
- Executing containers  

---

## 🔁 High-Level Architecture Flow

```text
Users
↓
AWS Load Balancer
↓
Worker Nodes (EC2)
↓
Pods / Containers
```

AWS manages:

```text
API Server
etcd
Scheduler
Controller Manager
```

We manage:

```text
Applications
Deployments
Services
Workloads
```

---

## 🔹 How We Provision the Cluster

From a management host, we typically install:

- AWS CLI  
- kubectl  
- eksctl  

Cluster creation example:

```bash
eksctl create cluster --name my-cluster --region ap-south-1
```

---

## ⚙️ Application Deployment Flow

Applications are deployed using:

```bash
kubectl apply -f deployment.yaml
```

Services are exposed using:

- ClusterIP → internal communication  
- LoadBalancer → external access  

---

## 📈 What Changes in Production

Compared to local setups like Minikube:

- No need to manage control plane  
- Built-in high availability  
- Integrated AWS networking (VPC)  
- Easier scaling  
- Better operational reliability  

---

## Key Insight

In production:

We do not manage Kubernetes infrastructure.

We manage applications running on Kubernetes.

---

## 📌 Simple Way to Remember

```text
Self-Managed Kubernetes
→ We manage everything

Amazon EKS
→ AWS manages control plane
→ We manage workloads
```

---

<img width="1536" height="1024" alt="K8S_Prod" src="https://github.com/user-attachments/assets/e68c5490-6fa0-4d22-92a1-e264f04284a3" />

---

## 🔚 Why EKS Matters

Using EKS helps us:

- Reduce operational complexity  
- Focus on application delivery  
- Improve scalability and reliability  
- Run Kubernetes in a production-ready way  

---
