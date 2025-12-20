# Module-3: Git
## Topic-3: Branching and Merging in Git

---

## 1. What is a Branch in Git?
A branch in Git represents an independent line of development.  
It allows developers to work on new features, bug fixes, or experiments without affecting the main codebase.

By default, Git creates a branch called `main`.

---

## 2. Why Branching is Important
Branching enables:
- Parallel development
- Safe experimentation
- Isolated feature development
- Easier collaboration within teams

Without branches, all changes would directly affect the main code.

---

## 3. Types of Branches

### 3.1 Main (or Master) Branch
- Primary branch of the repository
- Contains stable and production-ready code

### 3.2 Feature Branch
- Used to develop new features
- Typically merged into the main branch after completion

**Example:**
```bash
git checkout -b feature-login
```

3.3 Bugfix Branch
- Created to fix bugs
- Usually short-lived

Example:
```bash
git checkout -b bugfix-auth-error
```

3.4 Release Branch
- Used to prepare code for production release
- Allows final testing and minor fixes

4. Working with Branches
List all branches
```bash
git branch
```

Create a new branch
```bash
git branch <branch-name>
```

Switch to a branch
```bash
git checkout <branch-name>
```

Create and switch to a branch
```bash
git checkout -b <branch-name>
```

5. What is Merging?
Merging is the process of integrating changes from one branch into another.
Most commonly, feature branches are merged into the main branch after development and testing.

6. Types of Merge

- 6.1 Fast-Forward Merge
  - Occurs when the main branch has no new commits
  - Git simply moves the branch pointer forward

- 6.2 Three-Way Merge
  - Occurs when both branches have diverged
  - Git creates a new merge commit

7. Performing a Merge
Step-by-step merge process
```bash
git checkout main
git pull origin main
git merge feature-login
```

8. Merge Conflicts
What is a Merge Conflict?
A merge conflict occurs when Git cannot automatically resolve differences between branches.
This usually happens when:
- Same line of code is modified in both branches
- A file is deleted in one branch but modified in another

- Resolving Merge Conflicts
- 1. Identify conflicted files
- 2. Open the files and resolve conflicts manually
- 3. Add resolved files
```bash
git add <file-name>
```
4. Commit the merge
```bash
git commit
```

9. Deleting Branches
- Delete a local branch
```bash
git branch -d <branch-name>
```

- Delete a remote branch
```bash
git push origin --delete <branch-name>
```

10. Best Practices for Branching
- Keep the main branch stable
- Use meaningful branch names
- Delete branches after merging
- Regularly sync with the main branch
- Avoid long-lived feature branches

11. Branching Strategy Overview
Common branching strategies include:
- Git Flow
- GitHub Flow
- Trunk-Based Development
Choosing the right strategy depends on team size and release process.

12. Summary
- Branches allow parallel development
- Merging combines changes from branches
- Conflicts are normal and manageable
- Branching is essential for DevOps collaboration
