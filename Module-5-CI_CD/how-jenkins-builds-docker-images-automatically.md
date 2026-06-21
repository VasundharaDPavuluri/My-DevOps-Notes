# How Jenkins Builds Docker Images Automatically ?!

## Introduction

Modern software delivery relies on consistency.

One of the biggest challenges in application deployment is ensuring that the same application behaves consistently across different environments.

Docker solved this problem by packaging applications and their dependencies into portable container images.

However, manually building Docker images for every release does not scale.

This is where Jenkins plays a critical role.

Jenkins automates the process of building, tagging, and publishing Docker images, making software delivery faster, more reliable, and repeatable.

---

## Why Manual Docker Builds Do Not Scale

In small environments, developers can manually build and push Docker images.

Typical workflow:

1. Build the application
2. Run Docker build
3. Tag the image
4. Push image to registry
5. Deploy application

Example:

```bash
docker build -t myapp:v1 .
docker push myapp:v1
```

This works initially but introduces several challenges.

### Common Problems

- Manual errors
- Inconsistent image versions
- Missing build steps
- Difficult troubleshooting
- Lack of traceability
- Poor scalability

As applications and teams grow, automation becomes essential.

---

## Automated Docker Image Build Workflow

A modern CI/CD workflow automates the entire process.

```text
Developer Pushes Code
        |
Git Repository
        |
Webhook Trigger
        |
Jenkins Pipeline
        |
Application Build
        |
Docker Image Build
        |
Image Tagging
        |
Push To Registry
        |
Ready For Deployment
```

Every step follows a standardized process.

---

## Step 1: Source Code Commit

A developer pushes code changes to a Git repository.

Example:

```bash
git add .
git commit -m "feature update"
git push origin main
```

This action triggers the CI/CD pipeline.

---

## Step 2: Jenkins Pipeline Starts

GitHub or GitLab sends a webhook notification to Jenkins.

Jenkins automatically starts the pipeline.

Responsibilities include:

- Source code checkout
- Application build
- Test execution
- Docker image creation

No manual intervention is required.

---

## Step 3: Application Build

Before creating a Docker image, Jenkins builds the application.

Examples:

### Java Application

```bash
mvn clean package
```

### Node.js Application

```bash
npm install
npm run build
```

Output:

```text
Application Artifact
├── app.jar
├── app.war
└── build files
```

---

## Step 4: Dockerfile Processing

Jenkins reads the Dockerfile from the repository.

Example:

```dockerfile
FROM openjdk:17-jdk-slim

WORKDIR /app

COPY target/app.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

The Dockerfile acts as a blueprint for creating the container image.

---

## Step 5: Docker Image Creation

Jenkins executes a Docker build command.

Example:

```bash
docker build -t myapp:${BUILD_NUMBER} .
```

Example output:

```text
myapp:101
myapp:102
myapp:103
```

Each build generates a unique image version.

Benefits:

- Traceability
- Version control
- Easy rollback

---

## Step 6: Image Tagging

Tagging identifies a specific image version.

Common tagging approaches:

### Build Number

```text
myapp:101
```

### Git Commit SHA

```text
myapp:a4d8f92
```

### Semantic Versioning

```text
myapp:v1.2.0
```

Best Practice:

Use immutable tags for production deployments.

---

## Step 7: Push Image to Registry

After image creation, Jenkins pushes the image to a registry.

Common registries:

- Docker Hub
- Amazon ECR
- Azure Container Registry
- JFrog Artifactory
- Harbor

Example:

```bash
docker push myapp:101
```

The registry becomes the centralized source for deployment artifacts.

---

## Jenkins Pipeline Example

Example Jenkinsfile:

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/example/app.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push myapp:${BUILD_NUMBER}'
            }
        }
    }
}
```

This pipeline automates the complete image creation process.

---

## Why This Matters

One important principle in modern software delivery is:

> Deployments should not build software.

Deployments should consume a pre-built, tested, versioned artifact.

Docker images provide that artifact.

Benefits include:

- Consistent deployments
- Faster releases
- Easier rollbacks
- Better traceability
- Improved reliability

---

## Architecture Overview

```text
Developer
    |
Git Repository
    |
Webhook
    |
Jenkins Pipeline
    |
Docker Image Build
    |
Container Registry
    |
Deployment Platform
```

Examples of deployment platforms:

- Kubernetes
- Amazon EKS
- ECS
- Docker Swarm
- Virtual Machines

---

## Benefits of Automated Docker Builds

### Consistency

Every image is built using the same process.

### Repeatability

Builds can be reproduced reliably.

### Faster Delivery

Automation reduces manual effort.

### Traceability

Every image version can be tracked.

### Easier Rollbacks

Previous image versions can be redeployed quickly.

### Scalability

Supports growing teams and applications.

---

## Key Insight

Docker images become the delivery artifact.

Jenkins ensures that these artifacts are:

- Built automatically
- Versioned consistently
- Stored securely
- Ready for deployment

This creates a predictable and reliable software delivery process.

---

## Simple Way to Remember

```text
Source Code
      |
Jenkins
      |
Docker Image
      |
Registry
      |
Deployment
```

Build once.

Deploy anywhere.

---

## Summary

Jenkins automates the process of building Docker images from application source code.

By integrating with Git repositories, Docker, and container registries, Jenkins creates versioned and reproducible artifacts that can be deployed consistently across environments.

This automation improves reliability, reduces manual effort, and enables modern CI/CD practices at scale.

<img width="1024" height="1536" alt="How Jenkins Builds Docker Images Automatically" src="https://github.com/user-attachments/assets/7a2da8a5-be39-4986-ac31-ab379e53b5a3" />
