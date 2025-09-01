# Git & GitHub Important Definitions

## Git
Git is a version control system that helps track changes in code.  
It allows developers to save project history, work in branches, and collaborate easily.

## GitHub
GitHub is an online platform where Git repositories are stored and shared.  
It provides tools for collaboration, pull requests, issue tracking, and project management.

## Repository (Repo)
A repository is a storage place where all project files and history are kept.  
It can be local (on your computer) or remote (on GitHub).

## Staging Area
The staging area is like a waiting room where changes are placed before committing.  
It lets you decide which changes will go into the next commit.

## Commit
A commit is a snapshot of your project at a particular time.  
Each commit has a unique ID and message describing the changes.

## Remote
A remote is an online version of your repository stored on platforms like GitHub.  
It allows collaboration and backup of your project.

## Origin
Origin is the default name Git gives to your main remote repository.  
It usually points to the GitHub link of your project.

---

# Useful Git Commands

### Basic Setup
- `git init` → Create a new Git repository  
- `git clone <url>` → Copy a remote repository to your computer  
- `git remote add origin <link>` → Link your local repo to a remote repo  
- `git remote -v` → Show remote repositories  

### Checking Status & History
- `git status` → Check the status of files in the repo  
- `git log` → View commit history  
- `git diff` → Show unstaged changes  
- `git diff <branch>` → Compare current branch with another branch  

### Staging & Committing
- `git add <file>` → Add a file to the staging area  
- `git add .` → Add all changes to the staging area  
- `git commit -m "message"` → Save changes with a message  

### Branching & Merging
- `git branch` → Show available branches  
- `git branch <branch>` → Create a new branch  
- `git checkout <branch>` → Switch to another branch  
- `git checkout -b <branch>` → Create and switch to a new branch  
- `git branch -d <branch>` → Delete a branch  
- `git branch -M main` → Rename current branch to `main`  
- `git merge <branch>` → Merge another branch into current one  

### Working with Remote
- `git push origin main` → Upload changes to GitHub  
- `git pull origin main` → Download latest changes from GitHub  

---

# Git Workflow (Simple Diagram)
- Working Directory → Staging Area → Commit → Remote (GitHub)
