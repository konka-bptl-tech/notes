### ✅ What is Git?
Git is a distributed version control system where each developer has a full clone of the repository. It keeps track of code changes over time through commits (snapshots), allowing each developer to work independently while also improving collaboration.

In the context of Git and software development, collaboration refers to:

1. Multiple developers contributing to the same project.
2. Sharing code through a remote repository (like GitHub or GitLab).
3. Reviewing, merging, and managing each other's changes without conflicts.
4. Keeping a clear history of who did what and when.
---
### ✅ Why Use Git?
1. **Track changes** – Easily see who changed what and when.
2. **Collaborate** – Multiple developers can work on the same project without overwriting each other's work.
3. **Backup code** – Remote repositories (e.g., GitHub) serve as backups.
4. **Branching and merging** – Safely develop new features or fix bugs in isolation.
5. **Undo mistakes** – Roll back to previous versions if something breaks.
---
### 🔄 Git Areas Explained
1. **Working Directory (Working Area):**
   * This is your actual project folder with all the files you're editing.
   * Changes here are not yet tracked by Git until you **add** them.
2. **Staging Area (Index):**
   * A temporary area where you prepare changes before committing.
   * You decide what changes to include in the next commit.
3. **Local Repository (Local Git):**
   * Where your commits are stored permanently.
   * After committing, changes move from the staging area to the local repository.
---
### 🔧 Key Git Commands to Move Between Areas
| Action                                              | Command                             |
| --------------------------------------------------- | ----------------------------------- |
| Move from **Working Directory → Staging**           | `git add <filename>` or `git add .` |
| Move from **Staging → Working Directory** (unstage) | `git restore --staged <filename>`   |
| Move from **Staging → Local Repository**            | `git commit -m "your message"`      |
---
### 📝 Quick Example
```bash
# Modify a file
echo "new change" >> file.txt
# 1. Stage the file
git add file.txt
# 2. Unstage it (move back to working area)
git restore --staged file.txt
# 3. Stage again and commit it
git add file.txt
git commit -m "Added new change to file.txt"
```
---
### 🐙 What is GitHub?
* **GitHub** is a **cloud-based platform** that hosts **Git repositories**.
* It acts as a **central repository** where all developers can **push**, **pull**, and **collaborate** on code.
* GitHub is a **hosted service** that provides tools on top of Git to simplify team collaboration and software delivery.
---
### 💡 Use Cases of GitHub
1. **Central Code Repository**
   * Developers push and pull code to a shared remote repository.
2. **Code Review**
   * Teams can review each other's changes using **pull requests** and inline comments.
3. **Pull Requests (PRs)**
   * Allow discussion, review, and merging of code changes into the main branch.
4. **GitHub Actions (CI/CD)**
   * Automate workflows like testing, building, and deploying code using GitHub Actions.
5. **Security & Access Control**
   * Offers branch protection, role-based permissions, and vulnerability alerts.
6. **Project Management**
   * Integrates tools like **issues**, **project boards**, and **milestones** for tracking work.
7. **Open Source Collaboration**
   * Hosts millions of open-source projects, allowing global contributions.
---
* **`git log`** – Shows the commit history of the repository.
* **`git diff`** – Displays changes between working directory, staging area, and commits.
* **`git status`** – Shows the current state of files in the working directory and staging area.
* **Set main branch (`git branch -m main`)** – Renames the current branch to `main`.
* **`git init`** – Initializes a new Git repository in the current directory.
* **`git remote add origin <URL>`** – Connects your local repo to a remote repository (e.g., GitHub).
* **`git pull`** – Fetches and merges changes from the remote repository into your current branch.
* **`git push`** – Sends your local commits to the remote repository.
---
# Branches
# Merge,FastForwardMerge,Rebase,Cherry-Pick
# Git Stash
