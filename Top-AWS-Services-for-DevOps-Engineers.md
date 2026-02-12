# Top AWS Services for DevOps Engineers

AWS plays a major role in modern DevOps practices by providing managed infrastructure and platform services that reduce operational overhead.

---

## 1. AWS Service Model Overview

AWS primarily operates at:
- **Infrastructure as a Service (IaaS)**
- **Platform as a Service (PaaS)**

It also provides limited **SaaS offerings**, but that is not its main focus.

### Why this matters for DevOps
AWS helps teams:
- Reduce infrastructure toil
- Automate deployments
- Improve scalability and reliability
- Focus more on code and delivery

---

## 2. AWS as Infrastructure as a Service (IaaS)

AWS started strong in the IaaS layer.

### Key Characteristics
- You manage OS, applications, and security configurations
- AWS manages hardware, networking infrastructure, and data centers

### Core IaaS Services
- **Amazon EC2** – Virtual machines
- **Amazon VPC** – Private and secure networking
- **Amazon EBS** – Block storage for EC2

---

## 3. AWS as Platform as a Service (PaaS)

This is AWS’s **biggest advantage today**.

### Key Characteristics
- No server management
- Built-in scaling and availability
- Faster development and deployment

### PaaS Examples
- **AWS Lambda**
- **Amazon RDS**
- **AWS Elastic Beanstalk**

---

## 4. AWS as Software as a Service (SaaS)

AWS provides some SaaS offerings, but this is not its primary model.

### Examples
- **Amazon QuickSight**
- **Amazon WorkMail**

---

## 5. Core AWS Services for DevOps Engineers

### 5.1 Compute & Networking
- **Amazon EC2** – Compute instances
- **Amazon VPC** – Networking, CIDR, security groups, routing

---

### 5.2 Storage
- **Amazon EBS** – Persistent block storage
- **Amazon S3** – Object storage for artifacts, backups, logs

---

### 5.3 Identity & Security
- **AWS IAM** – Identity and access management
- **AWS KMS** – Encryption key management

---

### 5.4 Monitoring & Observability
- **Amazon CloudWatch**
  - Metrics, logs, alarms
  - Can trigger Lambda & SNS for alerts

**Example:**  
If an unencrypted EBS volume is created →  CloudWatch detects it →  Lambda triggers →  SNS sends alert email.

---

### 5.5 Serverless Compute
- **AWS Lambda**
  - Run code without managing servers
  - Event-driven execution
  - Pay only for execution time

#### Lambda vs EC2
- **Lambda:** Stateless, event-driven, minimal ops
- **EC2:** Long-running apps, full control

---

## 6. CI/CD & Cloud Build Services

AWS provides fully managed CI/CD services.

### Problems They Solve
**Before:**
- Managing Jenkins servers
- Patching OS
- Handling plugin failures
- Scaling build agents

**Now:**
- Fully managed
- Auto-scaling
- Pay-per-use
- Built-in security & logs

---

### 6.1 AWS CodeBuild
- Builds source code
- Runs tests
- Produces artifacts
- Uses `buildspec.yml`

---

### 6.2 AWS CodePipeline
- Orchestrates CI/CD workflow
- Source → Build → Test → Deploy
- Integrates with GitHub, CodeCommit, CodeBuild

---

### 6.3 AWS CodeDeploy
- Deploys applications to:
  - EC2
  - ECS
  - Lambda
- Supports blue/green & rolling deployments

---

### CI/CD Flow on AWS

Source (GitHub / CodeCommit)

↓

CodeBuild

↓

Artifacts (S3)

↓

CodeDeploy / ECS / Lambda

---

## 7. Governance, Security & Cost Management

- **AWS CloudTrail**
  - Tracks every API call
  - Enables governance, auditing, and compliance

- **AWS Config**
  - Tracks configuration changes
  - Ensures compliance and drift detection

- **AWS Billing & Cost Explorer**
  - Cost visibility and optimization

---

## 8. Containers & Orchestration

- **Amazon ECS** – AWS native container orchestration
- **AWS Fargate** – Serverless containers
- **Amazon EKS** – Managed Kubernetes service

---

## 9. Logging & Analysis
- **ELK Stack (Elasticsearch, Logstash, Kibana)**
  - Centralized logging
  - Search and visualization

---

## 10. Summary

AWS provides a rich ecosystem of services that:
- Reduce infrastructure complexity
- Enable automation
- Improve reliability
- Support modern DevOps practices

For DevOps engineers, understanding **how these services fit together** is more important than memorizing individual services.

--- 
