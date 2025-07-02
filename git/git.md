# Git Version Control

## 1. Introduction to Git

Git is a distributed version control system (DVCS). Each developer has a full local copy of the entire project history, so you don't rely on a single central server. Git tracks changes over time, showing who made each change and allowing you to revert to previous versions if needed.

* **Tracking changes:** Git saves snapshots of your project, so you can compare versions and undo changes if something breaks.
* **Collaboration:** Multiple people can work on the same project concurrently. Git helps merge everyone's changes without conflict.
* **Branching:** You can create branches to develop features or fixes in isolation. Merging branches back keeps the main codebase stable.
* **Experimentation:** Try new ideas safely. If an experimental branch doesn’t work out, you can discard it without affecting the stable code.
* **Industry standard:** Git is the most commonly used version control system in software development. Learning Git is essential for any developer.


## 2. Centralized vs. Distributed Version Control

Version control systems manage project history. **Centralized** systems (like SVN) use a single server for all files and history. Developers check out files, make changes, and check them back in. **Distributed** systems (like Git) give every developer a full repository copy on their machine.

* **Centralized (SVN):** Pros: simpler to understand. Cons: if the server goes down, work halts; slower since many commands need the server.
* **Distributed (Git):** Pros: faster local operations, works offline, very robust (any clone has full history). Cons: more complex initially.

Git's distributed model means you can work and commit offline and then sync when you're online. Your clone even serves as a backup of the whole project.


## 3. Installing and Configuring Git


**Recap**: Git is a distributed version control system that tracks project changes, enables collaborative development through branching/merging, and preserves historical versions for safety.  

## Homework (Rocky Linux):  
1. **Install Git**  
```bash
sudo dnf install git -y
```  

2. **Verify Installation**  
```bash
git --version
```  

3. **Configure Identity**  
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```  

4. **Create Test Repository**  
```bash
mkdir ~/git-lab && cd ~/git-lab
git init
echo "# Git Test Project" > README.md
git add .
git commit -m "Initial commit"
```  

**Email submission**:  
- **Subject**: `HW3 – Git Installation – [Your Name]`  
- **Body**:  
  1. Git version from `git --version`  
  2. Your configured name/email  
  3. Output of `git log --oneline` in ~/git-lab 

## 4. Initializing a Repository and File States  

To start tracking a project with Git, navigate to your project’s folder and run:  

```bash
git init
```  

This creates a hidden `.git` directory, initializing a new Git repository. Git will now track changes to files in this folder.  

Files in Git exist in one of three main states:  

* **Working Directory (Untracked/Modified):**  
  Your active project files. New files are **untracked** by default. Existing tracked files become **modified** when changed but not staged.  
* **Staging Area (Staged):**  
  When you run `git add <file>`, changes move from the working directory to the staging area, preparing them for a commit.  
* **Repository (Committed):**  
  Running `git commit` permanently saves the staged snapshot to the repository.  

![Git File States Diagram](https://github.com/user-attachments/assets/decd4238-37b1-48e1-92a0-e8115daf10c3)  

### Core Concepts Made Simple  
- **Repository:** Project folder tracked by Git  
- **Commit:** Saved snapshot (like a game save point)  
- **Branch:** Parallel development line for experiments  
- **Remote:** Cloud-hosted copy (e.g., GitHub/GitLab)  

### Local vs. Remote Repositories  
- **Local Repository:**  
  Lives on your computer (created by `git init`). This is your private workspace where you make changes and commit snapshots.  
- **Remote Repository:**  
  Hosted on a server (e.g., GitHub/GitLab). Acts as a shared central hub for collaboration.  
  - Sync with `git push` (upload local → remote)  
  - Update with `git pull` (download remote → local)  

Use `git status` to see file states (untracked, modified, or staged). This command is essential for understanding your current workflow status.


## Homework:  
1. Create repository and file:  
```bash  
mkdir ~/rocky-git-lab && cd ~/rocky-git-lab  
git init  
echo "1. Learn Git states" > tasks.txt  
```  

2. Verify file state:  
```bash  
git status  
```  

**Email submission**:  
- **Subject**: `HW4 – Initializing a Repository and File States – [Your Name]`  
- **Body**:  
  a) Full output of `git status`  
  b) Why `tasks.txt` appears as untracked  
  c) Command to transition it to staged state


## 5. Ignoring Files with .gitignore

Some files should not be tracked by Git (for example, compiled binaries, log files, or personal notes). Create a file named `.gitignore` in the root of your project to list patterns for files Git should ignore.

Common patterns:

* `*.log` to ignore all `.log` files.
* `build/` to ignore a directory named `build`.
* `secret.txt` to ignore a specific file.

Add and commit the `.gitignore` file to share it with others. After adding patterns, `git status` will no longer list those ignored files.

## Homework:  
1. **Create files to ignore**:  
```bash  
cd ~/rocky-git-lab  
echo "Personal notes" > notes.txt  
touch cache.tmp output.tmp  
```  

2. **Create .gitignore with Vim**:  
```bash  
vim .gitignore  
```  
*Follow these steps in Vim*:  
```text  
i                  # Enter Insert Mode  
*.tmp<Enter>       # Pattern 1  
notes.txt<Enter>   # Pattern 2  
build/<Esc>        # Pattern 3, then Normal Mode  
:wq<Enter>         # Write and quit  
```  

3. **Verify exclusion**:  
```bash  
git status --ignored  
```  

**Email submission**:  
- **Subject**: `HW5 – .gitignore – [Your Name]`  
- **Body**:  
  a) Output of `cat .gitignore`  
  b) `git status --ignored` output  
  c) Vim commands used to create the file 


## 6. Staging and Committing Changes

To include changes in your next snapshot, you first add them to the staging area with `git add`. For example:

```
git add <filename>
git add .       # stage all changes in current directory
```

After staging, create a commit with a message:

```
git commit -m "Describe the changes"
```

This saves the staged changes in the repository as a new snapshot.

**Commit message tips:**

* Keep it short but descriptive (e.g., "Fix login bug").
* Use present tense (e.g., "Add feature" instead of "Added feature").
* For longer explanations, omit `-m` and write a multi-line message (the first line is a summary).

 
**Recap**: Use `git add` to stage changes and `git commit` to save snapshots. Vim is essential for creating and modifying files before committing.  

## Homework:  
1. **Create file with Vim**:  
```bash  
cd ~/rocky-git-lab  
vim plan.md  
```  
*Vim steps*:  
```text  
i                     # Insert mode  
# Project Plan<Enter>  
1. Research Git workflows<Esc>  
:wq                   # Save and quit  
```  

2. **Stage and commit**:  
```bash  
git add plan.md  
git commit -m "Add initial project plan"  
```  

3. **Verify commit**:  
```bash  
git log --oneline  
```  

**Email submission**:  
- **Subject**: `HW6 – Git Commit – [Your Name]`  
- **Body**:  
  a) `plan.md` content  
  b) `git log --oneline` output  
  c) Vim commands used to create file



## 7. Cloning and Adding Remotes

To get a local copy of an existing repository, use:

```
git clone <repository_url>
```

This creates a new directory with the project name, downloads the entire `.git` directory (all history), and checks out the latest version. It also sets up a remote named `origin` pointing to that URL.

To see your remotes, use:

```
git remote -v
```

This lists names and URLs of remotes (usually `origin`). If you created a new local repo with `git init` and want to connect it to a remote, use:

```
git remote add <name> <repository_url>
```

A common convention is `name = origin`, as in:

```
git remote add origin https://github.com/username/my-project.git
```

  
**Recap**: Use `git clone` to copy remote repositories locally, and `git remote add` to connect local repositories to remote servers.  

## Homework:  
1. **Clone a public repository**:  
```bash  
git clone https://github.com/example/example // explore GitHub and choose something interesting.  
```  

2. **Navigate and verify**:  
```bash  
cd example  
git log --oneline | head -3  
```  

3. **Add remote to your repo**:  
```bash  
cd ~/rocky-git-lab  
git remote add origin https://github.com/yourusername/rocky-git-lab.git  
```  

4. **Verify remotes**:  
```bash  
git remote -v  
```  

**Email submission**:  
- **Subject**: `HW7 – Git Remotes – [Your Name]`  
- **Body**:  
  a) First 3 commits from Linux repository  
  b) `git remote -v` output from your repo  
  c) Purpose of `origin` in Git workflows  


## 8. Synchronizing with Remote (Fetch, Pull, Push)

When collaborating, you often need to sync with a central repository.

* **Fetch:** `git fetch origin` downloads new commits, branches, and tags from `origin` but does **not** merge them into your local branches. It updates your local view of the remote branch (e.g., `origin/main`).
* **Pull:** `git pull origin main` is shorthand for `git fetch` followed by `git merge origin/main`. It fetches changes from the remote and then merges them into your current branch.
* **Push:** `git push origin main` sends your local commits from `main` to the `main` branch on the remote `origin`. If it’s the first time you push a new branch, use `git push -u origin <branch>` to set the upstream.

It's good practice to pull frequently to integrate others' changes and reduce merge conflicts.


**Recap**: Use `git fetch` to download remote changes, `git pull` to fetch + merge changes, and `git push` to upload your commits to a shared repository.  

## Homework:  
1. **Create analog repository**:  
```bash  
mkdir ~/sync-lab && cd ~/sync-lab  
git init  
echo "Version 1" > file.txt  
git add . && git commit -m "Initial commit"  
```  

2. **Simulate remote collaboration**:  
```bash  
# Simulate teammate's change  
mkdir ~/teammate-copy && cp -r ~/sync-lab/. ~/teammate-copy  
cd ~/teammate-copy  
echo "Version 2" > file.txt  
git commit -am "Teammate update"  
```  

3. **Practice sync commands**:  
```bash  
cd ~/sync-lab  
git fetch ~/teammate-copy/.git  # Fetch changes  
git diff HEAD @{u}              # See incoming changes  
git merge                       # Merge changes  
```  

**Email submission**:  
- **Subject**: `HW8 – Sync Simulation – [Your Name]`  
- **Body**:  
  1. `git diff` output before merge  
  2. Final content of `file.txt`  
  3. Your analogy: "Fetch is like ______, while pull is like ______"  


## 9. Branching and Merging

Branches are independent lines of development. Use them to work on new features without affecting the main code:

* **List branches**

  * Local:

    ```bash
    git branch
    ```
  * Remote:

    ```bash
    git branch -r
    ```
  * All:

    ```bash
    git branch -a
    ```

* **Create a branch**

  ```bash
  git branch new-feature
  ```

* **Switch branches**

  ```bash
  git checkout new-feature
  ```

  **Shortcut:**

  ```bash
  git checkout -b new-feature
  ```

  (creates **and** switches)

* **Merge a branch**

  1. Switch to the target branch (e.g., `main`):

     ```bash
     git checkout main
     ```
  2. Merge in your feature branch:

     ```bash
     git merge new-feature
     ```

  * If `main` has no new commits, Git will perform a *fast-forward* merge.
  * If both branches have new commits, Git will create a new **merge commit**.

* **Clean up**
  After merging, you can delete the feature branch if you no longer need it:

  ```bash
  git branch -d new-feature
  ```


## Homework 

```bash
# 1. Create repository
mkdir branching-lab && cd branching-lab
git init
echo "# Branching Experiment" > README.md
git add . && git commit -m "Initial commit"

# 2. Create and work on feature branch
git checkout -b text-formatting
echo "## Formatting Options" >> README.md
echo "1. Bold" >> README.md
git commit -am "Add formatting options"

# 3. Switch to main and make changes
git checkout main
echo "## Text Styles" >> README.md
echo "1. Serif" >> README.md
git commit -am "Add text styles"

# 4. Merge branches
git merge text-formatting

# 5. View branch structure
git log --oneline --graph --all
```

Email submission:  
Subject: HW9 – Branching and Merging – [Your Name]  
Body:  
Output of git branch -a before merging:  
What type of merge occurred?


## 10. Resolving Merge Conflicts

When you merge two branches, Git attempts to auto-combine changes. If both branches modify the same lines, a **merge conflict** occurs. Git will:

1. **Pause the merge**

2. **Insert conflict markers** into the affected file(s):

   ```diff
   <<<<<<< HEAD
   // code from your current branch
   =======
   // code from the branch being merged
   >>>>>>> feature-branch
   ```

3. **Resolve the conflict**:

   * Open the file and decide which code to keep (you can also combine both).
   * Remove the `<<<<<<<`, `=======`, and `>>>>>>>` lines.
   * Save the file.

4. **Finalize the merge**:

   ```bash
   git add <file>
   git commit
   ```

5. **Abort the merge** (if you change your mind before committing):

   ```bash
   git merge --abort
   ```

## Homework 
```bash
# 1. Prepare repository
mkdir conflict-lab && cd conflict-lab
git init
echo "1. Python" > languages.txt
git add . && git commit -m "Initial commit"

# 2. Create conflicting changes
git checkout -b web-languages
echo "2. JavaScript" >> languages.txt
git commit -am "Add web language"

git checkout main
echo "2. Java" >> languages.txt
git commit -am "Add backend language"

# 3. Create merge conflict
git merge web-languages  # Conflict occurs

# 4. Resolve conflict (Vim required)
# Resolve by keeping both entries
git add . && git commit -m "Resolve language conflict"

# 5. Verify resolution
cat languages.txt
```

Email submission:  
Subject: HW10 – Merge Conflicts – [Your Name]  
Body:  
Explain your conflict resolution strategy  


## 11. Deleting Branches

Once a feature branch has been merged, you can safely remove it to keep your repository tidy:

* **Delete locally**

  ```bash
  git branch -d <branch_name>
  ```

  Git will refuse to delete if the branch hasn’t been merged.

* **Force-delete locally** (use with caution):

  ```bash
  git branch -D <branch_name>
  ```

* **Delete on the remote**

  ```bash
  git push origin --delete <branch_name>
  ```

## Homework 



```bash
# 1. Create repository
mkdir branch-cleanup && cd branch-cleanup
git init
echo "# Branch Management" > README.md
git add . && git commit -m "Initial commit"

# 2. Create multiple branches
git checkout -b feature-1
echo "Feature 1" > feature1.txt
git add . && git commit -m "Add feature 1"

git checkout main
git checkout -b feature-2
echo "Feature 2" > feature2.txt
git add . && git commit -m "Add feature 2"

# 3. Merge and delete branches
git checkout main
git merge feature-1
git branch -d feature-1

# 4. Attempt invalid deletion
git branch -d feature-2  # Should fail

# 5. Force delete and clean remote
git branch -D feature-2

# Simulate remote
git remote add origin https://github.com/your-username/branch-cleanup.git
git push -u origin main

# 6. Delete remote branch (simulated)
git push origin --delete feature-2
```

Email submission:  
Subject: HW11 – Deleting Branches – [Your Name]  
Body:  
When would you use git branch -D instead of -d?  


## 12. Git Project


```bash
# Git Project: Climate Research

# 1. Create repository
mkdir climate-research && cd climate-research
git init

# 2. Configure identity
git config --local user.name "Your Name"
git config --local user.email "you@example.com"

# 3. Create README with Vim
vim README.md  # Add "# Global Warming Study" and save
git add README.md
git commit -m "Initial commit"

# 4. Create .gitignore with Vim
vim .gitignore  # Add patterns: *.tmp, *.log, data/, notes.txt
git add .gitignore
git commit -m "Add .gitignore"

# 5. Create data file with Vim
vim climate-data.csv  # Add "1. Historical temperature data"
git status  # Verify untracked file

# 6. Stage and commit changes
git add climate-data.csv
git commit -m "Add initial dataset"

# 7. Create feature branch
git checkout -b data-analysis

# 8. Make changes in branch
vim climate-data.csv  # Add "2. CO2 emission data"
git commit -am "Add emissions data"

# 9. Switch to main and create conflicting change
git checkout main
vim climate-data.csv  # Add "2. Precipitation data"
git commit -am "Add precipitation data"

# 10. Merge branches (will cause conflict)
git merge data-analysis

# Resolve conflict in Vim
vim climate-data.csv  # Keep both entries, remove conflict markers
git add climate-data.csv
git commit -m "Resolve merge conflict"

# 11. Delete feature branch
git branch -d data-analysis

# 12. Add remote
git remote add origin https://github.com/your-username/climate-research.git

# 13. Push to remote
git push -u origin main

# 14. Simulate collaboration
# In another terminal:
#   git clone https://github.com/your-username/climate-research.git climate-collab
#   cd climate-collab
#   vim research-notes.md  # Add content
#   git add research-notes.md
#   git commit -m "Add research notes"
#   git push origin main

# Back in original repo:
git pull origin main

# 15. View project history
git log --oneline --graph
```



**Homework Assignment: Git Porject**

**Email submission:**

- **Subject**: `HW12 – Git Porject – [Your Name]`
- **Body**:
  1. Output of `git log --oneline --graph`:
  2. Content of `climate-data.csv` after merge conflict resolution:
  3. Output of `git remote -v`:
  4. Explain what caused the merge conflict in step 10 and how you resolved it.

