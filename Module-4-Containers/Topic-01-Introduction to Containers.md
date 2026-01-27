# Module-4: Containers
## Topic-01: Introduction to Containers

## 1. Virtualization & Virtual Machines – A Quick Recap
- Virtualization solved many problems of physical servers.

### Traditional Setup
```bash
Physical Server → Hypervisor → Virtual Machines → Applications
```
- A hypervisor runs on a physical server
- Multiple Virtual Machines (VMs) are created
  - Each VM has:
  - Its own OS
  - Fixed CPU & RAM allocation
  - Installed applications

### The Problem with VMs
VMs allocate resources statically.

#### Example:
- Physical server: 100 GB RAM, 100 CPUs
- 4 VMs created → each gets 25 GB RAM, 25 CPUs
- One VM uses only 10 GB RAM & 6 CPUs
- Result:
  - 15 GB RAM + 19 CPUs wasted

The same problem exists in cloud environments.
- Organizations running thousands or millions of EC2 instances often observe massive resource underutilization, leading to high infrastructure cost.

------------

## 2. Why Containers Were Introduced
- Virtual Machines solved physical server problems.
- Containers solve Virtual Machine inefficiency.

### Key Insight
- VMs are heavy
- Each VM carries a full operating system
- Resources are not shared dynamically

### What Containers Fix
- Enable dynamic resource sharing
- Increase workload density
- Reduce infrastructure cost
- Improve application portability

VMs allocate resources statically, causing large-scale wastage.
Containers allow multiple workloads to share CPU and memory efficiently.

⚠️ Important:
- Containers do not replace VMs.
They run on top of VMs and optimize resource usage.

----------------

## 3. Architecture Shift: VMs vs Containers
Before Containers
```bash
Physical Server → Hypervisor → VM → Application
```

With Containers
```bash
Physical Server → VM → Container Runtime → Containers
```

✔️ Problems solved by VMs → Physical server limitations

✔️ Problems solved by Containers → VM resource wastage

<img width="1045" height="549" alt="VMvsContainersImg" src="https://github.com/user-attachments/assets/44d2728b-ea7e-4422-a4f1-08e66cd38e35" />

----------

## 4. What is a Container?
A container is a lightweight, standardized package that includes:
- Application code
- Required libraries
- System dependencies

It shares the host OS kernel, instead of carrying a full OS.
- A container is a lightweight, portable bundle that packages an application along with its dependencies and runs by sharing the host operating system kernel.

-------------

## 5. Why Containers are Lightweight
- No separate OS per container
- Uses host OS kernel
- Starts in seconds
- Consumes fewer resources

### Container Creation Options
Containers can be created:
  - Directly on physical servers
  - On top of virtual machines (most common in cloud)

----------

## 6. Container Images & Base Images
#### What is a Container Image?
- A read-only template
- Used to create containers
- Similar to an AMI in AWS

#### Base Image
A base image is the starting point for building container images.
It provides:
- Linux user-space filesystem
- Package manager
- Sometimes runtime (Python, Java, Node, etc.)

Defined in a Dockerfile using:
```text
FROM <base-image>
```

📌 Example:
``` Dockerfile
FROM python:3.10
```
--------------

## 7. Docker & Containerization
#### What is Docker?
Docker is a containerization platform that allows you to:
- Build container images
- Package applications
- Ship them across environments
- Run them as containers

##### Docker solves the classic problem: “It works on my machine.”

#### Docker Enables Consistency Across:
- Developer laptops
- Test environments
- Production servers
- Cloud platforms (EC2, ECS, EKS)

-----------

## 8. Docker Architecture
### Core Components
- #### Docker CLI
    Commands like docker build, docker run
- #### Docker Daemon
    Builds images and runs containers
- #### Container Runtime
    Executes containers
- #### Images & Containers

#### Workflow
```text
Dockerfile → Docker Image → Container
```

------------

## 9. Dockerfile Explained
A Dockerfile is a set of instructions to build an image.
Common instructions:
```text 
FROM base-image
WORKDIR /app
COPY requirements.txt .
RUN install dependencies
COPY application code
CMD / ENTRYPOINT
```

#### 📌 Difference:
- Dockerfile → Instructions to build an image
- Base Image → Pre-built image used as the starting point

----------------

## 10. Docker Engine Limitation
Docker Engine uses a central daemon.
#### Issue
- Single Point of Failure
- Security concerns in restricted environments
  
#### Solution
##### - Buildah
- Daemonless image build tool
- Preferred in secure and Kubernetes-native setups

Buildah allows building container images without a running Docker daemon.

--------------

## 11. Containerizing an Application – End-to-End Flow
```text
Developer → Code → Dockerfile → Docker Image → Container
```

#### Step-by-Step
1. Developer writes application code
2. Dockerfile defines:
  - Base image
  - Dependencies
  - Ports
  - Startup command
3. Docker image is built (read-only)
4. Container is created and run from the image

##### 📌 Similar to:
AMI → EC2 Instance in AWS

----------

## 12. Key Takeaways
- Virtual Machines solved physical server problems
- Containers solve VM resource inefficiency
- Containers are lightweight and portable
- Docker standardizes application packaging
- Containers are foundational for Kubernetes & DevOps workflows

------------

## 📚 References
- https://docs.docker.com/get-started/
- https://www.docker.com/play-with-docker/

--------------------------------------
