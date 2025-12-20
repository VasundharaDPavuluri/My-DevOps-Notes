# Module-3: Git
## Topic-4: GitHub Workflow

---

## 1. What is GitHub?
GitHub is a cloud-based platform that hosts Git repositories and enables collaboration, code review, and automation.

It provides:
- Centralized code hosting
- Collaboration tools
- Pull requests & code reviews
- Integration with CI/CD pipelines

---

## 2. Why GitHub is Important in DevOps
GitHub plays a critical role in DevOps by:
- Acting as a single source of truth
- Enabling collaboration between teams
- Triggering CI/CD pipelines
- Managing Infrastructure as Code (IaC)
- Supporting automation and integrations

---

## 3. What is a GitHub Workflow?
A GitHub workflow defines **how code changes move from development to production** using Git and GitHub.

A typical workflow includes:
- Creating branches
- Making commits
- Opening pull requests
- Reviewing code
- Merging changes

---

## 4. Common GitHub Workflow Models

### 4.1 Centralized Workflow
- All developers work on a single branch
- Suitable for small teams
- Minimal branching

---

### 4.2 Feature Branch Workflow
- Each feature is developed in a separate branch
- Changes are merged via pull requests
- Most commonly used workflow

---

### 4.3 Forking Workflow
- Developers fork the repository
- Changes are pushed to personal forks
- Pull requests are created to upstream repo
- Common in open-source projects

---

## 5. Feature Branch Workflow (Step-by-Step)

### Step 1: Clone the Repository
```bash
git clone <repository-url>
cd <repository-name>
```

### Step 2: Create a Feature Branch
```bash
git checkout -b feature-new-change
```

### Step 3: Make Changes and Commit
```bash
git status
git add .
git commit -m "Add new feature"
```

Step 4: Push Branch to GitHub
```bash
git push origin feature-new-change
```

Step 5: Create a Pull Request (PR)
- Open GitHub repository
- Click Compare & pull request
- Add title and description
- Request reviews if needed

Step 6: Code Review
- Reviewers examine changes
- Comments and suggestions are added
- Changes may be requested before approval

Step 7: Merge Pull Request
- Merge into `main` branch
- Delete feature branch after merge
---
## 6. Pull Requests (PR)
What is a Pull Request?
A Pull Request is a request to merge changes from one branch into another.
PRs help in:
- Code review
- Quality checks
- Team collaboration
- Avoiding broken code in main branch
---

## 7. Code Review Best Practices
- Keep PRs small and focused
- Write clear commit messages
- Add meaningful PR descriptions
- Review code carefully before merging

---

## 8. GitHub Issues
GitHub Issues are used to:
- Track bugs
- Plan features
- Manage tasks
- Document discussions
Issues can be linked to Pull Requests for better tracking.

---

## 9. GitHub Actions (Workflow Automation)
GitHub Actions allow automation of workflows such as:
- CI/CD pipelines
- Testing
- Code quality checks
- Deployment automation

Example triggers:
- On push
- On pull request
- On merge

---

## 10. Protecting the Main Branch
Branch protection rules help:
- Prevent direct pushes to main
- Enforce PR reviews
- Require status checks before merging
This ensures code quality and stability.

---

## 11. GitHub Workflow in DevOps CI/CD
Typical CI/CD flow:
```bash
Code Commit → GitHub → CI Pipeline → Tests → Build → Deploy
```

GitHub integrates seamlessly with:
- Jenkins
- GitHub Actions
- AWS CodePipeline
- GitLab CI

---

## 12. Best Practices for GitHub Workflow
- Use feature branches
- Never commit directly to main
- Enforce code reviews
- Automate testing
- Keep repository clean and documented

---

## 13. Summary
- GitHub enables collaboration and automation
- Pull requests are core to GitHub workflow
- Feature branch workflow is most common
- GitHub is central to DevOps CI/CD pipelines

---
