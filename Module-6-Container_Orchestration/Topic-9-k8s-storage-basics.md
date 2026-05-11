# ◾ Why Containers Lose Data (And How Kubernetes Fixes It)

One important thing becomes very clear while working with containers:

Containers are temporary.  
Which means their data is temporary too.

---

## 📌 The Problem

Let’s say an application writes:

- Logs  
- Uploaded files  
- Cache  
- Database data  

inside a container.

Everything works fine until:

- Container crashes  
- Pod gets deleted  
- Pod moves to another node  

Then suddenly:

👉 The data is gone.

---

## 🧠 Why This Happens

Containers and Pods in Kubernetes are:

- Ephemeral  
- Replaceable  
- Continuously recreated  

Their filesystem does not survive pod lifecycle changes.

---

## ⚠️ The Challenge

Applications still need persistent data.

Examples:

- Databases  
- Application logs  
- Shared files  
- User uploads  

So Kubernetes needs a way to separate:

```text
Application Lifecycle
from
Storage Lifecycle
```

---

## ⚙️ How Kubernetes Solves This

Kubernetes introduces the concept of **Volumes**.

Volumes allow data to exist independently from containers.

---

# 🔹 Common Storage Types

## 1. emptyDir

Temporary storage created when a Pod starts.

### Characteristics

- Shared between containers in same Pod  
- Deleted when Pod is deleted  

### Common Use Cases

- Cache  
- Temporary files  
- Shared logs  

---

## 2. hostPath

Mounts storage directly from the worker node filesystem.

Example:

```text
/data/html
```

### Common Use Cases

- Learning  
- Local testing  
- Node-level access  

⚠️ Not preferred for production environments.

---

## 3. PersistentVolume (PV) & PersistentVolumeClaim (PVC)

Production-grade storage approach.

### PersistentVolume (PV)

- Actual storage resource  

### PersistentVolumeClaim (PVC)

- Request for storage made by application  

Pods use PVC instead of directly using storage.

---

## 🔁 Real Storage Flow

```text
Pod
↓
PVC
↓
PV
↓
Storage (EBS / EFS / NFS)
```

---

## 🚀 Why This Design Matters

Because now:

- Pods can restart  
- Containers can be recreated  
- Data still survives  

---

## 🧠 Key Insight

Kubernetes separates:

```text
Compute
from
Storage
```

Pods are temporary.  
Storage should not be.

---

## 📌 Simple Way to Remember

```text
Container = Temporary
Storage  = Persistent
```
<img width="1536" height="1024" alt="Ephemeral" src="https://github.com/user-attachments/assets/05d4c842-c86a-4b51-a615-8eab5c24e13c" />

---

## 🔚 Why This Matters in Production

Without persistent storage:

- Databases lose data  
- Applications become unreliable  
- Logs disappear after restarts  

With Kubernetes storage:

- Data survives pod failures  
- Applications remain reliable  
- Stateful workloads become possible  

---
