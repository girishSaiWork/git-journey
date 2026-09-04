# Git & GitHub — Complete Guide

> **Course by Sumit Saha** — Notes extracted from the full tutorial covering Git fundamentals, branching, merging, and GitHub collaboration.

---

## 📌 Git vs GitHub

| Git | GitHub |
|-----|--------|
| A **local** version control tool that runs on your computer | A **cloud-based** platform to host Git repositories |
| Tracks every change: *what* changed, *when*, *who*, and *where* | Central hub where teams push, pull, and collaborate |
| Works offline | Requires internet for remote operations |

> **Analogy:** Git is the coffee ☕ — GitHub is the coffee shop where it's served.

Other remote hosting alternatives: **GitLab**, **Bitbucket**. GitHub (owned by Microsoft) remains the most popular.

---

## 🏗️ Git Architecture — Local & Remote

```
┌─────────────────────── LOCAL ───────────────────────┐      ┌──── REMOTE ────┐
│                                                      │      │                │
│  Working Directory ──► Staging Area ──► Repository   │ ───► │   GitHub Repo  │
│   (your files)        (git add)        (git commit)  │ push │                │
│                                                      │ ◄─── │                │
└──────────────────────────────────────────────────────┘ pull └────────────────┘
```

### The Three Local Phases

1. **Working Directory** — The project folder where you create, edit, and delete files.
2. **Staging Area** — An intermediate checkpoint where you prepare changes before saving. Think of it as standing in front of a mirror before leaving for a party — you review everything before committing.
3. **Local Repository** — The permanent save. A `git commit` locks in your staged changes as a recorded version in your project's history.

> A **repository** is a digital cabinet for your code — it stores every version and the complete change history. Git creates a hidden `.git` folder that tracks everything internally.

---

## ⚙️ Installation & Setup

### Install Git
1. Visit [git-scm.com](https://git-scm.com) → Download for your OS (Windows / macOS / Linux).
2. Windows: choose 32-bit or 64-bit installer. You also get **Git Bash** (a Linux-like terminal).
3. macOS: install via Homebrew (`brew install git`).

### Verify Installation
```bash
git --version
# Shows installed version, e.g., git version 2.42.0
```

### First-Time Configuration (Mandatory)
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
- `--global` → applies to all repos on your machine.
- `--local` → applies only to the current repo.

> Git attaches your identity to every commit for tracking who made which changes.

---

## 🚀 Core Commands

### Initializing a Repository

| Method | Command | When to Use |
|--------|---------|-------------|
| **Local init** | `git init` | Start tracking an existing local folder |
| **Clone remote** | `git clone <url>` | Download an existing GitHub repo to your machine |

```bash
# Local init
mkdir my-project && cd my-project
git init
# Output: Initialized empty Git repository

# Clone from GitHub
git clone https://github.com/user/repo.git
```

Both methods create a hidden `.git` folder — the core of the repository.

---

### Checking Status

```bash
git status
```
Shows which files are **modified**, **untracked** (new), or **staged**. Use it frequently to understand your project's current state.

---

### Staging Changes — `git add`

Staging = telling Git *"these changes are ready for the next commit."*

| Command | Scope |
|---------|-------|
| `git add --all` or `git add -A` | Stage **everything** across the entire project |
| `git add .` | Stage everything in the **current directory** and subdirectories |
| `git add <file>` | Stage a **specific file** (e.g., `git add index.html`) |
| `git add *.txt` | Stage all files matching extension (new/modified only, **not deleted**) |
| `git add *` | Stage new/modified files (**not deleted** ones) |

> ⚠️ **Key difference:** `--all` / `-A` stages everything including deletions. The `*` (star) does **not** stage deleted files.

### Unstaging

```bash
git reset          # Unstage all files (moves them back to working directory)
```

---

### Committing — `git commit`

```bash
git commit -m "Your descriptive commit message"
```
Permanently saves staged changes to the local repository. Each commit gets a **unique ID** (hash) used for version tracking.

### Undo the Last Commit

```bash
git reset HEAD~1   # Undo last commit, return changes to working directory
```

---

### Removing Files — `git rm`

| Command | Effect |
|---------|--------|
| `git rm <file>` | Delete file **and** stage the deletion |
| `git rm -f <file>` | **Force** delete (even if file has uncommitted changes) |
| `git rm --cached <file>` | Remove from staging only, **keep the file** on disk |
| `git rm -r <folder>` | **Recursively** delete a folder and all its contents |

---

### Restoring Files — `git restore`

Used to **undo uncommitted local changes** and revert files to their last committed state.

```bash
git restore <file>           # Restore a specific file
git restore .                # Restore everything in current directory
git restore --staged <file>  # Unstage a file (keeps working directory changes)
git restore --staged .       # Unstage everything
```

> `git restore` is your "undo button" for work you haven't committed yet.

---

### Viewing Commit History — `git log`

```bash
git log                # Full commit history with details
git log --oneline      # Compact one-line-per-commit view (shows short IDs)
```

Press `q` to exit the log viewer.

---

### Comparing Commits — `git diff`

```bash
git diff <newer-commit-id> <older-commit-id>
```
- Shows what was **added** (green) and **removed** (red) between two commits.
- Place the **newer** commit ID first for a forward-looking perspective.

---

## 🌿 Branching

A branch is a **separate line of development**. The default branch is `main` (formerly `master`).

> **Analogy:** The `main` branch is the main kitchen. Feature branches are taste kitchens where you experiment safely before serving.

### Branch Commands

| Command | Action |
|---------|--------|
| `git branch` | List all branches (`*` marks the current one) |
| `git branch <name>` | Create a new branch (inherits current branch's state) |
| `git checkout <name>` | Switch to a branch |
| `git checkout <commit-id>` | Travel to a specific commit (detached HEAD state) |
| `git checkout main` | Return to the latest main branch state |

> When you switch branches, Git **instantly adjusts your file system** to show only that branch's files. No duplication, no conflict.

---

## 🔀 Merging

Merging = **combining changes from two branches into one**.

```bash
# While on the branch you want to merge INTO:
git merge <source-branch>
```

### Example Workflow
```bash
git checkout main
git merge development          # Brings development changes into main
```

### Merge Conflicts

Occur when the **same lines** in the **same file** were changed differently in both branches. Git can't decide which version to keep.

**Resolving a conflict:**
1. Git marks the conflicting section in the file with `<<<<<<<`, `=======`, `>>>>>>>` markers.
2. Manually edit the file — choose which version to keep or combine both.
3. Remove the conflict markers.
4. Stage and commit the resolution:
   ```bash
   git add .
   git commit -m "Resolved merge conflict"
   ```

---

## 🔄 Remote Operations — Push, Fetch, Pull

| Command | Direction | Description |
|---------|-----------|-------------|
| `git push origin <branch>` | Local → Remote | Upload local commits to GitHub |
| `git fetch` | Remote → Local (repo only) | Download remote changes **without** merging |
| `git pull` | Remote → Local (repo + files) | Fetch **and** merge in one step |

> **`git pull` = `git fetch` + `git merge`**

```bash
# Push all branches
git push origin main
git push origin staging
git push origin development

# Pull latest changes
git pull
```

---

## 📦 Git Stash — Temporary Storage

Use when you need to **switch branches** but have **uncommitted work** you're not ready to commit.

```bash
git stash              # Temporarily save uncommitted changes
git stash pop          # Restore latest stash AND remove it from stash list
git stash apply        # Restore latest stash BUT keep it in the stash list
git stash list         # View all stashed items
git stash drop         # Delete the most recent stash entry
```

| `pop` | `apply` |
|-------|---------|
| Restores changes + **removes** from stash list | Restores changes + **keeps** in stash list |

> Think of `pop` as **cut** and `apply` as **copy**.

### Applying a Specific Stash
```bash
git stash pop stash@{0}
git stash apply stash@{1}
```

---

## ⏪ Git Revert vs Git Reset

### `git revert <commit-id>`
- Creates a **new commit** that undoes the changes from a specific commit.
- **History is preserved** — the original commit and the revert commit are both visible.
- ✅ Safe for shared/remote branches.

> **Analogy:** Like adding extra ingredients to fix an over-salted dish instead of throwing it away.

### `git reset`
- Takes you back to a specific commit and **discards all commits after it**.
- **History is erased** — no record of removed commits.
- ⚠️ Use with caution, especially on shared branches.

```bash
git reset --hard       # Reset everything (staged + working directory + deleted files)
```

---

## 🔁 Git Rebase

Rebase = **re-apply your branch's commits on top of another branch**, creating a **clean, linear history**.

```bash
# While on feature branch:
git rebase main
```

### What Happens Behind the Scenes
1. Git finds the common ancestor commit between `feature` and `main`.
2. Temporarily removes your `feature` commits.
3. Applies the new `main` commits into your branch.
4. Re-applies your `feature` commits on top, one by one.

### Merge vs Rebase

| Merge | Rebase |
|-------|--------|
| Creates an extra merge commit | No extra commits — linear history |
| Preserves exact branch history | Rewrites commit history (new IDs) |
| Safe for shared branches | ⚠️ **Avoid on public/shared branches** |

> ⚠️ **Warning:** Rebase rewrites commit IDs. If others are working on the same branch, their local copies won't match. Only rebase on **personal/local branches**.

---

## 🤝 Pull Requests (PRs) on GitHub

A **Pull Request** is a formal request to merge your branch changes into another branch (usually `main`).

### Creating a PR on GitHub
1. Go to the repository → **Pull Requests** tab → **New Pull Request**.
2. Set **Base** branch (where changes go, e.g., `main`) and **Compare** branch (source of changes, e.g., `development`).
3. Review the file diff — see what's added/removed.
4. Click **Create Pull Request** → add a title and description.
5. Team reviews, discusses, and leaves comments.
6. Click **Merge Pull Request** → **Confirm Merge**.

> PRs ensure every change is **reviewed before merging**, keeping the main branch stable and enabling safe team collaboration.

---

## 📋 Quick Reference Cheat Sheet

```
git init                          # Initialize a local repo
git clone <url>                   # Clone a remote repo
git status                        # Check current state
git add .                         # Stage all changes
git commit -m "message"           # Commit staged changes
git log --oneline                 # View compact commit history
git diff <id1> <id2>              # Compare two commits
git branch <name>                 # Create a branch
git checkout <branch>             # Switch branches
git merge <branch>                # Merge a branch into current
git push origin <branch>          # Push to remote
git pull                          # Fetch + merge from remote
git stash                         # Stash uncommitted changes
git stash pop                     # Restore stashed changes
git restore <file>                # Undo uncommitted changes
git revert <commit-id>            # Undo a commit (creates new commit)
git reset --hard                  # Hard reset to last commit
git rebase <branch>               # Rebase current branch onto another
git rm <file>                     # Delete + stage deletion
git config --global user.name ""  # Set global username
git config --global user.email "" # Set global email
```

---

## 🔑 Key Takeaways

1. **Git tracks everything** — every change, every version, every contributor.
2. **Working Directory → Staging → Commit** is the core workflow.
3. **Branches** let you develop features in isolation without breaking `main`.
4. **Merge** combines branches; **Rebase** creates cleaner linear history.
5. **Push/Pull** sync your local work with GitHub.
6. **Stash** saves your work-in-progress when you need to switch context.
7. **Pull Requests** enable code review and safe collaboration.
8. **Revert** is safer than **Reset** for undoing changes on shared branches.
