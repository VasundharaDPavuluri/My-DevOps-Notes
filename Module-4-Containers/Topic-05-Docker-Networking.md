# Docker Networking
## Bridge vs Host vs Overlay — and why custom bridge networks improve container security?

Container communication is easy to start… but easy to misconfigure.

---

## Problem

By default, containers run on the **bridge network** and can communicate freely.  
Without proper isolation, this can lead to:

- Unintended container access  
- Port conflicts  
- Hard-to-trace connectivity issues  
- Expanded attack surface  

**Networking misconfiguration is one of the most overlooked container security risks.**

---

## Understanding Docker Network Types

### 🔹 Bridge (default)
- Used on a single host. Containers communicate via internal IPs.  
- Best for standalone apps and local setups.

### 🔹 Host
- Container shares the host network stack.  
- No isolation — highest performance, lowest security.

### 🔹 Overlay
- Used in Docker Swarm / multi-host setups.  
- Enables communication across multiple nodes.

---

## 🛠 Better Approach: Custom Bridge Network

Instead of using the default bridge, create your own network:

```bash
docker network create secure-net

docker run -d --network secure-net --name app1 nginx
docker run -d --network secure-net --name app2 nginx
```
---
### Benefits

- ✔ Automatic DNS-based service discovery  
- ✔ Container isolation from other networks  
- ✔ Controlled communication boundaries  
- ✔ Safer microservice interaction  

**Only containers in the same custom network can talk to each other.**

---

## 📈 Impact

- ✔ Improved container isolation  
- ✔ Reduced attack surface  
- ✔ Cleaner service discovery  
- ✔ Easier microservice communication  
- ✔ Production-friendly network segmentation  

---

## Key Takeaway
Containers should not just run — they should communicate securely.

<img width="1024" height="1328" alt="Docker Network" src="https://github.com/user-attachments/assets/7b3ef9b5-a23e-41d9-b5d5-f4e6f6ba405d" />
