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
**Merge conflicts** occur when Git can't automatically merge changes between branches because both branches have modifications to the same lines of code or the same file. Addressing merge conflicts is a key skill in collaborative development. Here’s how to address them:

---

### **1. Identify the Conflict**

When a merge conflict occurs, Git will notify you about which files have conflicts and need to be resolved. This usually happens after running the `git merge` or `git pull` command. You'll see something like:

```bash
Auto-merging <file-name>
CONFLICT (content): Merge conflict in <file-name>
```

The conflicting files will be marked with conflict markers in the content, like:

```plaintext
<<<<<<< HEAD
<your changes>
=======
<changes from the branch you're merging>
>>>>>>> <branch-name>
```

---

### **2. Resolve the Conflict Manually**

- Open the conflicting file in a text editor or IDE.
- Look for the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
- Decide which changes to keep:
  - **Keep changes from your branch** (the `HEAD` section).
  - **Keep changes from the merged branch**.
  - **Combine both changes** if necessary.
- After resolving, delete the conflict markers and ensure the file is in a working state.

---

### **3. Stage the Resolved Files**

Once you've manually resolved the conflicts and saved the file(s), stage the changes:

```bash
git add <file-name>
```

If there are multiple files, you can add them all at once:

```bash
git add .
```

---

### **4. Complete the Merge**

Once all conflicts are resolved and staged, complete the merge by committing the changes:

```bash
git commit
```

Git will open the commit message editor, which typically includes a default message indicating that it was a merge commit. You can customize the message if needed or leave it as is.

If you were performing a `git pull` and had a merge conflict, you can finalize it with:

```bash
git pull --continue
```

---

### **5. Verify the Merge**

Check the status of your repository to ensure everything is resolved and ready:

```bash
git status
```

Ensure no unresolved conflicts remain, and everything is staged properly.

---

### **6. Push the Changes (if working with a remote)**

Once the merge is complete and committed, push the changes to the remote repository:

```bash
git push origin <branch-name>
```

---

### **Best Practices to Avoid and Manage Merge Conflicts**

1. **Pull Regularly**: Frequently pull the latest changes from the main branch (or the branch you're merging into) to keep your branch up-to-date and reduce the chances of conflicts.

   ```bash
   git pull origin <main-branch>
   ```

2. **Work in Small, Focused Changes**: Make smaller commits with specific changes. Large commits with many changes are more likely to cause conflicts.

3. **Use Feature Branches**: Isolate different tasks or features into separate branches to reduce the likelihood of conflicts.

4. **Communicate with Your Team**: Make sure that multiple people aren’t working on the same file or lines of code at the same time. Clear communication on who is working on what can help reduce conflicts.

5. **Automate Testing**: Set up a CI pipeline that runs tests on merged branches to ensure no code breaks after resolving conflicts.

---

### **Tools for Resolving Merge Conflicts**

Many IDEs (e.g., VS Code, IntelliJ, or Eclipse) provide visual tools to help you resolve merge conflicts, making it easier to view both sets of changes and choose what to keep.

You can also use Git GUI tools like **GitKraken**, **SourceTree**, or **GitHub Desktop**, which provide a more user-friendly interface for resolving conflicts.

---

### **How to Handle Complex Merge Conflicts**

In cases where conflicts are difficult to resolve manually (e.g., changes span across many lines or files), it’s essential to:

- Take **small steps**: Resolve one conflict at a time, checking the impact of changes carefully.
- **Consult your team**: If unsure about how to resolve the conflict, ask your team for guidance on which changes to keep.

---

By following these steps, you can effectively address merge conflicts and keep your codebase clean and organized.
