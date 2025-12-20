# Module-3: Git
## Topic-5: Git in DevOps and CI/CD

---

## 1. Role of Git in DevOps
Git is the backbone of DevOps practices.  
It acts as the **single source of truth** for application code, infrastructure code, and configuration files.

In DevOps, every change begins with a Git commit.

---

## 2. Git as a Single Source of Truth
Git stores:
- Application source code
- Infrastructure as Code (Terraform, CloudFormation)
- Configuration files (Ansible, Helm, YAML)
- CI/CD pipeline definitions

This ensures:
- Versioning
- Traceability
- Auditability
- Easy rollback

---

## 3. Git and Continuous Integration (CI)

### What is Continuous Integration?
Continuous Integration is the practice of automatically integrating code changes into a shared repository multiple times a day.

### Git’s Role in CI:
- Developers push code to Git
- CI pipeline is triggered automatically
- Code is built and tested
- Feedback is provided quickly

### Common CI Tools Integrated with Git:
- Jenkins
- GitHub Actions
- GitLab CI
- AWS CodeBuild

---

## 4. Git and Continuous Delivery / Deployment (CD)

### Continuous Delivery
- Code is always in a deployable state
- Deployment to production is manual

### Continuous Deployment
- Every successful change is automatically deployed to production

Git triggers CD pipelines after:
- Successful builds
- Passing tests
- Approved pull requests

---

## 5. Typical Git-Based CI/CD Workflow

```text
Code Change → Git Commit → Push to GitHub → 
CI Pipeline → Build → Test → 
CD Pipeline → Deploy
```

---

## 6. Branching Strategy in CI/CD
Common strategies include:
- Feature branches for development
- main or release branch for production
- Short-lived branches for quick integration

CI pipelines usually run on:
- Pull requests
- Push to main branch

---

## 7. Git in Infrastructure as Code (IaC)
Git is used to manage infrastructure code such as:
- Terraform
- AWS CloudFormation
- Ansible playbooks
- Kubernetes manifests

Benefits: 
- Versioned infrastructure
- Rollback capability 
- Change history
- Team collaboration

---

## 8. GitOps Approach
What is GitOps?
GitOps is a DevOps practice where Git becomes the source of truth for system state.

Changes to infrastructure or applications are made by:
- Updating Git repositories
- Automated tools syncing environments with Git

Common GitOps tools:
- Argo CD
- Flux

---

## 9. Security and Access Control in Git
Git supports:
- Role-based access control
- Branch protection rules
- Mandatory pull request reviews
- Commit history auditing
This helps enforce security and compliance.

---

## 10. Best Practices for Using Git in DevOps
- Commit frequently with meaningful messages
- Protect the main branch
- Use pull requests for all changes
- Automate testing and deployments
- Avoid committing secrets
- Maintain clean repository structure

---

## 11. Real-World DevOps Use Case
In real projects:
- Developers push code to Git
- CI pipeline runs tests automatically
- CD pipeline deploys applications
- Infrastructure changes are tracked in Git
- Rollbacks are performed using Git history

---

## 12. Summary
- Git is central to DevOps workflows
- Git enables CI/CD automation
- Git manages both application and infrastructure code
- GitOps makes Git the source of truth
- Mastering Git is essential for DevOps engineers

---
