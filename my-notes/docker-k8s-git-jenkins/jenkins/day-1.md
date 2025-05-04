# What is Git?
Git is a **distributed version control system** that tracks changes in source code during software development. It allows developers to save snapshots of their code through **commits**, making it easy to manage and revisit previous versions of a project.
- **Improves collaboration** between developers by allowing multiple people to work on the same codebase simultaneously.
- **Enables independent development**, so developers can work on separate features or fixes without conflicting with each other's code.
- **Supports branching and merging**, making it easy to experiment with new ideas safely and integrate them later.
---
# Git: Working Directory, Staging Area, and Local Repository
- **Working Directory**:  
  This is where you actively edit and make changes to your code files.
- **Staging Area (Index)**:  
  Before committing, you add changes to the staging area. It acts as a preparation zone where you select what changes will go into your next commit.
- **Local Repository (Local Git)**:  
  After staging, when you commit, Git saves the snapshot of your changes into the local repository. It's a complete history of your project stored on your machine.
---
# Git Basic Commands
- **`git add .`** — Stages all the changes in the current directory.
- **`git add <file-name>`** — Stages a specific file.
- **`git status`** — Shows the current state of the working directory and staging area (what's changed, what's staged, etc.).
- **`git commit -m "your message"`** — Commits the staged changes with a descriptive message.
- **`git log`** — Displays the history of commits in the repository.
---
# Git SSH Authentication
- **Generate an SSH key**:  
  Use `ssh-keygen` to create a new SSH key pair on your local machine.
- **Add the public key to GitHub**:  
  Copy the contents of your public key (usually `~/.ssh/id_rsa.pub`) and add it to your GitHub account under **Settings → SSH and GPG keys**.
- **Test the connection**:  
  Run `ssh -T git@github.com` to verify that your machine can authenticate with GitHub over SSH.
---
# Push Code to GitHub Central Repository
- **Create a repository on GitHub**:  
  Go to GitHub and create a new empty repository.
- **Connect local repo to GitHub**:  
  Use `git remote add origin <repository-URL>` to link your local repository to GitHub.
- **Push code to GitHub**:  
  Use `git push -u origin main` (or `master`, depending on your branch name) to upload your local commits to GitHub.
- **Pull updates from GitHub**:  
  Use `git pull origin main` to fetch and merge any changes from the remote repository into your local branch.
---
# GitHub
- GitHub is a **centralized managed repository system** where developers can store, share, and collaborate on code projects.
- It offers multiple features:
  - **CI/CD Integration** — Automate testing and deployments directly from your repository.
  - **Code Review Mechanism** — Collaborators can review and comment on code changes before merging.
  - **Issue Tracking** — Open and manage issues to track bugs, tasks, or feature requests.
  - **Pull Requests** — Propose, discuss, and merge changes between branches in a controlled way.
---
# Difference Between Git and GitHub
| Git | GitHub |
|:---|:---|
| Git is a **distributed version control system** used to track changes in code. | GitHub is a **cloud-based platform** that hosts Git repositories and provides collaboration tools. |
| It works **locally** on your computer. | It works **online**, allowing you to store and share code with others. |
| Git manages **snapshots** of your project history. | GitHub adds features like **issue tracking, pull requests, CI/CD integration, and team collaboration**. |
| No internet connection is needed for basic Git operations. | Requires internet access to push, pull, or collaborate remotely.
---
# What is a Branch in Git?
A branch in Git is a separate line of development that allows you to work on different features, bug fixes, or experiments independently from the main codebase. It helps keep different changes isolated until you're ready to integrate them back into the main project.
---
# Git Branch Commands
- **Create a new branch**:  
  `git branch <branch-name>`
- **Switch to a branch** (check out the branch):  
  `git checkout <branch-name>`  
  Or, in newer versions, you can use:  
  `git switch <branch-name>`
- **Delete a branch** (locally):  
  `git branch -d <branch-name>`  
  Use `-D` to force delete if the branch hasn't been merged yet:  
  `git branch -D <branch-name>`
---
# Git: Merge, Fast-Forward Merge, Rebase, and Cherry-Pick
### 1. **Merge**
- **What it does**: Combines the changes from two branches into one. If there are changes on both branches, Git will try to merge them. If there are conflicts (e.g., changes to the same line of code), you'll need to fix them manually.
  - **Command**:  
    ```bash
    git merge <branch-name>
    ```
### 2. **Fast-Forward Merge**
- **What it does**: Happens when there are no changes on the current branch since the last time it was updated. Git just moves the current branch pointer to the latest commit of the other branch. It’s a quick and simple merge without any extra merge commit.
  - **Command**:  
    Git does this automatically if no conflicts are found when you run `git merge`.
### 3. **Rebase**
- **What it does**: Rebase takes the changes from one branch and "reapplies" them on top of another branch. It helps make the history of changes look clean and straight without extra merge commits.
  - **Command**:  
    ```bash
    git rebase <branch-name>
    ```
### 4. **Cherry-Pick**
- **What it does**: Cherry-pick lets you pick a specific commit (a change) from one branch and apply it to another branch. It's like picking only the changes you want, without merging everything.
  - **Command**:  
    ```bash
    git cherry-pick <commit-hash>
    ```
### How can you resolve conflicts
"When Developer1 and Developer2 make changes to the same file and the same line of code, Git is unable to automatically merge the changes and raises a conflict. At that point, we bring both developers into the loop to discuss and decide which changes to keep and which to discard. Once we agree on the final version, we manually edit the file to remove the conflict markers, add the resolved file using git add, and then push the changes after committing."
---
# Git Stash
**What it does**:  
`git stash` temporarily saves your uncommitted changes (both staged and unstaged) so you can work on something else. This is helpful when you're in the middle of work but need to switch to another task without committing your incomplete changes.
---
### Key Git Stash Commands
1. **Stash Changes**  
   Save your current changes to the stash (without committing them) and revert your working directory to the last commit.
   ```bash
   git stash
   ```
2. **List Stashed Changes**  
   View all the stashed changes you've saved.
   ```bash
   git stash list
   ```
3. **Apply Stash**  
   Apply the most recent stashed changes to your working directory.
   ```bash
   git stash apply
   ```
4. **Apply Specific Stash**  
   Apply a specific stash from the list (replace `<stash@{n}>` with the stash reference).
   ```bash
   git stash apply stash@{n}
   ```
5. **Drop Stash**  
   Delete a specific stash (replace `<stash@{n}>` with the stash reference).
   ```bash
   git stash drop stash@{n}
   ```
6. **Pop Stash**  
   Apply the most recent stash and then remove it from the stash list.
   ```bash
   git stash pop
   ```
7. **Clear All Stashes**  
   Remove all stashes.
   ```bash
   git stash clear
   ```
---
