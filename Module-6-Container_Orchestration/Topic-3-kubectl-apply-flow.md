# ◾ What Happens When You Run “kubectl apply”? (Behind the Scenes)

While working with Kubernetes, one important question came up:

👉 What actually happens inside the cluster when we run a simple command?

```bash
kubectl apply -f deployment.yaml
```

This single command triggers a complete workflow inside Kubernetes.

---

## 🧠 High-Level Flow

```text
kubectl
↓
API Server
↓
etcd (stores desired state)
↓
Controller Manager
↓
Scheduler
↓
Worker Node
↓
kubelet
↓
Container Runtime
↓
Pod Running
```

---

## ⚙️ Step-by-Step Breakdown

### 1. kubectl (Client)

- Sends request to Kubernetes cluster  
- Uses API Server as entry point  

---

### 2. API Server

- Validates request (syntax, authentication, authorization)  
- Acts as the gateway to the cluster  
- Stores desired state in etcd  

---

### 3. etcd (Cluster Database)

- Stores configuration and cluster state  
- Maintains both desired and current state  

Example:

```text
"3 pods of nginx should be running"
```

---

### 4. Controller Manager

- Continuously compares:

👉 Desired State vs Actual State  

- If resources are missing → triggers creation  

Example:

```text
Desired = 3 pods
Actual = 2 pods
→ Controller creates 1 new pod
```

---

### 5. Scheduler

- Assigns pod to the best available node  

Based on:

- CPU availability  
- Memory availability  
- Node health  
- Scheduling rules  

---

### 6. Worker Node

Once scheduled, execution moves to the selected worker node.

---

### 7. kubelet

- Node agent responsible for execution  
- Communicates with API Server  
- Ensures containers inside pods are running  

---

### 8. Container Runtime

- Pulls container image (if not already present)  
- Creates and starts the container  

Examples:

- containerd  
- Docker (via runc)  
- CRI-O  

---

### 9. Pod Running

- Application is now running inside the pod  
- Ready to serve traffic  

---

## 🔁 Continuous Reconciliation (Important)

Kubernetes does not stop after deployment.

It continuously runs this loop:

```text
Observe → Compare → Correct
```

If any deviation happens:

- Pod crashes  
- Node fails  
- Desired replicas change  

👉 Kubernetes automatically restores the system.

---

## 🧠 Key Insight

Kubernetes is not just executing commands.

It is maintaining a **desired state system**.

> You don’t tell Kubernetes how to do things  
> You tell it what should exist  
> Kubernetes ensures it continuously  

---

## 📌 Simple Way to Remember

```text
You define → Kubernetes ensures
```
---
<img width="500" height="600" alt="K8S-apply-flow" src="https://github.com/user-attachments/assets/116ef7da-a658-4f4b-8bef-faae744aad19" />

---
## 🚀 Key Takeaway

Kubernetes abstracts the complexity of managing containers by:

- Automating deployment  
- Handling scheduling intelligently  
- Ensuring application reliability  
- Maintaining system state continuously  

---

## Summary

A single command like:

```bash
kubectl apply -f deployment.yaml
```

Triggers:

- API validation  
- State storage  
- Scheduling decisions  
- Container execution  

All handled automatically by Kubernetes.

---
