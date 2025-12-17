# Module-3: Git
## Topic-2: Git Basics

---

## 1. What is Git?
Git is a **Distributed Version Control System (DVCS)** used to track changes in source code during software development.

It allows developers to:
- Track changes efficiently
- Collaborate with teams
- Work offline
- Restore previous versions when needed

---

## 2. Git Architecture
Git works using multiple areas that represent the flow of changes:

Working Directory → Staging Area → Local Repository → Remote Repository

### Explanation:
- **Working Directory** – Where files are created or modified
- **Staging Area** – Where changes are prepared before committing
- **Local Repository** – Stores committed changes locally
- **Remote Repository** – Shared repository hosted on platforms like GitHub

---

## 3. Installing Git

### On Linux
```bash
sudo apt update
sudo apt install git -y
```

### On Windows
Download and install Git from:
https://git-scm.com/

---

## 4. Initial Git Configuration
Before using Git, user identity must be configured.
```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```
Verify configuration:
```bash
git config --list
```
---
## 5. Creating a Git Repository
Initialize a new repository
```bash
git init
```
Clone an existing repository
```bash
git clone <repository-url>
```
---

## 6. Basic Git Commands
Check repository status
```bash
git status
```
Add files to staging area
```bash
git add <file-name>
git add .
```
Commit changes
```bash
git commit -m "meaningful commit message"
```
View commit history
```bash
git log
```
or
```bash
git log --oneline
```
View file differences
```bash
git diff
```
---

## 7. Working with Remote Repositories
Add remote repository 
```bash
git remote add origin <repository-url>
```

View remote repositories
```bash
git remote -v
```
Push changes to remote
```bash
git push origin main
```
Pull changes from remote
```bash
git pull origin main
```
---

## 8. Branch Basics
List branches
```bash
git branch
```
Create a new branch
```bash
git branch <branch-name>
```
Switch branches
```bash
git checkout <branch-name>
```
Create and switch to a new branch
```bash
git checkout -b <branch-name>
```
---

## 9. Undoing Changes
Discard working directory changes
```bash
git restore <file-name>
```
Unstage files
```bash
git restore --staged <file-name>
```
Revert a commit
```bash
git revert <commit-id>
```
---

## 10. Git Ignore
.gitignore is used to exclude files from being tracked by Git.
**Example** .gitignore:
*.log
node_modules/
.env 

---

## 11. Common Git Workflow
Edit file → git add → git commit → git push

This workflow is commonly used in DevOps CI/CD pipelines.

---

## 12. Git Best Practices
- Write meaningful commit messages
- Commit small and logical changes
- Pull latest changes before pushing
- Never commit sensitive information
- Use branches for feature development

---

13. Summary
- Git is a distributed version control system
- Git enables collaboration and version tracking
- Git works both offline and online
- Git is a core skill for DevOps engineers

---
