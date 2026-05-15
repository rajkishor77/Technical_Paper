# 🔀 Git & GitHub — Complete Study Notes
> **After reading this, no concept of Git or GitHub will feel unfamiliar.**
> Last Updated: May 2026 | Interview-ready & Production-backed

---

## 📌 Table of Contents
1. [What is Git?](#1-what-is-git)
2. [What is GitHub?](#2-what-is-github)
3. [Git vs GitHub](#3-git-vs-github)
4. [Core Concepts](#4-core-concepts)
5. [Git Architecture & Workflow](#5-git-architecture--workflow)
6. [Git Installation & Configuration](#6-git-installation--configuration)
7. [Repository Setup](#7-repository-setup)
8. [Staging & Committing](#8-staging--committing)
9. [Branching](#9-branching)
10. [Merging & Rebasing](#10-merging--rebasing)
11. [Working with Remotes](#11-working-with-remotes)
12. [Undoing Changes](#12-undoing-changes)
13. [Stashing](#13-stashing)
14. [Tags](#14-tags)
15. [Git Log & History](#15-git-log--history)
16. [Git Diff](#16-git-diff)
17. [Advanced Git](#17-advanced-git)
18. [GitHub Features](#18-github-features)
19. [Git Workflows (Team Strategies)](#19-git-workflows-team-strategies)
20. [.gitignore](#20-gitignore)
21. [Complete Git Commands Reference](#21-complete-git-commands-reference)
22. [Common Interview Questions](#22-common-interview-questions)

---

## 1. What is Git?

Git is a **free, open-source distributed version control system (DVCS)** that tracks changes in files over time and allows multiple developers to collaborate on a project.

### Simple Analogy 📸
> Think of Git as a **Time Machine for your code**.
> - **Working Directory** = Your desk where you actively write and edit files
> - **Staging Area** = A tray on your desk — you place finished work here before filing
> - **Commit** = A permanent photograph of your desk at that exact moment
> - **Repository** = The photo album — all snapshots stored in order
> - **Branch** = A parallel universe — same project, different timeline
> - **Merge** = Combining two parallel universes back into one
> - **Remote (GitHub)** = A backup copy of your photo album stored in the cloud
> - **Clone** = Getting a full copy of someone else's photo album
> - **Pull** = Adding the latest photos from the cloud to your local album
> - **Push** = Sending your latest photos to the cloud

### Key Points
- Created by **Linus Torvalds** in 2005 for managing the Linux kernel
- Written in **C**
- **Distributed** — every developer has a full copy of the entire history
- Works **offline** — you can commit, branch, and view history without internet
- **Non-destructive** — history is never deleted, you can always go back
- Industry standard — used by virtually every software team in the world

---

## 2. What is GitHub?

GitHub is a **cloud-based hosting platform for Git repositories** that adds collaboration tools, project management, and social features on top of Git.

### What GitHub Adds on Top of Git

| Feature | What It Does |
|---------|-------------|
| **Remote hosting** | Store your repo online — accessible from anywhere |
| **Pull Requests (PRs)** | Propose changes, review code, discuss before merging |
| **Issues** | Track bugs, features, and tasks |
| **Actions (CI/CD)** | Automate tests, builds, and deployments |
| **Fork** | Copy someone else's repo to your account |
| **GitHub Pages** | Free static website hosting from a repo |
| **Code Review** | Line-by-line comments on pull requests |
| **Insights** | Contributor stats, commit graphs, traffic |
| **Security Alerts** | Notify you about vulnerable dependencies |

> **Key distinction:** Git is the tool; GitHub is a service that hosts Git repos. Other hosting alternatives: **GitLab**, **Bitbucket**, **Azure DevOps**.

---

## 3. Git vs GitHub

| Feature | **Git** | **GitHub** |
|---------|---------|------------|
| Type | Software (CLI tool) | Web platform / service |
| Works offline | ✅ Yes | ❌ No — needs internet |
| Stores code | On your local machine | On remote servers (cloud) |
| Free | ✅ Always free | Free + paid plans |
| Collaboration | No built-in tools | ✅ PRs, Issues, Actions |
| Installation needed | ✅ Yes | ❌ No — browser-based |
| Created by | Linus Torvalds (2005) | Tom Preston-Werner (2008) |
| Owned by | Open-source community | Microsoft (since 2018) |

---

## 4. Core Concepts

### 4.1 Repository (Repo)
- A folder that Git tracks — contains all your project files plus a hidden `.git/` folder
- The `.git/` folder stores the entire history, branches, and config — **never delete it**
- **Local repo** = on your machine
- **Remote repo** = on GitHub/GitLab

### 4.2 Working Directory
- The actual folder on your machine where you edit files
- Files here are in one of three states:
  - **Untracked** → new file Git has never seen
  - **Modified** → tracked file with unsaved changes
  - **Staged** → modified file moved to the staging area
  - **Committed** → file safely stored in Git history

### 4.3 Staging Area (Index)
- A preparation zone — you choose exactly which changes go into the next commit
- Lets you split work into logical commits even if you changed many files at once
- `git add` moves changes from Working Directory → Staging Area
- `git commit` moves changes from Staging Area → Repository

### 4.4 Commit
- A permanent **snapshot** of your staged changes saved in Git history
- Every commit has:
  - A **unique SHA-1 hash** (e.g., `a3b4c5d`) — its fingerprint
  - A **commit message** describing what changed
  - An **author name and email**
  - A **timestamp**
  - A pointer to its **parent commit** (except the very first commit)
- Commits are **immutable** — you can never change a past commit (only create new ones)

### 4.5 Branch
- A **lightweight movable pointer** to a specific commit
- Creating a branch costs nothing — no files are copied
- `main` (or `master`) = the default branch
- Branches let you work on features or fixes in isolation without affecting `main`
- The special pointer **HEAD** always points to the currently active branch/commit

### 4.6 Merge
- Combining the history of two branches into one
- Git finds the common ancestor and combines changes
- If two branches changed the same line differently → **merge conflict** (you resolve it manually)

### 4.7 Remote
- A version of your repository hosted online (GitHub, GitLab, Bitbucket)
- You interact with it via `push`, `pull`, and `fetch`
- **origin** = the default name given to the remote you cloned from
- You can have multiple remotes: `origin`, `upstream`, `staging`

### 4.8 HEAD
- A special pointer that tells Git "you are currently here"
- Normally points to the latest commit on your current branch
- **Detached HEAD** = HEAD points directly to a commit instead of a branch — happens when you `git checkout <commit-hash>`

---

## 5. Git Architecture & Workflow

### Three-Tree Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         Your Computer                                    │
│                                                                          │
│  ┌──────────────────┐   git add    ┌───────────────────┐                │
│  │                  │ ──────────►  │                   │                │
│  │  Working         │              │   Staging Area    │                │
│  │  Directory       │              │   (Index)         │                │
│  │                  │ ◄──────────  │                   │                │
│  │  (your files     │  git restore │  (changes ready   │                │
│  │   you edit)      │  --staged    │   to be committed)│                │
│  └──────────────────┘              └─────────┬─────────┘                │
│                                              │ git commit                │
│                                              ▼                           │
│                                   ┌──────────────────┐                  │
│                                   │   Local          │                  │
│                                   │   Repository     │                  │
│                                   │   (.git folder)  │                  │
│                                   │                  │                  │
│                                   │  commit history  │                  │
│                                   │  all branches    │                  │
│                                   │  all tags        │                  │
│                                   └──────────────────┘                  │
└──────────────────────────────────────────┬──────────────────────────────┘
                                           │  git push
                                           │  git pull / git fetch
                                           ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                  Remote Repository (GitHub / GitLab)                     │
│                                                                          │
│   All branches, all commits, all tags — shared with the whole team      │
└─────────────────────────────────────────────────────────────────────────┘
```

### File Lifecycle in Git

```
New file created
      ↓
  Untracked  ──── git add ────►  Staged  ──── git commit ────►  Committed
      ↑                              │                               │
      │                   git restore --staged                       │
      │                              │                               │
      │◄─────── git rm --cached ─────┘                               │
      │                                                              │
      │◄─────── edit the file ──────────────────────────────────────►│
      │                                                              │
  Untracked                       Modified (after editing a committed file)
```

---

## 6. Git Installation & Configuration

```bash
# ─── Install Git ──────────────────────────────────────────────────────────
# Ubuntu / Debian
sudo apt update && sudo apt install git

# macOS (using Homebrew)
brew install git

# Windows — download from https://git-scm.com/download/win

# Verify installation
git --version


# ─── First-Time Setup (do this once after installation) ───────────────────
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"    # VS Code as default editor
git config --global core.editor "vim"            # Vim as default editor
git config --global init.defaultBranch main      # use 'main' instead of 'master'
git config --global color.ui auto                # colored output
git config --global pull.rebase false            # merge on pull (not rebase)
git config --global core.autocrlf input          # line ending handling (macOS/Linux)
git config --global core.autocrlf true           # line ending handling (Windows)


# ─── View Configuration ───────────────────────────────────────────────────
git config --list                        # show all config
git config --global --list               # show global config only
git config user.name                     # show a specific value

# Config levels (each overrides the one above it)
# --system  → /etc/gitconfig            — applies to all users on the machine
# --global  → ~/.gitconfig              — applies to all your repos
# --local   → .git/config               — applies to this repo only (default)

# Edit config file directly
git config --global --edit
```

---

## 7. Repository Setup

```bash
# ─── Create a new repository from scratch ─────────────────────────────────
mkdir my-project
cd my-project
git init                             # creates .git/ folder — now tracked by Git
git init my-project                  # create + init in one step (creates the folder too)


# ─── Clone an existing repository ─────────────────────────────────────────
git clone https://github.com/user/repo.git           # clone via HTTPS
git clone git@github.com:user/repo.git               # clone via SSH (no password prompts)
git clone https://github.com/user/repo.git myfolder  # clone into a custom folder name
git clone --depth 1 https://github.com/user/repo.git # shallow clone — only latest commit
                                                      # (faster, less disk space)
git clone --branch develop https://github.com/user/repo.git  # clone specific branch


# ─── Connect local repo to remote ─────────────────────────────────────────
git remote add origin https://github.com/user/repo.git   # add remote named 'origin'
git remote add upstream https://github.com/original/repo.git  # add original repo as upstream (for forks)

# View remotes
git remote -v                        # show all remotes with their URLs
git remote show origin               # detailed info about a specific remote

# Change remote URL (e.g., HTTPS → SSH)
git remote set-url origin git@github.com:user/repo.git

# Rename a remote
git remote rename origin upstream

# Remove a remote
git remote remove origin
```

---

## 8. Staging & Committing

```bash
# ─── Check what's happening ────────────────────────────────────────────────
git status                           # show working tree status (staged, modified, untracked)
git status -s                        # short/compact format
git status --short


# ─── Stage changes ────────────────────────────────────────────────────────
git add filename.py                  # stage a specific file
git add folder/                      # stage an entire folder
git add .                            # stage ALL changes in current directory
git add -A                           # stage ALL changes everywhere (adds, modifies, deletes)
git add *.js                         # stage all .js files
git add -p                           # interactive staging — choose which hunks to stage
git add --patch                      # same as -p — review each change before staging


# ─── Unstage changes (move back from staging to working directory) ─────────
git restore --staged filename.py     # unstage one file (keep changes in working dir)
git restore --staged .               # unstage everything
git reset HEAD filename.py           # older syntax — same result


# ─── Discard changes in working directory (DANGEROUS — cannot undo) ────────
git restore filename.py              # discard changes in one file (goes back to last commit)
git restore .                        # discard ALL working directory changes
git checkout -- filename.py          # older syntax — same result


# ─── Committing ───────────────────────────────────────────────────────────
git commit -m "Your commit message"          # commit with inline message
git commit                                   # opens editor for a longer message
git commit -m "Title" -m "Detailed body"     # commit with title + body

# Stage + commit tracked files in one step (skips staging, doesn't include NEW files)
git commit -am "message"
git commit -a -m "message"

# Amend the LAST commit (change message or add forgotten files)
git add forgotten_file.py
git commit --amend -m "Updated commit message"
# WARNING: Never amend commits that have already been pushed to shared branches


# ─── Commit Message Best Practices ────────────────────────────────────────
# Format:
# <type>(<scope>): <short summary>
#
# Types:
# feat:     A new feature
# fix:      A bug fix
# docs:     Documentation changes
# style:    Formatting, missing semicolons (no logic change)
# refactor: Code change that neither fixes a bug nor adds a feature
# test:     Adding or updating tests
# chore:    Maintenance tasks (dependencies, build config)

# Good examples:
# feat(auth): add JWT authentication
# fix(cart): prevent negative quantity on checkout
# docs(readme): update installation steps
# refactor(models): simplify product query logic
```

---

## 9. Branching

```bash
# ─── View branches ────────────────────────────────────────────────────────
git branch                           # list local branches (* = current)
git branch -a                        # list local + remote tracking branches
git branch -r                        # list remote tracking branches only
git branch -v                        # list branches with last commit info
git branch --merged                  # branches fully merged into current branch
git branch --no-merged               # branches NOT yet merged


# ─── Create branches ──────────────────────────────────────────────────────
git branch feature/login             # create a branch (stays on current branch)
git checkout -b feature/login        # create AND switch to it (old syntax)
git switch -c feature/login          # create AND switch to it (new syntax — preferred)
git switch -c feature/login main     # create branch from main specifically


# ─── Switch branches ──────────────────────────────────────────────────────
git checkout feature/login           # switch to branch (old syntax)
git switch feature/login             # switch to branch (new syntax — preferred)
git switch -                         # switch back to the previous branch
git switch main                      # go back to main


# ─── Rename a branch ──────────────────────────────────────────────────────
git branch -m old-name new-name      # rename any branch
git branch -m new-name               # rename the CURRENT branch
git branch -M main                   # force rename current branch to 'main'


# ─── Delete branches ──────────────────────────────────────────────────────
git branch -d feature/login          # safe delete — only if fully merged
git branch -D feature/login          # force delete — even if NOT merged
git push origin --delete feature/login  # delete a branch on remote
git remote prune origin              # remove local references to deleted remote branches
git fetch --prune                    # fetch + clean up deleted remote branches


# ─── Branch Naming Conventions ────────────────────────────────────────────
# feature/user-authentication        → new features
# fix/cart-total-bug                 → bug fixes
# hotfix/payment-crash               → urgent production fixes
# release/v2.1.0                     → release preparation
# docs/update-readme                 → documentation only
# refactor/clean-up-models           → code improvements
# chore/update-dependencies          → maintenance tasks
```

---

## 10. Merging & Rebasing

### Merging

```bash
# ─── Merge a branch into current branch ───────────────────────────────────
git switch main
git merge feature/login              # merge feature/login INTO main

# Merge strategies:
# Fast-forward merge — linear history, no new commit created (default when possible)
# Three-way merge — creates a new merge commit when histories have diverged

git merge --no-ff feature/login      # force a merge commit even if fast-forward is possible
                                     # better for seeing feature history in the log
git merge --squash feature/login     # squash all feature commits into ONE staged change
                                     # then you commit it manually — clean history


# ─── Resolving Merge Conflicts ────────────────────────────────────────────
# When a conflict happens, Git marks the file like this:
#
# <<<<<<< HEAD (your current branch)
# This is your version of the line
# =======
# This is the incoming branch's version
# >>>>>>> feature/login
#
# Steps to resolve:
# 1. Open the file and edit it — keep what you want, delete the markers
# 2. git add <resolved-file>
# 3. git commit

git merge --abort                    # cancel the merge and go back to before
git mergetool                        # open a visual merge tool (vimdiff, VS Code, etc.)

# Check which files have conflicts
git status                           # shows "both modified" for conflicted files
git diff                             # shows conflict markers in the file
```

### Rebasing

```bash
# ─── What is Rebase? ──────────────────────────────────────────────────────
# Rebase re-applies your commits on top of another branch
# Result: cleaner, linear history (no merge commits)

# Without rebase:
# main:    A -- B -- C
# feature:          └── D -- E
# After merge: A -- B -- C -- M (merge commit)
#                        └── D -- E ┘

# After rebase:
# main:    A -- B -- C
# feature:            └── D' -- E'  (commits replayed on top of C)

git switch feature/login
git rebase main                      # rebase feature onto latest main

# Interactive rebase — rewrite, squash, reorder, edit commit history
git rebase -i HEAD~3                 # interactively edit last 3 commits
git rebase -i main                   # interactive rebase against main

# Interactive rebase options:
# pick   = keep commit as-is
# reword = keep commit but edit the message
# edit   = pause and amend the commit
# squash = combine with previous commit (keep both messages)
# fixup  = combine with previous commit (discard this message)
# drop   = remove this commit entirely

git rebase --abort                   # cancel the rebase
git rebase --continue                # continue after resolving a conflict
git rebase --skip                    # skip current commit and continue

# ⚠️ Golden Rule: NEVER rebase commits that have already been pushed to a shared branch
# It rewrites history and causes problems for everyone else
```

### Merge vs Rebase — When to Use What

| | **Merge** | **Rebase** |
|-|-----------|-----------|
| **History** | Preserves full history + merge commits | Creates a clean, linear history |
| **Safety** | Safe on shared branches | Only safe on LOCAL / private branches |
| **Traceability** | Easy to see when branches merged | Harder to trace original branch point |
| **Use when** | Merging feature to main (PRs) | Keeping feature branch up-to-date with main |
| **Golden Rule** | Always safe | Never on pushed/shared commits |

---

## 11. Working with Remotes

```bash
# ─── Fetch (download without merging) ─────────────────────────────────────
git fetch origin                     # download all changes from origin (does NOT merge)
git fetch origin main                # fetch a specific branch
git fetch --all                      # fetch from all remotes
git fetch --prune                    # fetch + delete references to deleted remote branches

# After fetch, remote changes are in origin/main — merge manually:
git merge origin/main                # merge fetched changes into current branch


# ─── Pull (fetch + merge in one step) ─────────────────────────────────────
git pull                             # fetch + merge from tracked remote branch
git pull origin main                 # pull from specific remote and branch
git pull --rebase                    # fetch + rebase instead of merge (cleaner history)
git pull --rebase origin main


# ─── Push (upload local commits to remote) ────────────────────────────────
git push origin main                 # push main branch to origin
git push origin feature/login        # push a feature branch
git push                             # push current branch to its tracked remote
git push -u origin feature/login     # push + set upstream tracking (first push of new branch)
git push --force-with-lease          # safer force push — fails if remote has new commits
git push --force                     # force push — DANGEROUS, overwrites remote history
git push origin --delete feature/login  # delete a remote branch
git push --tags                      # push all local tags to remote
git push origin v1.0.0               # push a specific tag


# ─── Fetch vs Pull ────────────────────────────────────────────────────────
# git fetch → downloads changes, lets YOU decide when to merge
# git pull  → downloads + immediately merges (convenience shortcut)
# Best practice: git fetch first, review changes, then merge


# ─── Tracking branches ────────────────────────────────────────────────────
# When you clone a repo, origin/main is the remote tracking branch
# git push -u origin main sets up tracking so you can just type git push/pull

git branch --set-upstream-to=origin/main main   # set tracking manually
git branch -vv                                  # see tracking info for all branches
```

---

## 12. Undoing Changes

```bash
# ─── Restore (safest — does NOT touch history) ────────────────────────────
git restore filename.py              # discard working directory changes
git restore .                        # discard all working directory changes
git restore --staged filename.py     # unstage a file (keep changes in working dir)
git restore --staged .               # unstage everything
git restore --source HEAD~1 filename.py  # restore file to how it was 1 commit ago


# ─── Reset (moves HEAD backwards) ─────────────────────────────────────────
# --soft → moves HEAD back, keeps all changes STAGED
git reset --soft HEAD~1              # undo last commit, keep changes staged

# --mixed (DEFAULT) → moves HEAD back, keeps changes in WORKING DIRECTORY (unstaged)
git reset HEAD~1                     # undo last commit, keep changes unstaged
git reset --mixed HEAD~1             # same thing

# --hard → moves HEAD back and DISCARDS all changes (DANGEROUS — cannot undo)
git reset --hard HEAD~1              # undo last commit and DELETE those changes forever
git reset --hard origin/main         # reset local branch to match remote (discard local changes)

# Reset to a specific commit
git reset --soft abc1234             # go back to commit abc1234, keep changes staged
git reset --hard abc1234             # go back to commit abc1234, delete everything after it

# ⚠️ Never use git reset on shared/pushed commits — it rewrites history


# ─── Revert (SAFE — creates a new commit that undoes a past commit) ────────
git revert HEAD                      # create a new commit that undoes the last commit
git revert abc1234                   # revert a specific commit by hash
git revert HEAD~3..HEAD              # revert the last 3 commits (creates 3 revert commits)
git revert --no-commit HEAD          # stage the revert but don't commit yet

# ✅ git revert is SAFE to use on shared/pushed branches — it doesn't rewrite history


# ─── Reset vs Revert ──────────────────────────────────────────────────────
# git reset  → deletes or moves commits — rewrites history — use only on LOCAL work
# git revert → adds a new "undo" commit — history intact — safe on shared branches


# ─── Remove untracked files ────────────────────────────────────────────────
git clean -n                         # dry run — show what would be deleted
git clean -f                         # delete untracked files
git clean -fd                        # delete untracked files AND directories
git clean -fX                        # delete only ignored files (listed in .gitignore)
git clean -fdx                       # delete untracked files, dirs, AND ignored files
```

---

## 13. Stashing

Stash temporarily saves your working directory changes without committing them — useful when you need to switch branches but aren't ready to commit.

```bash
# ─── Save to stash ────────────────────────────────────────────────────────
git stash                            # stash tracked modified and staged files
git stash push -m "work in progress on login"   # stash with a descriptive label
git stash -u                         # stash including UNTRACKED files
git stash --include-untracked        # same as -u
git stash -a                         # stash everything including IGNORED files
git stash push filename.py           # stash only a specific file


# ─── List stashes ─────────────────────────────────────────────────────────
git stash list
# Output: stash@{0}: WIP on feature/login: abc1234 work in progress on login
#         stash@{1}: WIP on main: def5678 some other work


# ─── Apply stash ──────────────────────────────────────────────────────────
git stash apply                      # apply most recent stash (keeps it in the list)
git stash apply stash@{2}            # apply a specific stash by index
git stash pop                        # apply most recent stash AND remove it from list
git stash pop stash@{1}              # pop a specific stash


# ─── Inspect stash ────────────────────────────────────────────────────────
git stash show                       # summary of most recent stash
git stash show -p                    # full diff of most recent stash
git stash show stash@{1} -p          # full diff of a specific stash


# ─── Branch from stash ────────────────────────────────────────────────────
git stash branch feature/new-branch  # create a new branch and apply the stash onto it


# ─── Remove stashes ───────────────────────────────────────────────────────
git stash drop                       # remove the most recent stash
git stash drop stash@{1}             # remove a specific stash
git stash clear                      # remove ALL stashes (DANGEROUS — cannot undo)
```

---

## 14. Tags

Tags mark specific commits as important — typically used to mark release versions.

```bash
# ─── Lightweight tag (just a name pointing to a commit) ───────────────────
git tag v1.0.0                       # tag the current commit
git tag v1.0.0 abc1234               # tag a specific past commit


# ─── Annotated tag (recommended for releases — stores metadata) ────────────
git tag -a v1.0.0 -m "Version 1.0.0 — First stable release"
git tag -a v1.0.0 abc1234 -m "Tag past commit"


# ─── View tags ────────────────────────────────────────────────────────────
git tag                              # list all tags
git tag -l "v1.*"                    # filter tags matching pattern
git show v1.0.0                      # show tag details + the tagged commit


# ─── Push tags to remote ──────────────────────────────────────────────────
git push origin v1.0.0               # push a single tag
git push origin --tags               # push ALL tags
git push --follow-tags               # push commits + their annotated tags


# ─── Delete tags ──────────────────────────────────────────────────────────
git tag -d v1.0.0                    # delete local tag
git push origin --delete v1.0.0      # delete tag from remote
git push origin :refs/tags/v1.0.0    # older syntax — same result


# ─── Checkout a tag (detached HEAD state) ─────────────────────────────────
git checkout v1.0.0                  # view code at that tag (read-only — detached HEAD)
git switch -c hotfix/v1.0.1 v1.0.0  # create a branch from a tag to make fixes
```

---

## 15. Git Log & History

```bash
# ─── Basic log ────────────────────────────────────────────────────────────
git log                              # full commit history (author, date, message)
git log --oneline                    # one line per commit (hash + message)
git log --oneline --graph            # ASCII branch graph
git log --oneline --graph --all      # graph for ALL branches
git log --oneline --graph --all --decorate   # show branch and tag names

# Custom pretty format
git log --pretty=format:"%h - %an, %ar : %s"
# %h = short hash, %an = author name, %ar = relative date, %s = subject


# ─── Filter log ───────────────────────────────────────────────────────────
git log -5                           # last 5 commits only
git log --since="2024-01-01"         # commits after a date
git log --until="2024-12-31"         # commits before a date
git log --since="2 weeks ago"        # relative date
git log --author="Alice"             # commits by a specific author
git log --grep="fix"                 # commits with "fix" in the message
git log --all --grep="login"         # search across all branches
git log -- filename.py               # commits that touched a specific file
git log -S "def transfer_money"      # commits that added/removed this string (pickaxe search)
git log -G "password"                # commits whose diff matches this regex
git log main..feature/login          # commits in feature/login not yet in main
git log feature/login..main          # commits in main not yet in feature/login
git log --merges                     # only merge commits
git log --no-merges                  # exclude merge commits


# ─── Show a specific commit ───────────────────────────────────────────────
git show abc1234                     # show commit details + diff
git show HEAD                        # show latest commit
git show HEAD~2                      # show 2 commits before HEAD
git show HEAD:filename.py            # show file content at HEAD
git show v1.0.0:src/app.py           # show file content at a tag


# ─── Blame — who wrote each line ─────────────────────────────────────────
git blame filename.py                # show author + commit hash for each line
git blame -L 10,30 filename.py       # blame only lines 10 to 30
git blame --since="1 year ago" filename.py  # blame with age filter


# ─── Reflog — Git's safety net ────────────────────────────────────────────
git reflog                           # shows ALL HEAD movements — even after reset
# This is how you recover "lost" commits after a bad git reset --hard
# Find the hash, then: git reset --hard <that-hash>
git reflog show feature/login        # reflog for a specific branch
```

---

## 16. Git Diff

```bash
# ─── What has changed? ────────────────────────────────────────────────────
git diff                             # unstaged changes (working dir vs staging area)
git diff --staged                    # staged changes (staging area vs last commit)
git diff HEAD                        # all changes (working dir + staged vs last commit)
git diff filename.py                 # diff for a specific file only


# ─── Compare branches and commits ────────────────────────────────────────
git diff main feature/login          # difference between two branches
git diff main..feature/login         # same thing
git diff abc1234 def5678             # difference between two commits
git diff HEAD~3 HEAD                 # compare 3 commits ago to now
git diff HEAD~1                      # compare last commit to current state


# ─── Useful diff options ──────────────────────────────────────────────────
git diff --stat                      # summary only (files changed, insertions, deletions)
git diff --name-only                 # just the names of changed files
git diff --name-status               # file names + status (A=added, M=modified, D=deleted)
git diff -w                          # ignore whitespace changes
git diff --word-diff                 # show diff at word level (not line level)
```

---

## 17. Advanced Git

### Cherry-Pick — Apply a Specific Commit from Another Branch

```bash
# Apply one commit from another branch to your current branch
git cherry-pick abc1234

# Apply multiple commits
git cherry-pick abc1234 def5678

# Cherry-pick a range
git cherry-pick abc1234^..def5678

# Cherry-pick without committing (apply as staged changes)
git cherry-pick --no-commit abc1234

git cherry-pick --abort              # abort if conflict
git cherry-pick --continue           # continue after resolving conflict
```

### Bisect — Find Which Commit Introduced a Bug

```bash
git bisect start                     # start the bisect session
git bisect bad                       # current commit has the bug
git bisect good v1.0.0               # this commit was known to be good
# Git checks out the midpoint commit — you test and tell Git if it's good or bad
git bisect good                      # this commit is fine
git bisect bad                       # this commit has the bug
# Git narrows it down — keep going until it says "abc1234 is the first bad commit"
git bisect reset                     # end bisect session, return to original branch
```

### Submodules — A Repo Inside a Repo

```bash
git submodule add https://github.com/user/lib.git libs/mylib  # add submodule
git submodule init                   # initialize submodules after cloning
git submodule update                 # pull submodule contents
git submodule update --init --recursive  # init + update all nested submodules
git clone --recurse-submodules <url>  # clone with all submodules
```

### Git Worktree — Multiple Working Directories

```bash
# Check out two branches simultaneously in different folders
git worktree add ../hotfix hotfix/payment-crash
git worktree list
git worktree remove ../hotfix
```

### Useful Shortcuts

```bash
# View the full object database
git cat-file -t abc1234              # type of object (commit, tree, blob, tag)
git cat-file -p abc1234              # contents of an object

# Find the commit that deleted a file
git log --all --full-history -- deleted_file.py

# Recover a deleted file
git checkout HEAD~1 -- deleted_file.py    # restore from 1 commit before deletion

# Show all files Git is tracking
git ls-files

# Count commits
git rev-list --count HEAD
git shortlog -sn                     # commit count per author

# Check if branch is up to date with remote
git fetch origin
git status                           # will say "Your branch is behind origin/main by N commits"
```

---

## 18. GitHub Features

### Pull Request (PR) Workflow

```
1. Fork or clone the repo
2. Create a feature branch:   git switch -c feature/new-feature
3. Make changes and commit:   git commit -m "feat: add new feature"
4. Push branch to GitHub:     git push -u origin feature/new-feature
5. Open a Pull Request on GitHub (base: main ← compare: feature/new-feature)
6. Team reviews, requests changes, approves
7. Merge the PR (merge commit, squash, or rebase)
8. Delete the feature branch
9. Pull the updated main:     git pull origin main
```

### GitHub CLI (`gh`) — GitHub from the Terminal

```bash
# Install: https://cli.github.com/

gh auth login                        # authenticate with GitHub
gh repo create myapp --public        # create a new GitHub repo
gh repo clone user/repo              # clone a repo
gh pr create --title "feat: login" --body "Adds JWT login"  # open a PR
gh pr list                           # list open PRs
gh pr checkout 42                    # check out PR #42 locally to review
gh pr merge 42 --squash              # merge PR #42
gh issue create --title "Bug: login fails" --body "Steps to reproduce..."
gh issue list                        # list open issues
gh issue close 15                    # close issue #15
gh release create v1.0.0 --title "Version 1.0.0" --notes "First release"
gh workflow run deploy.yml           # manually trigger a GitHub Action
gh repo fork user/repo --clone       # fork + clone in one command
```

### SSH Key Setup (Recommended — No password prompts)

```bash
# Generate an SSH key
ssh-keygen -t ed25519 -C "you@example.com"

# Start the SSH agent
eval "$(ssh-agent -s)"

# Add your key to the agent
ssh-add ~/.ssh/id_ed25519

# Copy your public key to clipboard
cat ~/.ssh/id_ed25519.pub
# Then go to GitHub → Settings → SSH Keys → New SSH Key → paste it

# Test the connection
ssh -T git@github.com
# "Hi username! You've successfully authenticated."
```

---

## 19. Git Workflows (Team Strategies)

### 19.1 GitHub Flow (Simple — Recommended for most teams)

```
main branch is always deployable
     ↓
Create feature branch from main
     ↓
Make commits on the feature branch
     ↓
Open a Pull Request
     ↓
Review + CI passes
     ↓
Merge into main
     ↓
Deploy immediately
```

**Best for:** Continuous deployment, small teams, web apps

### 19.2 Git Flow (Structured — For versioned releases)

```
main      → production code only — tagged with versions
develop   → integration branch — all features merged here
feature/* → branch from develop, merge back to develop
release/* → branch from develop, merge to main + develop
hotfix/*  → branch from main (urgent fixes), merge to main + develop
```

**Best for:** Mobile apps, libraries, versioned software with scheduled releases

### 19.3 Trunk-Based Development (Fast — For large teams with CI/CD)

```
All developers commit small changes directly to main (or very short-lived branches)
Feature flags hide incomplete features from users
Automated tests run on every commit
Deploy to production multiple times per day
```

**Best for:** Large engineering teams, Google/Facebook-style continuous deployment

---

## 20. .gitignore

```bash
# Create a .gitignore in the root of your project
touch .gitignore
```

```gitignore
# ─── Python ───────────────────────────────────────────────────────────────
__pycache__/
*.py[cod]
*.pyo
*.pyd
.Python
env/
venv/
.venv/
*.egg-info/
dist/
build/
.pytest_cache/

# ─── Django ───────────────────────────────────────────────────────────────
*.log
local_settings.py
db.sqlite3
media/
staticfiles/

# ─── Node.js ──────────────────────────────────────────────────────────────
node_modules/
npm-debug.log
yarn-error.log
dist/
.next/

# ─── Environment & Secrets ────────────────────────────────────────────────
.env
.env.local
.env.production
.env.*.local
*.pem
*.key
secrets/

# ─── IDEs & Editors ───────────────────────────────────────────────────────
.vscode/
.idea/
*.swp
*.swo
*~
.DS_Store          # macOS
Thumbs.db          # Windows

# ─── Docker ───────────────────────────────────────────────────────────────
.dockerignore

# ─── Pattern examples ─────────────────────────────────────────────────────
# *.log         → ignore all .log files anywhere
# logs/         → ignore the logs directory
# !debug.log    → BUT track debug.log even though *.log is ignored
# /TODO         → ignore TODO only in root, not subdirectories
# doc/**/*.pdf  → ignore all .pdf files in doc/ and subdirectories
```

```bash
# Add global gitignore (applies to all repos on your machine)
git config --global core.excludesfile ~/.gitignore_global

# Check what is being ignored
git check-ignore -v filename.py      # is this file being ignored? and why?
git status --ignored                 # show ignored files in git status

# If you already committed a file you want to ignore:
git rm --cached filename.py          # remove from tracking (file stays on disk)
git rm --cached -r node_modules/     # remove entire folder from tracking
git commit -m "chore: untrack node_modules"
# Now add it to .gitignore
```

---

## 21. Complete Git Commands Reference

### Setup & Config

```bash
git --version                        # check Git version
git config --global user.name "Name"
git config --global user.email "email"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --list
git config --global --edit           # open global config in editor
git help <command>                   # help for a command
git <command> --help                 # same thing
man git-<command>                    # manual page
```

### Repository

```bash
git init                             # initialize new repo
git init <folder>                    # init in a specific folder
git clone <url>                      # clone a remote repo
git clone <url> <folder>             # clone into a specific folder name
git clone --depth 1 <url>            # shallow clone (latest commit only)
git clone --branch <branch> <url>    # clone a specific branch
```

### Status & Info

```bash
git status                           # working tree status
git status -s                        # short format
git log                              # commit history
git log --oneline                    # compact history
git log --oneline --graph --all      # visual branch graph
git log --author="Name"              # filter by author
git log --since="1 week ago"         # filter by date
git log -- <file>                    # commits touching a file
git show <commit>                    # show a commit's details
git blame <file>                     # who wrote each line
git diff                             # unstaged changes
git diff --staged                    # staged changes
git diff <branch1> <branch2>         # compare branches
git shortlog -sn                     # commit count per author
git reflog                           # full HEAD movement history
```

### Staging & Committing

```bash
git add <file>                       # stage a file
git add .                            # stage all changes
git add -p                           # interactive staging
git restore --staged <file>          # unstage
git restore <file>                   # discard working dir changes
git commit -m "message"              # commit
git commit -am "message"             # stage tracked + commit
git commit --amend -m "new message"  # fix last commit
git rm <file>                        # delete file + stage deletion
git rm --cached <file>               # stop tracking (keep file on disk)
git mv <old> <new>                   # rename/move a file
```

### Branching

```bash
git branch                           # list branches
git branch -a                        # list all (local + remote)
git branch -v                        # list with last commit
git branch <name>                    # create branch
git switch <name>                    # switch to branch
git switch -c <name>                 # create + switch
git switch -                         # switch to previous branch
git branch -d <name>                 # delete (safe)
git branch -D <name>                 # force delete
git branch -m <old> <new>            # rename branch
git branch --merged                  # merged branches
git branch --no-merged               # unmerged branches
```

### Merging & Rebasing

```bash
git merge <branch>                   # merge branch into current
git merge --no-ff <branch>           # force merge commit
git merge --squash <branch>          # squash to one commit
git merge --abort                    # cancel merge
git rebase <branch>                  # rebase onto branch
git rebase -i HEAD~<n>               # interactive rebase (last n commits)
git rebase --abort                   # cancel rebase
git rebase --continue                # continue after resolving conflict
git cherry-pick <commit>             # apply a specific commit
```

### Remote

```bash
git remote -v                        # list remotes
git remote add origin <url>          # add remote
git remote set-url origin <url>      # change remote URL
git remote remove <name>             # remove remote
git fetch origin                     # download (no merge)
git fetch --all                      # fetch all remotes
git fetch --prune                    # fetch + clean dead branches
git pull                             # fetch + merge
git pull --rebase                    # fetch + rebase
git push origin <branch>             # push a branch
git push -u origin <branch>          # push + set upstream
git push --force-with-lease          # safe force push
git push origin --delete <branch>    # delete remote branch
git push --tags                      # push all tags
```

### Undoing

```bash
git restore <file>                   # discard working dir changes
git restore --staged <file>          # unstage
git reset HEAD~1                     # undo last commit (keep changes)
git reset --soft HEAD~1              # undo, keep staged
git reset --hard HEAD~1              # undo + DELETE changes (DANGEROUS)
git reset --hard origin/main         # reset to match remote
git revert <commit>                  # safe undo — creates new commit
git clean -n                         # dry run: what would be deleted
git clean -fd                        # delete untracked files + dirs
```

### Stash

```bash
git stash                            # stash changes
git stash push -m "label"            # stash with label
git stash -u                         # stash including untracked
git stash list                       # list stashes
git stash apply                      # apply most recent stash
git stash pop                        # apply + remove stash
git stash drop                       # remove most recent stash
git stash clear                      # remove ALL stashes
```

### Tags

```bash
git tag                              # list tags
git tag <name>                       # create lightweight tag
git tag -a <name> -m "msg"           # create annotated tag
git show <tag>                       # show tag details
git push origin <tag>                # push one tag
git push --tags                      # push all tags
git tag -d <name>                    # delete local tag
git push origin --delete <name>      # delete remote tag
```

### Advanced

```bash
git bisect start / good / bad        # binary search for a bug
git cherry-pick <commit>             # apply specific commit
git rebase -i HEAD~n                 # interactive rebase
git worktree add <path> <branch>     # multiple working trees
git submodule add <url>              # add submodule
git ls-files                         # list tracked files
git check-ignore -v <file>           # check if file is ignored
git rev-list --count HEAD            # count total commits
git archive --format=zip HEAD > src.zip  # export repo as zip
```

---

## 22. Common Interview Questions

### Q1: What is Git and why do we need it?
**Answer:** Git is a distributed version control system that tracks changes in files over time. Without it, developers would have to manually save copies of files (project_v1, project_v2_final, project_FINAL_FINAL...) and collaboration would be nearly impossible — two people editing the same file would overwrite each other's work. Git solves this by maintaining a complete history of every change, allowing multiple people to work on the same codebase simultaneously on separate branches, and merging their work together safely. It also makes it possible to revert any change, understand who changed what and why, and maintain stable production code while developing new features in parallel.

---

### Q2: What is the difference between `git fetch` and `git pull`?
**Answer:** Both download changes from a remote repository, but they behave differently afterward. `git fetch` only downloads the changes and stores them in remote tracking branches (like `origin/main`) — it does NOT touch your working directory or current branch. You get the changes but your code is unchanged; you can review them first with `git diff origin/main` then merge when ready. `git pull` is essentially `git fetch` followed immediately by `git merge` — it downloads and merges in one step. Best practice is to use `git fetch` first, review the changes, then merge manually to avoid surprises.

---

### Q3: What is the difference between `git merge` and `git rebase`?
**Answer:** Both integrate changes from one branch into another, but they create different histories. Merge creates a new "merge commit" that joins the two branch histories — it preserves the full context of when and where branches diverged and merged. Rebase takes your commits and replays them on top of another branch — it creates a clean, linear history as if the branch was always created from the latest point. Use merge for integrating a feature into main (it's safe on shared branches and preserves history). Use rebase to keep your feature branch up-to-date with main while you work, or to clean up messy local commits before a PR. The golden rule: never rebase commits that have already been pushed to a shared branch.

---

### Q4: What is the difference between `git reset` and `git revert`?
**Answer:** Both undo changes but in completely different ways. `git reset` moves the branch pointer backward to a previous commit — it rewrites history by removing commits. With `--soft` it keeps the changes staged, with `--mixed` it keeps them in the working directory, and with `--hard` it deletes them permanently. `git revert` creates a brand-new commit that is the inverse of a past commit — it undoes the change by adding a new commit, leaving all history intact. The key distinction: use `git reset` only on your LOCAL, unpushed commits. Use `git revert` when you need to undo a commit that has already been pushed to a shared branch — it's safe because it doesn't rewrite history.

---

### Q5: What is the staging area and why does it exist?
**Answer:** The staging area (also called the index) is an intermediate layer between your working directory and the repository. When you edit files, the changes go to the working directory. When you run `git add`, selected changes move to the staging area. When you run `git commit`, only what's in the staging area is committed. It exists to give you fine-grained control over what goes into each commit. For example, you might have changed three files but only want to commit one logical change — you can stage only those two related files and commit them with a meaningful message, then stage and commit the third file separately. This helps create clean, focused commit history.

---

### Q6: What is HEAD in Git?
**Answer:** HEAD is a special pointer that represents "where you currently are" in the repository. Normally it points to the tip of your current branch — when you commit, the branch pointer moves forward and HEAD moves with it. When you switch branches with `git switch`, HEAD moves to point to the new branch. A "detached HEAD" state occurs when HEAD points directly to a specific commit hash instead of a branch — this happens when you `git checkout <commit-hash>`. In detached HEAD state, any new commits you make won't belong to any branch and can be lost when you switch away, unless you create a new branch from that point.

---

### Q7: What is the difference between `git stash` and `git commit`?
**Answer:** Both save your current work, but for different purposes. `git commit` permanently saves a snapshot of your staged changes to the repository history — it's a formal record of what changed, why, and by whom. `git stash` is a temporary holding area — it saves your uncommitted changes (both staged and unstaged) and reverts your working directory to a clean state, so you can quickly switch branches or pull changes. Stashed work is not part of the commit history. Use stash when you need to briefly set aside unfinished work without polluting the history with an incomplete commit, and pop the stash when you're ready to continue.

---

### Q8: How do you resolve a merge conflict?
**Answer:** A merge conflict happens when two branches changed the same part of the same file differently. Git marks the conflicting section in the file with `<<<<<<<`, `=======`, and `>>>>>>>` markers. To resolve: first run `git status` to see which files conflict. Open each conflicted file, find the markers, and manually edit the file to keep the correct version (could be one side, the other, or a combination of both). Delete the conflict markers. Then run `git add <resolved-file>` to mark it resolved, and finally `git commit` to complete the merge. If things go wrong during the process, `git merge --abort` cancels the entire merge and returns to the state before you started.

---

### Q9: What is a Pull Request (PR) and why is it important?
**Answer:** A Pull Request is a GitHub feature (not a Git feature) where you propose merging your branch into another branch. It serves as a formal review process — before code reaches the main branch, the PR notifies teammates, shows exactly what changed with a diff, allows inline code comments on specific lines, runs automated checks (tests, linters), and requires approval from reviewers. PRs are central to team collaboration because they enforce code review, document the reasoning behind changes (the PR description), prevent bugs from entering the main codebase, and create an audit trail of what was merged and why. Even on small teams, PRs improve code quality significantly.

---

### Q10: What is the difference between `git clone`, `git fork`, and `git init`?
**Answer:** `git init` creates a brand-new, empty Git repository in your current folder — there's no remote, no history, nothing. `git clone` copies an existing repository from a URL to your local machine — you get the full history, all branches, and a remote named `origin` already configured. `git fork` is a GitHub concept (not a Git command) — it creates a personal copy of someone else's GitHub repository under your own GitHub account. The fork exists on GitHub, not on your machine. You then clone your fork to work locally. Forks are used for contributing to open-source projects where you don't have push access to the original repo — you make changes in your fork and open a Pull Request to the original.

---

## 🔄 Git — At a Glance

```
Working Directory → git add → Staging Area → git commit → Local Repo
                                                                ↓
                                                           git push
                                                                ↓
                                                        Remote (GitHub)
                                                                ↓
                                                           git pull / fetch
                                                                ↓
                                                        Local Repo → Working Directory
```

---

## 📝 Quick Revision Card

| Concept / Command | In One Line |
|-------------------|-------------|
| Git | Distributed version control system — tracks every change |
| GitHub | Cloud hosting for Git repos + collaboration tools |
| `git init` | Create a new empty Git repository |
| `git clone` | Copy a remote repo to your machine (with full history) |
| Working Directory | Where you actively edit files |
| Staging Area | Preparation zone — choose what goes into next commit |
| `git add .` | Stage ALL changes for the next commit |
| `git add -p` | Interactively choose which changes to stage |
| `git commit -m` | Save staged changes permanently to history |
| `git commit --amend` | Fix the last commit (message or files) |
| HEAD | Pointer to your current position in the repo |
| Branch | A lightweight pointer to a commit — isolated work zone |
| `git switch -c` | Create and switch to a new branch |
| `git merge` | Combine another branch into the current one |
| `git rebase` | Replay commits on top of another branch (clean history) |
| `git fetch` | Download remote changes (don't merge yet) |
| `git pull` | Fetch + merge in one step |
| `git push` | Upload local commits to remote |
| `git push -u` | Push + set upstream tracking for the branch |
| `git status` | See what's changed, staged, untracked |
| `git log --oneline` | Compact commit history |
| `git diff` | See unstaged changes |
| `git diff --staged` | See staged (about-to-commit) changes |
| `git stash` | Temporarily shelve uncommitted work |
| `git stash pop` | Restore shelved work |
| `git reset --soft` | Undo last commit, keep changes staged |
| `git reset --hard` | Undo last commit AND delete changes (DANGEROUS) |
| `git revert` | Safely undo a commit by creating a new one (history safe) |
| `git cherry-pick` | Apply one specific commit from another branch |
| `git bisect` | Binary search to find which commit introduced a bug |
| `git blame` | See who wrote every line of a file |
| `git reflog` | Full history of HEAD movements — Git's safety net |
| `git tag -a` | Mark a release version with an annotated tag |
| `.gitignore` | List files Git should never track |
| `git rm --cached` | Stop tracking a file without deleting it |
| Fork | GitHub copy of someone else's repo under your account |
| Pull Request | Propose + review changes before merging |
| `git remote -v` | See connected remote repositories |
| `git clean -fd` | Delete all untracked files and directories |

---

> 💡 **The Production Golden Rule:**
> Always follow — **Feature branch for every change + Descriptive commit messages (feat/fix/docs) + Never force push to main + Always use `git revert` on shared branches (never `git reset`) + Pull Request + code review before merge + Tag every release + `.gitignore` secrets from day one**.
> These 7 habits make you a reliable, professional engineer on any team.

---

*Notes prepared based on: Pro Git Book (Scott Chacon), GitHub Official Docs, Atlassian Git Tutorials, and production best practices (2025–2026)*
