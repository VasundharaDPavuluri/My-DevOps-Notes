# ◾ How Kubernetes Deploys Applications Without Downtime (Rolling Updates)

While deploying applications, one important question comes up:

👉 How do systems get updated without downtime?

No disruption. No failed requests.

---

## 📌 The Situation

Let’s say an application is running with:

- **5 pods (version v1)**  
- Users are actively using the system  

Now you want to deploy a new version:

👉 **v2**

---

## ⚠️ The Problem

If all pods are updated at once:

- Application goes down  
- Users face downtime  
- Requests fail  

This is not acceptable in production systems.

---

## ⚙️ How Kubernetes Solves This

Kubernetes uses a **Rolling Update strategy**.

Instead of replacing all pods at once, it updates them **gradually**.

---

## 🔁 What Actually Happens

When you update the image:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.21
```

Kubernetes performs the update step by step:

1. Creates **1 new pod (v2)**  
2. Waits until it is **healthy**  
3. Deletes **1 old pod (v1)**  
4. Repeats the process  

---

## 🔄 Rolling Update Flow

```text
v1 pods running
↓
New v2 pod created
↓
Health check passes
↓
Old v1 pod removed
↓
Repeat until all pods are v2
```

---

## 🎯 Controlled by These Settings

Kubernetes manages rolling updates using:

### maxUnavailable

- Maximum number of pods that can be unavailable during update  
- Example: `1`

👉 Ensures minimum availability

---

### maxSurge

- Maximum number of extra pods created during update  
- Example: `1`

👉 Ensures smooth rollout

---

## 🚀 Why This Works

Because of controlled updates:

- Application remains available  
- Users don’t experience downtime  
- Updates happen safely  
- Rollback is possible if something fails  

---

## 🧠 One Insight That Stood Out

Kubernetes doesn’t just deploy applications.

It manages **how transitions between versions happen**.

---

## 📌 Simple Way to Remember

```text
Don’t replace everything  
Replace gradually
```

<img width="515" height="593" alt="Post-5-Rolling Updates" src="https://github.com/user-attachments/assets/162be486-6a23-4559-9fcb-ac2dd88fb212" />

---

## 🔚 Summary

During an update:

- New pods are created  
- Old pods are removed gradually  
- System remains available  

👉 Result: **Zero downtime deployment**

---
