# How Kubernetes Actually Works: Control Plane vs Worker Nodes

### What actually happens inside a Kubernetes cluster?

At a high level, Kubernetes is not complex.

It follows a very simple design:

##### One part decides → One part executes

## 🧠 Control Plane (The Brain)

The control plane is responsible for managing the cluster.

It doesn’t run applications.
It makes decisions.

Key components:
- API Server → Entry point for all requests
- etcd → Stores cluster state (desired & current)
- Scheduler → Decides where to run pods
- Controller Manager → Ensures desired state = actual state

Example:
If you define:
```
replicas: 3
```
The control plane ensures that 3 pods are always running.

## 💪 Worker Nodes (The Execution Layer)

Worker nodes are where actual applications run.

Each node contains:
- kubelet → Talks to control plane, runs pods
- Container Runtime → Runs containers (containerd / Docker)
- kube-proxy → Handles networking & traffic routing

This is where your microservices actually execute.

## ⚙️ What Happens When We Deploy an App?

When we run:
```
kubectl apply -f deployment.yaml
```

This is what happens:
```
kubectl
  ↓
API Server
  ↓
etcd (stores desired state)
  ↓
Scheduler (selects node)
  ↓
Worker Node
  ↓
kubelet
  ↓
Container Runtime
  ↓
Pod starts running

```

### 🧠 One Insight That Helped Me

Kubernetes is not just “running containers”.

It is constantly doing this:

Comparing desired state vs actual state and fixing the gap automatically.

That’s why:

- Pods restart automatically
- Scaling works seamlessly
- Failures are handled without manual intervention

<img width="500" height="500" alt="k8s-post-2" src="https://github.com/user-attachments/assets/f778f8cc-8854-4d79-b4c0-73192c883077" />


📌 Simple Way to Remember

- Control Plane → decides
- Worker Nodes → execute
